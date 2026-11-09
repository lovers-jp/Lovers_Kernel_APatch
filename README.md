# Lovers Kernel APatch

**A custom Pixel kernel with adaptive tuning support.**
| Target family | Pixel 11 / Pixel 11 Pro / Pixel 11 Pro XL |

[English](README.md) | [日本語](README_ja.md)

Lovers Kernel APatch combines CPU scheduling, memory management, storage, and networking changes for the **Pixel 11 family**. It is designed for APatch/FolkPatch rooting workflows, with optional learning and control through **Lovers Kernel Tuner**.

> **Development release:** the current kernel builds successfully and passes KMI symbol checks. Five inherited ABI type differences remain, and physical-device validation is pending. See [validation details](docs/VALIDATION.md).

## Features

| Area | Included features |
|---|---|
| CPU scheduling | BORE on Android EEVDF, with a CASS CPU-placement port |
| IRQ balancing | SBalance with capacity-aware IRQ distribution and activity statistics |
| CPU frequency and idle | Schedutil adjustments, a TEO idle-prediction fix, and configurable CPU latency QoS |
| Memory management | DAMON/DAMOS adaptive reclaim, mTHP, and MGLRU tuning |
| ZRAM | Built-in ZRAM/ZSMALLOC, an LZ4-primary policy, and cold-page ZSTD recompression controls |
| Storage | ADIOS I/O scheduler, TMPFS tuning, and F2FS/EROFS changes |
| Networking | BBRv3, CAKE, and fq_codel support |
| Power policy | RCU_LAZY and power-efficient workqueues |
| SUSFS and protection | Standalone APatch-adapted SUSFS and Baseband Guard (BBG) |

Feature activity depends on the device, active CPUFreq governor, and runtime configuration. For example, schedutil controls apply to schedutil policies, and CASS's default mode gives valid Android vendor CPU selections priority. CAKE support does not automatically replace interface queues.

## Companion tools

### Lovers Kernel Tuner

**Lovers Kernel Tuner ** adds app-side learning, telemetry, and control for this kernel's tuning interfaces.

Open the app after installation and grant root access. For a new installation, AI tuning and automatic startup at boot are enabled by default. The app first observes supported features, then compares one setting at a time when workload conditions are suitable. Manual settings and a previously saved AI-OFF preference take priority.

Supported controls include CASS, SBalance, schedutil, CPU Idle, and ZRAM recompression timing, alongside existing controls. Charging limits—such as a **90% upper limit**—are available when the device exposes a supported charging interface.

The kernel provides the scheduling, IRQ-balancing, and recompression mechanisms; the app selects supported settings over time. Performance, FPS, battery-life, and long-term battery-health improvements have not been measured for this release.

### APatch SUSFS Control WebUI

**APatch SUSFS Control WebUI ** provides Japanese/English configuration for the standalone **`v2.3.0-ap1 / APatch-GKI`** SUSFS port.

Install its module ZIP through APatch/FolkPatch Manager, reboot, and open its WebUI from the manager. It supports saved rules, boot application, status reports, and configuration backup/restore. Some rule changes require a reboot. This WebUI release's physical-device operation remains unverified.

## Using this project

| Artifact | How it is used |
|---|---|
| Kernel "boot.img" | Using the manager associated with APatch, apply it to the "boot.img" file and repackage it. |
| SUSFS control module ZIP | Install as a module using an APatch-based Manager. |
| Lovers Kernel Tuner APK | Install as an Android app, open it, and grant root |


## Credits and licensing

Thanks to the Linux kernel, Android Common Kernel, Google Pixel kernel, SUSFS, Baseband Guard, BBR, BORE, ADIOS, and Sultan kernel contributors whose work this project builds on. Sultan-related changes are adapted for Android 6.12; they are not an unchanged import of the original kernel.
