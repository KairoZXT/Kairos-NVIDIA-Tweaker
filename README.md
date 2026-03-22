# ⚡ Kairos NVIDIA Tweaker

# 👏 About

KairoNVIDIATweaker is an open-source Windows batch utility that automatically optimizes NVIDIA GPU driver registry settings for improved performance, lower latency, and better system responsiveness.

The script targets key driver-level registry values commonly adjusted by advanced tweakers — covering power state management, scheduling, ASPM, HDCP, ECC, telemetry, and NVIDIA driver logging. These settings influence how Windows and the NVIDIA driver manage GPU power, scheduling behaviour, and background overhead.

A built-in **automatic backup system** snapshots your original registry state on every first launch, making every change fully revertible with a single keypress.

The script automatically configures recommended values for:

**Power State Management** – Disables dynamic and async P-state transitions to keep the GPU at full performance at all times

**Clock Gating (BLCG / ELCG / SLCG / FSPG)** – Disables GPU block-level, engine-level, and second-level clock gating to reduce micro-stutter and scheduling variance

**Power Thresholds** – Sets idle entry thresholds for NVDEC, NVENC, NVJPG, OFA, and graphics engine power gating to near-zero, eliminating unnecessary power state transitions

**ASPM (Active State Power Management)** – Disables PCIe Active State Power Management on the GPU to prevent link state latency

**HDCP** – Disables High-bandwidth Digital Content Protection initialisation overhead

**ECC (Error Correction Code)** – Disables ECC memory error correction on consumer GPUs where it has no practical benefit but adds overhead

**GraphicsDrivers Scheduler** – Optimises Windows GPU scheduler settings including hardware scheduling mode, preemption, queued present limits, and priority boost behaviour

**Telemetry** – Disables NVIDIA telemetry services, scheduled tasks, crash reporters, and telemetry DLLs

**Driver Logging** – Disables NVIDIA kernel driver logging to reduce unnecessary background I/O

# 🔒 Backup & Revert System

KairoNVIDIATweaker includes a smart backup system that runs silently every time the script is launched for the first time.

Before any changes are made, the script checks every registry value it intends to modify:

- **Value does not exist** — marked for deletion on revert (will be cleanly removed)
- **Value exists with a different data** — original value is saved to `C:\KairoBackUp\original_values.reg`
- **Value exists and already matches** — skipped, nothing to back up

All backup files are stored at `C:\KairoBackUp\`:

| File | Purpose |
|---|---|
| `original_values.reg` | Registry values that existed before tweaking, with their original data |
| `delete_on_revert.txt` | Values that did not exist before tweaking and should be deleted on revert |
| `backup_done.flag` | Prevents the backup from running more than once per session |

Selecting **[2] REVERT** from the menu will restore all original values, delete any newly added values, and reset the backup flag so a fresh snapshot is taken on the next launch.

# 🚨 Disclaimer

This script modifies Windows registry settings related to GPU driver behaviour and system scheduling.

While these tweaks have been tested and are beneficial on many systems, results may vary depending on hardware, NVIDIA driver version, and Windows version.

The author is not responsible for any damage, instability, or data loss that may occur from running this script.

**Use at your own risk. Always benchmark before and after to measure real impact.**

# 💻 Requirements

- Windows 10 or Windows 11
- NVIDIA GPU (GTX, RTX series)
- Administrator privileges
- Basic understanding of Windows optimisation (recommended)

# ❓ How to Use

1. Download the batch file
2. Right-click the file and select **Run as Administrator**
3. Press any key to continue past the intro screen
4. Select **[1] TWEAK** to apply all optimisations
5. Restart your computer for all changes to fully take effect

To undo all changes at any time, relaunch the script and select **[2] REVERT**.

# 🔧 What This Script Tweaks

**DisableDynamicPstate / DisableAsyncPstates**
Prevents the GPU from dynamically shifting between performance states during a session. Eliminates P-state transition overhead that can cause frame time variance.

**RMPowerFeature / RMPowerFeature2**
Internal NVIDIA RM (Resource Manager) power feature flags. Configured to disable unnecessary power saving features within the driver.

**Clock Gating (RMBlcg, RMElcg, RmElpg, RMSlcg, RMFspg)**
Controls block-level, engine-level, second-level, and fine-grained power gating inside the GPU. Disabling these prevents the hardware from powering down idle functional units, reducing wake-up latency and scheduling jitter.

**Power Management Thresholds**
A set of idle threshold values for NVDEC, NVENC, NVJPG, OFA, and graphics ring power gating. Setting these to 1 microsecond effectively prevents the driver from entering low-power idle states on these engines during active use.

**ASPM (RMDisableGpuASPMFlags / RMEnableASPMDT / RMEnableASPMAtLoad)**
Disables PCIe Active State Power Management on the GPU link. ASPM can introduce latency when the PCIe link transitions between power states, particularly under bursty GPU workloads.

**HDCP (RmDisableHdcp22 / RMHdcpKeyglobZero / RMSkipHdcp22Init)**
Disables HDCP 2.2 initialisation and key loading. On displays that do not require HDCP, this overhead is unnecessary.

**ECC (RMNoECCFuseCheck / RMEnableL1ECC / RMEnableSMECC)**
Disables Error Correction Code memory checking. ECC is intended for workstation and data centre GPUs. On consumer cards it provides no practical benefit and adds memory bandwidth overhead.

**GraphicsDrivers Scheduler**
Configures Windows WDDM scheduler settings including enabling Hardware Accelerated GPU Scheduling (HAGS), disabling priority boost interference, setting queued present limits, and disabling TDR (Timeout Detection and Recovery) to prevent driver resets during heavy workloads.

**Telemetry**
Disables the NvTelemetryContainer service, NVIDIA crash report scheduled tasks, GeForce Experience background tasks, and removes telemetry DLLs from the driver store.

**Driver Logging**
Disables NVIDIA kernel mode driver (nvlddmkm) internal logging masks to reduce unnecessary background disk I/O.

# 📜 License

This project is released under the MIT License.

You are free to use, modify, and distribute this script as long as the original license is included.

---

MIT License

Copyright (c) 2026 Kairo

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
