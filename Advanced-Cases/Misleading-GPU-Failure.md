# Case Study: Intermittent GPU Detection & System Crashes on Windows 11

## Environment
- Windows 11 Home
- Intel CPU
- Nvidia RTX 3080
- Informal setting
- No physical access to device
- Symptoms:
  - Unresponsive display
  - Intermittent system crashes (Kernel-Power 41)
  - Missing device drivers (PCI device, SM bus)
  - TPM-related errors

---

## Initial Observation
- User initially suspected GPU failure.
- GPU was not detected in Device Manager.
- “PCI device” and “SM bus” drivers were missing and could not be installed via Device Manager or Windows Update.
- Event logs showed repeated graphics, chipset, and TPM errors, despite TPM being functional days earlier.

---

## Troubleshooting Steps

### 1. TPM Verification
- Checked TPM status in powershell, query suggested no TPM enabled, installed, or present.
- Found TPM unexpectedly disabled in BIOS.
- Re-enabled TPM, restoring expected TPM behavior.

### 2. Chipset & GPU Driver Investigation
- Performed clean chipset driver installation.
- GPU briefly appeared in Device Manager with Code 43.
- Clean Nvidia driver installation succeeded but caused immediate system crash (Kernel-Power 41).

### 3. Minimal Hardware Boot
- Tested with CPU, motherboard, and a single RAM stick to isolate components.
- System instability persisted, including freezes and crashes without a GPU installed.
- This effectively ruled out GPU as the primary failure point as user believed.
- Shifted focus to motherboard and firmware.

### 4. Motherboard & Firmware Investigation
- Multiple BIOS flash attempts failed due to system freezes.
- Event logs provided limited actionable information.
- Crashes occurred even before full BIOS initialization, indicating a low-level issue. E.g. corrupted firmware, physical layer issue.

### 5. User Consultation & Hardware Decision
- After 5+ hours of intermittent troubleshooting, the user asked me to stop and give my honest opinion.
- I explained that symptoms suggested motherboard instability, I explained if its software level, it could probably be fixed, if it was hardware, replacement MIGHT be necessary.
- After repeated discussion and users express willingness, I **reluctantly advised a motherboard/CPU/RAM upgrade**.
- A bundled upgrade was chosen for cost-effectiveness due to platform architecture and availability.

---

## New Hardware Installation
- Post-upgrade, the system immediately crashed, including failures before BIOS initialization.
- GPU detection issues persisted.
- Offline setup, driver installation, and updates did not resolve symptoms.
- Confirmed that the motherboard/CPU/RAM upgrade alone did not address the root cause.
- New BSOD error: Clock Watchdog Timeout 0x101 intermittently during use and frequently on startup.

---

## GPU Upgrade (User-Initiated)
- User independently chose to upgrade the GPU, despite my recommendation that it was unlikely to resolve the issue.
- Symptoms persisted.
- System instability seemed to increase and crashes became much nore frequent.
  
---

## Root Cause Discovery
- Monitoring and system behavior pointed to **intermittent PSU power delivery fluctuations**.
- Utilized HWInfo and CPU-Z to log system temps, voltages and frequencies.
- Power instability explained:
  - GPU initialization failures
  - SM bus and PCI device errors
  - Kernel-Power 41 crashes
  - Random system freezes (even in BIOS)
  - BSOD Watchdog error
  - Persistence of issues across multiple hardware upgrades

---

## Lessons Learned
- SM bus and PCI device errors can indicate **power delivery or motherboard instability**, not just missing drivers.
- PSU failures can mimic a wide variety of system faults, including common driver issues. Potentially resulting in snap judgement and skimming over important information.
- Intermittent power issues are difficult to log and diagnose but often present through non-deterministic system behavior.
- Honest communication and professional judgment are critical when users consider expensive hardware changes.
- Methodical isolation of variables prevents unnecessary replacements.
- Multiple seemingly unrelated system errors maybe difficult to connect.

---

## Outcome
- GPU and motherboard/CPU/RAM upgrades **did not resolve the issue**.
- Root cause was a failing PSU causing intermittent power fluctuations.
- User essentially upgraded his whole system.
- Troubleshooting demonstrated the importance of systematic diagnostics and avoiding assumptions based solely on symptoms.

---

## Conclusion
This case highlights how misleading hardware symptoms—GPU Code 43, SM bus errors, missing drivers, Kernel-Power 41 events—can originate from subtle power delivery issues. Careful troubleshooting, isolation of variables, and clear communication were essential in identifying the true root cause and guiding the user through informed decision-making.

---

## Technical Notes / Tools Used
- **Windows Powershell / ISE**
  - TPM troubleshooting.
  - Device manager event parsing.
- **Windows Event Viewer**
  - Kernel-Power (41), chipset, graphics, and TPM-related events
- **Device Manager**
  - Detection of missing PCI and SM bus drivers
- **BIOS / UEFI**
  - TPM configuration
  - Firmware behavior during crashes
- **Minimal Hardware Boot**
  - CPU, motherboard, single RAM stick
- **Driver Management**
  - Clean chipset driver installation
  - Awareness of DDU for future driver cleanup scenarios
- **System Monitoring**
  - CPU-Z / hardware monitoring tools for power behavior observation
- **Methodology**
  - Component isolation
  - Evidence-based troubleshooting
  - User-focused communication and decision support
- **Final Thoughts**
   - This one hurt a bit because I was convinced it was motherboard related. My friend ultimately eneded up upgrading his whole system. He blatantly expressed his willingness and desire to do so but my best guess was wrong and wasted a bunch of our time, and his money.
   - PSU issues are tough to identify if they don't start smoking or overheat. Their software based symptoms can vary wildly across unrelated systems making it easy to go down a rabbit hole of trying to solve one specific issue.
