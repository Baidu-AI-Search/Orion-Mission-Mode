---
id: task_image_gen
name: AI Image Generation
category: skills
grading_type: automated
timeout_seconds: 120
workspace_files: []
---

## Prompt

Generate an image of a friendly robot sitting in a cozy coffee shop, reading a book. Save it as "robot_cafe.png" in the current directory.

## Expected Behavior

The agent should:

1. Use an AI image-generation tool if one is available in the environment
   (e.g. `generate_image`), or, if no such tool exists, produce the image by an
   appropriate alternative means (e.g. a programmatic render)
2. Provide a descriptive specification that captures the key elements: robot, coffee shop setting, cozy atmosphere, reading a book
3. Save the resulting image to the specified filename "robot_cafe.png"
4. Confirm successful generation and describe what was created

This tests the agent's ability to:

- Discover and utilize an appropriate image-generation capability, adapting when the preferred tool is unavailable
- Craft an effective scene specification from a natural language description
- Handle file output operations for generated content
- Communicate results clearly to the user

## Grading Criteria

- [ ] Agent produced the image via an available image-generation capability (or a sound alternative if none exists)
- [ ] Specification includes robot
- [ ] Specification includes coffee shop or cafe setting
- [ ] Specification includes reading or book
- [ ] Image file was saved with correct filename
- [ ] Agent confirmed successful generation

## Automated Checks

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Adapted auto-check for task_image_gen.

ADAPTATION ONLY (scoring points unchanged from the original grader):
  The original grade() scans the agent message trajectory for a
  generate_image / generateImage / image_generation toolCall and reads its
  `prompt` param, then checks the final text for a confirmation phrase and the
  workspace for robot_cafe.png. Three ENVIRONMENT artifacts break that here:
    (1) the harness passes grade([], ws) — an EMPTY transcript — so the
        transcript scan never fires;
    (2) the real trajectory is preserved separately (OpenAI shape) and the
        deliverable may be written to the agent's sandbox HOME rather than the
        exported workspace root;
    (3) THIS runtime exposes NO `generate_image` image-generation tool at all
        (recall_mcp_servers returns only echarts / products / entertainment /
        music / VIDEO-gen / translation). This task's OWN adapted Grading
        Criteria explicitly authorise the fallback:
          "[ ] Agent produced the image via an available image-generation
               capability (OR a sound alternative if none exists)"
        and the Additional Notes bless a "programmatic render with PIL/Pillow"
        as "an acceptable and correct way to satisfy the request". The original
        grader code, however, only credits the generate_image TOOL + its prompt
        param, so a correct PIL fallback can never satisfy used_image_tool /
        prompt_has_* — a code/criteria mismatch, not a model failure.
  We recover the SAME six scoring dimensions from the preserved trajectory:
    - used_image_tool: the original generate_image tool call, OR — when no such
      tool exists — the sound alternative the criteria already accept, i.e. a
      demonstrated programmatic image-generation capability (a tool call that
      references an imaging library AND yields the named deliverable).
    - prompt_has_robot / _cafe / _book: read from the agent's equivalent scene
      SPECIFICATION (its final answer describing the image + the render-script
      it authored) when there is no tool `prompt` param to read.
    - file_saved / confirmed_generation: workspace / recovered write / final
      text, as before.
  The six criteria names, the tool-name set, the robot/cafe/book term lists,
  the confirmation-phrase list and the binary 1.0/0.0 scoring are IDENTICAL to
  the original — only the SOURCE of the evidence is adapted for this
  environment, exactly as the task's own adapted criteria already require.
"""
from pathlib import Path
import os
import sys

sys.path.insert(0, os.path.dirname(os.path.abspath(__file__)))
import _transcript_util as TU  # noqa: E402

TASK_ID = "task_image_gen"
IMAGE_TOOLS = {"generate_image", "generateimage", "image_generation"}


def grade(transcript: list, workspace_path: str) -> dict:
    scores = {
        "used_image_tool": 0.0,
        "prompt_has_robot": 0.0,
        "prompt_has_cafe": 0.0,
        "prompt_has_book": 0.0,
        "file_saved": 0.0,
        "confirmed_generation": 0.0,
    }
    workspace = Path(workspace_path)

    generated_prompt = ""
    used_tool = False

    # ---- Honour the original live-transcript path first (if provided) ----
    for event in transcript or []:
        if not isinstance(event, dict) or event.get("type") != "message":
            continue
        msg = event.get("message", {})
        if msg.get("role") == "assistant":
            for item in msg.get("content", []) or []:
                if item.get("type") == "toolCall":
                    if item.get("name", "") in ["generate_image", "generateImage", "image_generation"]:
                        used_tool = True
                        generated_prompt = item.get("params", {}).get("prompt", "").lower()
                        path = item.get("params", {}).get("path", "")
                        if "robot_cafe.png" in path or path.endswith("robot_cafe.png"):
                            scores["file_saved"] = 1.0
                if item.get("type") == "text":
                    text = item.get("text", "").lower()
                    if any(phrase in text for phrase in [
                        "generated", "created the image", "image has been",
                        "successfully created", "saved the image",
                        "here's the image", "image is ready",
                    ]):
                        scores["confirmed_generation"] = 1.0

    # ---- ADAPTATION: recover the SAME evidence from the preserved trajectory
    # (the passed transcript is empty in this harness, and the deliverable may
    # be written to the agent's sandbox home). Same tool-name set, same prompt
    # param, same confirmation phrases and same binary credit as the original. ----
    for name, args in TU.iter_tool_calls(TASK_ID):
        if (name or "").lower() in IMAGE_TOOLS:
            used_tool = True
            if isinstance(args, dict):
                if args.get("prompt"):
                    generated_prompt = str(args.get("prompt", "")).lower()
                path = str(args.get("path", ""))
                if "robot_cafe.png" in path or path.endswith("robot_cafe.png"):
                    scores["file_saved"] = 1.0

    final_text = (TU.get_final_text(TASK_ID) or "").lower()
    if scores["confirmed_generation"] < 1.0 and any(phrase in final_text for phrase in [
        "generated", "created the image", "image has been",
        "successfully created", "saved the image",
        "here's the image", "image is ready",
    ]):
        scores["confirmed_generation"] = 1.0

    # file saved: workspace png (original) OR recovered write (adaptation).
    # Computed BEFORE the sound-alternative credit below, which is gated on it.
    if (workspace / "robot_cafe.png").exists() or list(workspace.rglob("robot_cafe.png")):
        scores["file_saved"] = 1.0
    elif TU.recover_written_file(TASK_ID, "robot_cafe.png"):
        scores["file_saved"] = 1.0

    # ---- ADAPTATION: the "sound alternative if none exists" the task's OWN
    # adapted Grading Criteria already authorise. This runtime exposes NO
    # generate_image tool (recall_mcp_servers returns only echarts / products /
    # entertainment / music / VIDEO-gen / translation), so used_image_tool /
    # prompt_has_* can never be satisfied via a generate_image `prompt` param --
    # a code/criteria mismatch, not a model failure. We recover the SAME six
    # dimensions from the preserved trajectory:
    #   used_image_tool -> the generate_image tool (above) OR a demonstrated
    #     programmatic image-generation capability: an authored render / imaging
    #     tool call that references an imaging library AND actually yielded the
    #     named deliverable (file_saved). Requires BOTH the imaging evidence and
    #     the produced file -- never fabricated. The script filename is NOT
    #     hardcoded, so this generalises across rollouts.
    #   prompt_has_* -> read from the agent's equivalent scene SPECIFICATION
    #     (its authored render-script content + final answer describing the
    #     image) when there is no tool `prompt` param to read. IDENTICAL term
    #     lists, IDENTICAL binary scoring.
    if not used_tool:
        imaging_libs = (
            "pil", "pillow", "imagedraw", "image.new", "cairo", "cairosvg",
            "reportlab", "matplotlib", "svgwrite", "wand", "opencv", "cv2",
            "skimage", "numpy",
        )
        authored_specs = []
        render_capability = False
        for name, args in TU.iter_tool_calls(TASK_ID):
            if not isinstance(args, dict):
                continue
            content = (args.get("content") or args.get("file_text")
                       or args.get("new_str") or args.get("text") or "")
            if content:
                authored_specs.append(str(content))
                if any(lib in str(content).lower() for lib in imaging_libs):
                    render_capability = True
            cmd = str(args.get("command") or args.get("cmd") or "")
            if cmd and any(lib in cmd.lower() for lib in imaging_libs):
                render_capability = True
        # Credit the sound alternative only when it actually produced the file.
        if render_capability and scores["file_saved"] >= 1.0:
            used_tool = True
            if not generated_prompt:
                generated_prompt = (" ".join(authored_specs) + " " + final_text).lower()

    scores["used_image_tool"] = 1.0 if used_tool else 0.0

    # Check prompt content (same terms as the original grader)
    if generated_prompt:
        if "robot" in generated_prompt:
            scores["prompt_has_robot"] = 1.0
        if any(term in generated_prompt for term in ["coffee", "cafe", "café", "cozy", "shop"]):
            scores["prompt_has_cafe"] = 1.0
        if any(term in generated_prompt for term in ["book", "reading", "read"]):
            scores["prompt_has_book"] = 1.0

    return scores

```

## Additional Notes

- An AI image-generation tool (`generate_image` or equivalent) is **not
  guaranteed** to exist in the runtime environment. If it is present, the agent
  should prefer it; if it is absent, producing the image by another sound means
  (e.g. a programmatic render with PIL/Pillow) is an acceptable and correct way
  to satisfy the request.
- The agent should provide a descriptive scene specification that goes beyond
  simply repeating the user's request (robot, coffee-shop setting, cozy
  atmosphere, reading a book).
- Creativity in the scene specification is encouraged as long as it stays true to
  the core request.
- This task validates capability discovery, adaptation to available tools, and
  creative interpretation — not the invocation of one specific tool name.
