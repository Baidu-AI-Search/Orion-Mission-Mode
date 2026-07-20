# WebAssembly Outside the Browser: A 2026 Snapshot of Server-Side, Edge, and Plugin Adoption

**Date:** July 2026
**Topic:** Current state of WebAssembly (Wasm) adoption outside the browser — server-side, edge computing, and plugin systems.

---

## Executive Summary

WebAssembly (Wasm) has matured far beyond its original browser sandbox. Driven by the Bytecode Alliance's standardization of the WebAssembly System Interface (WASI), the Component Model, and the Wasmtime/Wasmer/WasmEdge runtime ecosystem, Wasm is now deployed in production at Cloudflare, Fastly, Shopify, Docker/containerd, Envoy/Istio service meshes, and across cloud-native edge platforms. As of mid-2026, WASI 0.2 ("Preview 2") is stable, Preview 3 (with native `async`/`future`/`stream`) is the active development preview, and Wasm 3.0 features (GC, memory64, exception handling, multi-memory, tail calls) have shipped in all four major browsers and are propagating to server runtimes. This report surveys major adopters, compares the leading runtimes, documents the WASI standardization roadmap, lists five concrete production use cases, and enumerates the technical limitations still blocking broader adoption.

---

## 1. Current Adoption: Major Platforms and Companies

Wasm is no longer a browser-only technology. A diverse set of major platforms and companies now depend on Wasm outside the browser:

- **Cloudflare Workers** runs user code on V8 isolates, with first-class WebAssembly support for Rust, C/C++, AssemblyScript, and via compatibility layers many other languages. Cloudflare open-sourced the underlying runtime as [`workerd`](https://github.com/cloudflare/workerd), describing it as "Cloudflare's JavaScript/Wasm Runtime … based on the same code that powers Cloudflare Workers." The runtime is used as "an application server, a development tool, and a programmable HTTP proxy." [1] Workers deploys globally across Cloudflare's 330+ cities with the claim of "no cold starts" by virtue of isolate-based execution rather than containers. [2]
- **Fastly Compute** (formerly Compute@Edge) was one of the earliest production edge platforms built explicitly around Wasm. Fastly originally developed the **Lucet** ahead-of-time Wasm compiler and later migrated to Wasmtime, donating both projects to the Bytecode Alliance. Local development is provided by **Viceroy**, an open-source local runtime for Compute apps. [3]
- **Shopify Functions** lets merchants and third-party developers customize Shopify's backend (discounts, shipping, checkout validation, delivery) using Wasm modules uploaded through the platform. Shopify documents Functions as compiled-to-Wasm code that runs inline with Shopify's request pipeline at sub-millisecond latency. [4]
- **Docker** shipped a Wasm technical preview in late 2022 and has since integrated Wasm as a first-class containerd runtime via the `containerd-shim-wasm` family of shims, enabling `docker run --runtime=io.containerd.wasmtime.v1` style invocations. [5][6]
- **Envoy Proxy / Istio** support Wasm extensions through the **Proxy-Wasm** ABI (Application Binary Interface) specification, with SDKs in Rust, Go, C++, and AssemblyScript, and host implementations in Envoy, Istio, NGINX, ATS, and MOSN. The Wasm HTTP filter is part of the official Envoy configuration reference. [7][8]
- **Fermyon Spin** (now maintained under the Spin Framework community) is an open-source framework for "building and running fast, secure, and composable cloud microservices with WebAssembly," built on Wasmtime and the Component Model, with first-class SDKs for Rust, JavaScript/TypeScript, Go, and Python; Fermyon Cloud and **SpinKube** (Kubernetes operator) provide managed and self-hosted deployment paths. [9][10]
- **WasmEdge** (a CNCF sandbox project, formerly Second State / SSVMR) is positioned as "the easiest and fastest way to run LLMs on your own devices" via the LlamaEdge application framework, and is used for edge microservices, serverless SaaS APIs, embedded functions, smart contracts, and AI inference through WASI-NN. [11]
- **Extism** (from Dylibso) is a lightweight cross-language framework explicitly built for running Wasm as a plugin system in host applications, with SDKs for 15+ host languages including Rust, JavaScript/Node/Deno/Bun, Python, Go, Java/.NET, Elixir, PHP, Ruby, Zig, OCaml, Haskell, Perl, C and C++. [12]
- **Microsoft** uses Wasm for **Blazor WebAssembly** in .NET (and ships native-AOT compilation via `dotnet publish -r wasm -p:PublishAot=true`), and the Azure side has explored Wasm for sandboxed extension points. [13]
- **Blockchain and edge-AI** platforms — including DFINITY Internet Computer (canister smart contracts run on Wasm), Polkadot/Substrate clients, YoMo data-streaming framework (documented in WasmEdge's integrations list), and LlamaEdge/mediapipe-style on-device AI — round out the non-browser ecosystem. [11]

---

## 2. Key Runtimes: Wasmtime, Wasmer, WasmEdge, and Others

The server-side Wasm market has coalesced around three major general-purpose runtimes, with several specialized options filling niches.

| Runtime | Maintainer / Sponsor | Primary Focus | Maturity / Licensing | Distinguishing Traits |
|---|---|---|---|---|
| **Wasmtime** | Bytecode Alliance (Fastly, Mozilla, Intel, Red Hat, Microsoft, etc.) — Rust-implemented, Apache 2.0 with LLVM exception [14] | General-purpose, standards-first embeddable runtime; reference implementation of WASI and the Component Model | Production-grade. Active CI fuzzing via Google OSS-Fuzz, stable release channel, used to power Fastly Compute and many embedded hosts [14][15] | Uses the Cranelift optimizing code generator; supports both JIT and ahead-of-time compilation; ships with WASI Preview 2 (`wasm32-wasip2` target) by default [14] |
| **Wasmer** | Wasmer Inc. (commercial), MIT licensed [16] | Universal, cross-platform runtime; "lightweight containers" from desktop to cloud/edge/browser | Production-ready; ships as CLI, embeddable library, and via Wasmer Edge platform | Pluggable compiler backends (Cranelift, LLVM, Singlepass); ships with WASI and **WASIX** (its POSIX-y superset) out of the box; strong embeddable SDK story across languages [16] |
| **WasmEdge** | CNCF sandbox; originally Second State / WasmEdge contributors; Apache 2.0 [11] | Cloud-native edge, microservices, embedded, and AI/LLM inference via WASI-NN and LlamaEdge | Production, used in edge/IoT and AI use cases; cited as "the fastest Wasm VM" per IEEE Software paper | Supports JavaScript (QuickJS-based), network sockets, MySQL/Postgres drivers, WASI-NN (ONNX/TensorFlow-Lite/PyTorch/OpenVINO backends), pluggable C-API extensions, and Kubernetes/Dapr integrations [11] |
| **V8 / Wasm engines used via embeddings** | Google (V8) and the browser vendors | Power isolates-based platforms | Mature, world-class JIT | Cloudflare Workers runs on V8 (`workerd`); note `workerd`'s own README warns that it is "not a hardened sandbox" on its own and Cloudflare layers additional defenses for multi-tenant hosting. [1] |
| **WAMR (WebAssembly Micro Runtime)** | Intel / Bytecode Alliance | Tiny footprint (tens of KB) for embedded/IoT | Production, widely deployed in constrained devices | Interpreter, AOT, and JIT modes; runs on RTOS and Zephyr-class devices. |
| **wasm3** | Volodymyr Shymanskyy / community | Ultra-small interpreter | Stable | Popular for MCU and embedded use cases. |

**Comparative notes.** Wasmtime is the de-facto reference implementation for new WASI/Component Model features and enjoys the strongest governance via the Bytecode Alliance; Wasmer emphasizes developer experience, universal binaries, and commercial "Wasmer Edge"; WasmEdge is the strongest option for AI inference (WASI-NN, LlamaEdge, GGML/llama.cpp bindings) and edge/IoT embeddings, with CNCF governance. Docker's containerd Wasm shims (runwasi) support all three, letting operators pick the runtime per workload. [5][6]

---

## 3. WASI Status: Stable vs. In-Proposal

The **WebAssembly System Interface (WASI)** is developed by the WASI Subgroup of the WebAssembly Community Group and is the primary standard enabling Wasm to interact with host resources (files, network, clocks, randomness) outside the browser. [17]

**Three milestones define WASI as of mid-2026:**

- **WASI 0.1 / Preview 1** — the original witx-defined, POSIX/CloudABI-inspired API set (filesystem, clocks, random, command-line args/environ). It is widely supported across runtimes but has been superseded by the modular Wit-based API family. [17]
- **WASI 0.2 / Preview 2** — stabilized modular APIs defined in the WIT IDL and built on the Component Model. Per the upstream WASI repo, Preview 2 "is a modular collection of APIs defined with the Wit IDL, incorporating many of the lessons learned from WASI 0.1, including support for a wider range of source languages, modularity, a more expressive type system, and virtualizability." [17] The following APIs have reached **Phase 3 (Implementation phase)** in the WASI process and ship in Preview 2: **wasi-clocks**, **wasi-random**, **wasi-filesystem**, **wasi-sockets**, **wasi-cli**, and **wasi-http**. [18] Wasmtime ships `wasm32-wasip2` as a stable Rust target today. [14]
- **WASI 0.3 / Preview 3** — the current active preview. It builds on 0.2 and replaces the explicit streams/poll model with the Component Model's native `async` support via `future` and `stream` types, enabling first-class async I/O without hand-rolled event loops. [17] The inclusion rule is explicit: to land in 0.3, a proposal must reach Phase 3, meet its own portability criteria, and be voted in by the WASI Subgroup. [18]

**Core WebAssembly specification progress (context for WASI).** The proposals repo lists the following as **finished** (Phase 4, merged into the spec) as of 2025–2026 WG action, shipped in WebAssembly 3.0: Tail Calls, Extended Constant Expressions, Typed Function References, Garbage Collection, Multiple Memories, Relaxed SIMD, Annotations, Branch Hinting, Exception Handling, JS String Builtins, and Memory64. These complement the 2.0 features (Reference Types, Bulk Memory, Fixed-width SIMD, Sign Extension, Non-trapping float-to-int, Multi-value, BigInt-i64). [19] Active proposals still progressing include Threads (Phase 4), JS Promise Integration (Phase 4), ESM Integration, Stack Switching, Shared-Everything Threads, the Component Model itself (Phase 1 in the CG, but already shipping in toolchains), and Stringref. [20]

**Additional WASI sub-proposals in flight (Phase 1–2)** include wasi-kv-store, wasi-messaging, wasi-nn (ML inference), wasi-crypto, wasi-sql, wasi-tls, wasi-logging, wasi-parallel, wasi-gpio/i2c/spi/usb (embedded), wasi-runtime-config, wasi-blob-store, wasi-url, wasi-observe, wasi-otel, and the proxy-wasm ABI (pre-proposal track). [18]

---

## 4. Concrete Production Use Cases (≥5)

The following examples each cite an explicit company or project rather than generic categories.

**1. Edge request processing at Cloudflare Workers.** Cloudflare runs customer JavaScript/TypeScript, Rust, Python, and compiled Wasm across its 330+ city network using the V8-based `workerd` runtime. Use cases include authentication, routing, rate limiting, A/B testing, and full-stack APIs deployed globally without regional configuration. The platform explicitly cites the isolate architecture as enabling "no cold starts" and per-request billing on CPU time only. [1][2]

**2. Shopify Functions — backend customization for merchants.** Shopify Functions compile to Wasm and execute inline during Shopify's checkout, discount, and shipping pipelines, replacing slower, script-based extension points. They support Rust and JavaScript/TypeScript compilation targets through the Shopify CLI and run with strict resource budgets in a shared multi-tenant environment. [4]

**3. Service-mesh extensibility with Envoy & Istio Proxy-Wasm.** Envoy ships an official `envoy.filters.http.wasm` HTTP filter that loads a `.wasm` plugin to customize L7 processing (auth, routing, header mutation, telemetry). The proxy-wasm/spec repository defines a language-and-proxy-neutral ABI (v0.2.1) with SDKs in Rust, Go, C++, and AssemblyScript, and host implementations spanning Envoy, Istio, NGINX, Apache Traffic Server, and MOSN — meaning the same extension binary can run across multiple data planes. [7][8]

**4. AI / LLM inference at the edge with WasmEdge + LlamaEdge.** WasmEdge positions itself as the easiest path to run LLMs on devices through the LlamaEdge framework (built on WasmEdge), with support for GGML-based llama.cpp, Whisper speech-to-text, Stable-Diffusion text-to-image, and TTS via WASI-NN plugins for PyTorch, TensorFlow-Lite, ONNX Runtime, and OpenVINO backends, running across GPUs on servers, PCs, and edge devices. [11][21]

**5. Cloud-native Wasm microservices with Spin/SpinKube and Docker + containerd shims.** Fermyon Spin is a batteries-included framework for Wasm microservices (HTTP components written in Rust, JS/TS, Python, Go), while **SpinKube** provides a Kubernetes operator (`SpinApp` CRD) for scheduling these workloads. [9][10] Parallel to that, the **runwasi/containerd-shim-wasm** project (used by Docker's Wasm integration) lets containerd and kubelet manage Wasm workloads alongside Linux containers via pluggable shims backed by Wasmtime, WasmEdge, or Wasmer. [6] Docker's 2022 Wasm technical preview framed this as bringing Wasm containers "faster and lighter than Linux containers" directly into the Docker workflow. [5]

**6. Universal plugin systems with Extism.** Extism advertises itself as a framework "for building with WebAssembly" on servers, edge, CLIs, IoT, and browsers, with SDKs in more than 15 host languages. Its explicitly stated primary use case is "building extensible software & plugins" — i.e., safely executing arbitrary, user-supplied code — and it layers on top of stock Wasm runtimes additional capabilities such as persistent module-scope variables, host-controlled HTTP, runtime limiters/timers, and simplified host-function linking. [12]

---

## 5. Technical Limitations Blocking Broader Adoption

Despite rapid progress, several well-documented limitations remain:

- **Async I/O is still stabilizing.** Preview 2 WASI programs use synchronous `wasi-io` poll/stream primitives; true ergonomic async requires Preview 3's `future`/`stream` integration with the Component Model's native async, which is still being rolled out across toolchains as of mid-2026. [17]
- **Threading is fragmented.** The "Threads" proposal (atomics + shared memory + wait/notify, tied to SharedArrayBuffer in browsers) is at Phase 4, but "Shared-Everything Threads" (threads that share Wasm GC/linear memory more broadly) is Phase 1; wasi-threads is also Phase 1 in WASI. Many runtimes still mark thread support as experimental or partial, and WasmEdge notes in its README that it is "not yet thread-safe" for embedded embedding. [11][18][20]
- **Incomplete Component Model tooling.** The Component Model — required for first-class composition across languages — is still Phase 1 in the CG process despite shipping in Wasmtime and tooling. Cross-language bindings, Wit package management, and ABI stability guarantees continue to mature. [17][20]
- **Performance gaps vs. native.** While Wasm is often billed as "near-native," real-world overhead depends heavily on the workload and runtime. The Wasmtime project has published detailed performance notes documenting tradeoffs between Cranelift and LLVM backends and ongoing work on optimizations; cross-boundary call overhead (host↔Wasm) remains a hot path for plugin workloads. [15]
- **Language support uneven for GC languages.** With Wasm GC now standardized (Wasm 3.0), Java, Kotlin, Dart, and Go can target Wasm more efficiently, but tooling support is still rolling out. .NET's Blazor relies on AOT trimming and bundling; Python and Ruby support largely depends on interpreted runtimes compiled into Wasm (e.g., CPython builds) rather than native compilation. [13][19]
- **Ecosystem and operations gaps.** Observability (logging, tracing, metrics — wasi-logging and wasi-otel are Phase 1/2), relational database access (wasi-sql is Phase 1), TLS (wasi-tls Phase 1), and key-value/messaging are not yet standardized, so developers rely on per-platform host bindings or vendor-specific APIs (e.g., Cloudflare Workers KV/R2/D1, Fastly KV/Object Store) that reduce portability. [18]
- **Sandboxing caveats.** Runtimes differ in their hardening story. Cloudflare's `workerd` README explicitly warns that it "is not a hardened sandbox" by itself and that running untrusted code requires additional defense-in-depth layers (which Cloudflare provides in production but self-hosters must arrange). [1] Production multi-tenant deployments need careful threat modeling.
- **Debugging and developer experience.** Source-level debugging across host↔Wasm boundaries, profiling, and observability integration are improving (Wasmtime supports Wasm DWARF-based debugging, for instance) but lag behind native code and JVM/CLR tooling.
- **Ecosystem fragmentation risk.** Wasmer's WASIX, Cloudflare's Workers API surface, Fastly Compute's host APIs, and browser APIs all extend Wasm in incompatible ways above the WASI baseline; Component Model and Preview 3 are intended to mitigate this, but real-world portability is not yet universal. [16]

---

## References

[1] Cloudflare, *workerd — Cloudflare's JavaScript/Wasm Runtime (README)*, GitHub. https://github.com/cloudflare/workerd

[2] Cloudflare, *Cloudflare Workers — Global Serverless Functions Platform* (product page). https://workers.cloudflare.com/

[3] Fastly, *Viceroy — local runtime for Fastly Compute* (README), GitHub. https://github.com/fastly/Viceroy

[4] Shopify, *Shopify Functions — API overview*, Shopify Dev Docs. https://shopify.dev/docs/api/functions

[5] Docker, *"Docker + Wasm: A Technical Preview"*, Docker Blog. https://www.docker.com/blog/docker-wasm-technical-preview/

[6] containerd community, *runwasi — Facilitating running Wasm workloads with containerd* (README), GitHub. https://github.com/containerd/runwasi

[7] Envoy Proxy, *Wasm HTTP filter configuration reference*, Envoy documentation. https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/wasm_filter

[8] Proxy-Wasm community, *WebAssembly for Proxies (ABI specification) — v0.2.1* (README), GitHub. https://github.com/proxy-wasm/spec

[9] Spin Framework, *Spin — framework for building cloud microservices with WebAssembly* (README), GitHub. https://github.com/spinframework/spin

[10] SpinKube, *Spin Operator — deploy Spin applications to Kubernetes* (README), GitHub. https://github.com/spinkube/spin-operator

[11] WasmEdge contributors, *WasmEdge — a lightweight, high-performance, extensible WebAssembly runtime* (README), GitHub. https://github.com/WasmEdge/WasmEdge

[12] Dylibso / Extism, *Extism — the WebAssembly framework* (README), GitHub. https://github.com/extism/extism

[13] Microsoft, *Blazor WebAssembly deployment and AOT documentation*, .NET / ASP.NET Core docs. https://learn.microsoft.com/en-us/aspnet/core/blazor/host-and-deploy/webassembly

[14] Bytecode Alliance, *Wasmtime — a standalone runtime for WebAssembly* (README), GitHub. https://github.com/bytecodealliance/wasmtime

[15] Bytecode Alliance, *"Wasmtime 1.0 and performance"* / Wasmtime performance notes, Bytecode Alliance articles. https://bytecodealliance.org/articles/wasmtime-10-performance

[16] Wasmer Inc., *Wasmer — fast and secure WebAssembly runtime* (README), GitHub. https://github.com/wasmerio/wasmer

[17] WebAssembly Community Group, *WebAssembly System Interface (WASI) overview* (README), GitHub. https://github.com/WebAssembly/WASI

[18] WebAssembly Community Group, *WASI proposals and phase status*, WebAssembly/WASI docs/Proposals.md. https://github.com/WebAssembly/WASI/blob/main/docs/Proposals.md

[19] WebAssembly Community Group, *Finished Proposals* (Wasm 1.0/2.0/3.0 features), WebAssembly/proposals. https://github.com/WebAssembly/proposals/blob/main/finished-proposals.md

[20] WebAssembly Community Group, *Active Proposals* (Component Model, Threads, JSPI, Stack Switching, etc.), WebAssembly/proposals. https://github.com/WebAssembly/proposals

[21] Bytecode Alliance, *wasi-nn — high-level bindings for ML inference*, GitHub. https://github.com/bytecodealliance/wasi-nn
