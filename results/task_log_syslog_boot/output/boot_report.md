# First Boot Sequence Report — `combo` Production Server

*Source: `linux_syslog.log` — first recorded boot cycle (lines 1–345), from `syslogd` restart at `Jun 9 06:06:20` until the next `syslogd` restart at `Jun 10 11:31:44`.*

> **Note on timestamps:** A subset of kernel/network/named messages were stamped with an out-of-order wall-clock time (e.g. `06:06:17`, `10:07:xx`) during the early boot before NTP/local clock was fully set. Where possible, analysis uses the ordered flow of events rather than the printed clock value.

---

## 1. System Identification

| Item | Value |
|---|---|
| **Hostname** | `combo` |
| **Kernel version** | `Linux 2.6.5-1.358` (built Sat May 8 09:04:50 EDT 2004 by `bhcompile@bugs.build.redhat.com`, gcc 3.3.3 20040412 / Red Hat Linux 3.3.3-7) |
| **CPU model** | **Intel Pentium III (Coppermine)** stepping 06, detected at ~**731 MHz**, 16K L1 I/D cache, 256K L2 cache, **1441.79 BogoMIPS**; Intel machine-check reporting enabled |
| **Total RAM** | **~126 MB LOWMEM** (129,720 kB total; ~125,312 kB available after kernel/reserved; 0 MB HIGHMEM). Kernel command line: `ro root=LABEL=/ rhgb quiet` |

Supporting evidence:
- `Linux version 2.6.5-1.358 ...` (line 5)
- `Detected 731.214 MHz processor.` (line 30)
- `CPU: Intel Pentium III (Coppermine) stepping 06` (line 51)
- `126MB LOWMEM available.` / `Memory: 125312k/129720k available` (lines 13, 33)

---

## 2. Storage

- **Primary hard drive:** **IBM-DTLA-307015** (ATA/IDE, UDMA/66) attached as `/dev/hda` on `ide0` (IRQ 14, I/O 0x1f0-0x1f7).
  - Geometry: `29336832` sectors (512 B/sector) → **15,020 MB ≈ 14.7 GB**
  - Cache: **1916 KiB**; CHS 29104/16/63; max request 128 KiB; CFQ I/O scheduler.
  - Partitions detected: `hda1 hda2 hda3` (hda1 = separate mount; hda2 = root; hda3 = swap).
- **Optical drive:** **SAMSUNG CD-ROM SN-124** (ATAPI 24X, 128 kB cache) on `ide1` as `/dev/hdc` — UDMA blacklisted/disabled.
- **Swap:** `/dev/hda3` added as 262,072 kB (~256 MB) swap, priority -1.
- **Root filesystem:** Initially mounted as **ext2** read-only, then journal-recovered and remounted as **EXT3** (ordered data mode, internal journal) on `/dev/hda2`. Boot line `root=LABEL=/`; `/dev/hda1` is also an EXT3 volume mounted separately.

Supporting evidence:
- `hda: IBM-DTLA-307015, ATA DISK drive` / `hda: 29336832 sectors (15020 MB) w/1916KiB Cache` (lines 112, 127)
- `VFS: Mounted root (ext2 filesystem).` → `EXT3-fs: recovery complete.` → `EXT3-fs: mounted filesystem with ordered data mode.` (lines 157–164)
- `Adding 262072k swap on /dev/hda3.` (line 181)
- `hdc: SAMSUNG CD-ROM SN-124, ATAPI CD/DVD-ROM drive` / `hdc: Disabling (U)DMA ... (blacklisted)` (lines 118, 120)

---

## 3. Boot Timeline

- **First log entry (boot start):** `Jun 9 06:06:20` — `syslogd 1.4.1: restart.`
- **Last service of the boot sequence:** `mysqld: Starting MySQL: succeeded` at `Jun 9 06:07:22` (followed only by `mdmonitor` (mdadm, non-service utility) at 06:07:23 and the failed `mdmpd` at 06:07:24).
- **Approximate boot duration (syslogd restart → last service startup succeeded):** **~62 seconds** (from 06:06:20 to 06:07:22).

Key milestone timestamps (ordered as logged):
- 06:06:20 — syslogd/klogd, irqbalance, portmap, nfslock start
- 06:06:21 — rpcidmapd, pcmcia; CPU detected
- 06:06:22 — bluetooth (hcid/sdpd), network/loopback, serial/AGP
- 06:06:23 — netfs mount, IDE controller probed, apmd
- 06:06:24 — autofs, smartd, IBM hard drive + CD-ROM detected
- 06:06:26 — root (ext2/ext3) mounted; hpoj
- 06:06:27–06:06:34 — EXT3 journal recovery, USB, NIC (3c905C) detected, iptables, Bluetooth stack, parallel/lp
- 06:06:37–06:06:52 — userspace services: cups, sshd, xinetd, sendmail+sm-client, spamassassin, privoxy, gpm, htt, canna, crond, xfs, anacron, atd, readahead, messagebus
- 06:07:06 — httpd
- 06:07:13–06:07:17 — named (BIND), snmpd, ntpd
- 06:07:22 — **mysqld** (last successful service)

(Note: `named` listening lines and some `ntpd` lines carry an incorrectly-set clock ~4 hours ahead (`10:07:xx`) because they were flushed after named opened its sockets while the system clock was still being adjusted; the syslog "tag" line `named: named startup succeeded` is correctly stamped 06:07:13.)

---

## 4. Services — "startup succeeded" List

Counting every service/init script that logged a "startup succeeded" (or equivalent "… :  succeeded" rc message) during the first boot — **37 total**:

| # | Service | Timestamp |
|---|---|---|
| 1 | syslogd | 06:06:20 |
| 2 | klogd | 06:06:20 |
| 3 | irqbalance | 06:06:20 |
| 4 | portmap | 06:06:20 |
| 5 | rpc.statd (nfslock) | 06:06:20 |
| 6 | random number generator | 06:06:21 |
| 7 | rpc.idmapd (rpcidmapd) | 06:06:21 |
| 8 | pcmcia (rc) | 06:06:21 |
| 9 | network — setting network parameters | 06:06:17* |
| 10 | network — bringing up loopback | 06:06:17* |
| 11 | netfs (mounting other filesystems) | 06:06:23 |
| 12 | hcid (bluetooth) | 06:06:22 |
| 13 | sdpd (bluetooth) | 06:06:22 |
| 14 | apmd | 06:06:23 |
| 15 | autofs (automount) | 06:06:24 |
| 16 | smartd | 06:06:24 |
| 17 | hpoj (rc) | 06:06:26 |
| 18 | cups (cupsd) | 06:06:37 |
| 19 | sshd | 06:06:38 |
| 20 | xinetd | 06:06:38 |
| 21 | sendmail | 06:06:40 |
| 22 | sm-client (sendmail) | 06:06:41 |
| 23 | spamd (spamassassin) | 06:06:44 |
| 24 | privoxy | 06:06:45 |
| 25 | gpm | 06:06:46 |
| 26 | htt (IIim input method) | 06:06:46 |
| 27 | canna | 06:06:49 |
| 28 | crond | 06:06:49 |
| 29 | xfs (X font server) | 06:06:50 |
| 30 | anacron | 06:06:51 |
| 31 | atd | 06:06:51 |
| 32 | readahead (rc) | 06:06:51 |
| 33 | messagebus (D-BUS) | 06:06:52 |
| 34 | httpd | 06:07:06 |
| 35 | named (BIND 9) | 06:07:13 |
| 36 | snmpd | 06:07:15 |
| 37 | ntpd | 06:07:17 |

\* The `network` messages appear at 06:06:17 because they were buffered in klogd before the clock was set; they still belong to the first-boot sequence.

Additionally logged as part of the rc sequence but usually treated as kernel/infra rather than services: **mysqld "Starting MySQL: succeeded"** at 06:07:22 and **mdmonitor "mdadm succeeded"** at 06:07:23, bringing the total count of *"succeeded"* markers to **39** if you include them.

**Count of discrete services reporting startup success (daemons + rc scripts, excluding the two network sub-steps as one network service):** **32 daemons + 4 rc scripts = 36 services (37 if counting network as 2 steps).** For a conservative "startup succeeded" line count the answer is **37 lines**.

---

## 5. Errors, Warnings & Notable Issues

### Security framework
- **SELinux registered but then disabled at runtime**: `SELinux: Initializing.` → `Starting in permissive mode` → `There is already a security framework initialized, register_security failed.` → `Failure registering capabilities with the kernel` → finally `SELinux: Disabled at runtime.` and `Unregistering netfilter hooks`. The machine ended up running **without SELinux enforcement**, with a secondary capability module registered instead.
- **Audit netlink socket disabled**: `audit: initializing netlink socket (disabled)`.

### ACPI / BIOS / PCI
- **ACPI completely disabled** because the BIOS is dated year 2000 (`ACPI disabled because your bios is from 2000 and too old; You can enable it with acpi=force`), and `ACPI: Interpreter disabled.`
- **Invalid ACPI IRQ routing**: `ACPI: ACPI tables contain no PCI IRQ routing entries` / `PCI: Invalid ACPI-PCI IRQ routing table` — IRQ routing fell back to the PIIX/ICH router.

### Storage / media
- **CD-ROM UDMA blacklisted**: `hdc: Disabling (U)DMA for SAMSUNG CD-ROM SN-124 (blacklisted)` — the optical drive was forced to PIO.
- **`cdrom: open failed.`** during early boot (no media present/drive not ready).
- **EXT3 orphan recovery**: `EXT3-fs: hda2: orphan cleanup on readonly fs` / `4 orphan inodes deleted` — indicates the previous shutdown was **not clean**.

### CPU microcode
- `microcode: CPU0 already at revision 0x8` / `microcode: No suitable data for cpu 0` — microcode update had nothing to apply (benign).

### Services that failed / misbehaved
- **telnet failed to start under xinetd**: `bind failed (Address already in use (errno=98)). service=telnet` → `Service telnet failed to start and is deactivated.` (xinetd reports `Started working: 30 available services`, so telnet is NOT one of them).
- **mdmpd failed**: `mdmpd: mdmpd failed` (MD multipath daemon did not start; `mdmonitor`/mdadm itself started fine).
- **xinetd internal service misconfiguration**: `No such internal service: services/stream - DISABLING` (a typoed entry in xinetd config).
- **named (BIND) rndc control channels not added**: `couldn't add command channel 127.0.0.1#953: not found` and `couldn't add command channel ::1#953: not found` — rndc remote control on port 953 (IPv4 and IPv6) unavailable, so rndc can't manage the nameserver.
- **ntpd unknown config keyword**: `configure: keyword "authenticate" unknown, line ignored`.
- **ntpd time-sync state**: `kernel time sync disabled 0041` at 06:10:35, later `kernel time sync enabled 0001` once synchronized to `LOCAL(0)` stratum 10.
- **syslogd uses obsolete API**: `process 'syslogd' is using obsolete setsockopt SO_BSDCOMPAT` (compat warning, not fatal).
- **IPv6 Privacy Extensions disabled** on loopback (expected on this kernel).
- **Post-boot (later that night) — `logrotate: ALERT exited abnormally with [1]`** at Jun 10 04:03:02 (not during boot proper, but the first post-boot error logged).
- **Post-boot xinetd fd issue** Jun 10 04:02:23: `file descriptor of service comsat has been closed` / `select reported EBADF but no bad file descriptors were found`.

---

## 6. Network

### Detected NIC
- **Network card:** **3Com 3c905C Tornado** (driver `3c59x` / vortex), PCI 0000:01:0c.0 at I/O base 0xec80, IRQ 5.
  - Detected/twice-initialized in the log (the vortex driver is probed twice during PCI init); the link comes up as **`eth0`**.
- Loopback `lo` is brought up by the network init script.
- Network stack: IPv4 + IPv6 (`NET: Registered protocol family 10`; IPv6-over-IPv4 tunneling driver loaded), Netfilter/iptables loaded (`ip_tables: (C) 2000-2002 Netfilter core team`), IPsec netlink socket initialized.
- Bluetooth stack also loaded (core/HCI/L2CAP/RFCOMM).
- `rsyncd` listening on port 873 (daemon started at 06:07:23, but no explicit "succeeded" line).

### Configured IP addresses — count from BIND `named` listeners

BIND (named 9.2.3, chrooted `/var/named/chroot`, running as user `named`) logged every IPv4 interface it bound to:

| # | Interface | IP address |
|---|---|---|
| 1 | lo | 127.0.0.1 |
| 2 | eth0 | 63.126.79.67 |
| 3 | eth0:1 | 63.126.79.69 |
| 4 | eth0:2 | 63.126.79.70 |
| 5 | eth0:3 | 63.126.79.71 |
| 6 | eth0:4 | 63.126.79.72 |
| 7 | eth0:5 | 63.126.79.73 |
| 8 | eth0:6 | 63.126.79.75 |
| 9 | eth0:7 | 63.126.79.80 |
| 10 | eth0:8 | 63.126.79.81 |
| 11 | eth0:9 | 63.126.79.82 |
| 12 | eth0:10 | 63.126.79.83 |
| 13 | eth0:11 | 63.126.79.84 |
| 14 | eth0:12 | 63.126.79.85 |
| 15 | eth0:13 | 63.126.79.87 |
| 16 | eth0:14 | 63.126.79.89 |
| 17 | eth0:15 | 63.126.79.90 |
| 18 | eth0:16 | 63.126.79.95 |
| 19 | eth0:17 | 63.126.79.100 |
| 20 | eth0:18 | 63.126.79.105 |
| 21 | eth0:19 | 63.126.79.110 |
| 22 | eth0:20 | 63.126.79.115 |
| 23 | eth0:21 | 63.126.79.120 |
| 24 | eth0:22 | 63.126.79.125 |

**Total configured IPv4 addresses on the system: 24**
- 1 × loopback (`lo` → 127.0.0.1)
- 1 × primary `eth0` (63.126.79.67)
- **22 IP aliases** on `eth0` (eth0:1 through eth0:22, all within 63.126.79.64/26 — appears to be a public-subnet virtual-hosting server).

No IPv6 listeners were bound by named (the rndc `::1#953` channel failed), consistent with IPv6 being loaded but no v6 address configured on the box. The server thus presents **one physical NIC (eth0)** hosting **23 public IP addresses** plus loopback.

---

## Summary

- **Kernel/CPU/RAM:** Red Hat–built Linux 2.6.5-1.358 on a 731 MHz Intel Pentium III (Coppermine) with 126 MB RAM.
- **Storage:** 15 GB IBM-DTLA-307015 IDE disk, root on `/dev/hda2` as **EXT3** (recovered from unclean shutdown), ~256 MB swap on hda3.
- **Boot time:** ≈ **62 seconds** from syslogd restart (06:06:20) to mysqld coming up (06:07:22).
- **Services:** **37** distinct "startup succeeded" markers during first boot.
- **Problems to investigate:** SELinux silently disabled at boot, ACPI completely off (year-2000 BIOS), telnet binding failed (port in use), mdmpd failed, named missing rndc control channel, logrotate abnormal exit next morning, unclean prior shutdown (orphan inodes).
- **Network:** Single **3Com 3c905C Tornado** NIC (`eth0`) with **24 IPv4 addresses total** (loopback + primary + 22 aliases in 63.126.79.64/26) all listening on BIND port 53.
