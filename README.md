# AkariOS
A one-click Windows setup for power users. Paste one line into PowerShell and AkariOS debloats Windows, applies a long list of performance and privacy tweaks, installs a few essentials, and cleans up after itself.

> Based on [WinSux](https://github.com/FR33THYFR33THY/WinSux) by FR33THY (MIT License). See [Credits](#credits).

# Read this first
AkariOS changes a lot of the system, including security features. It is built for people who know what they are turning off.

- **Run it on a fresh install or a machine you can reinstall.** There is no uninstaller.
- **Security is reduced on purpose.** See [Security changes](#security-changes).
- **Your PC restarts several times.** Close your work and don't interrupt it.
- **Graphics drivers are removed.** You install a fresh one afterwards (see [Graphics](#graphics)).
- **It is tuned for desktops.** The power plan turns off power saving, so laptop battery life will suffer.
- **Use it at your own risk.** The restore point it creates is made at the end of the run, so it captures the system after the changes.

# Requirements
- Windows 10/11 Home/Pro/LTSC/IoT/Server
- Online access
- An elevated (Administrator) PowerShell window

# IWR
Paste the code below into an elevated Administrator PowerShell window.
```
iwr 'https://github.com/isleap9/Akari-OS/raw/refs/heads/main/akarios.ps1' -useb | iex
```
If a download fails, the script says which file is missing and stops before it changes anything.

# What it does
The run has three phases. The script restarts the PC between them and carries on by itself.

## Phase 1: preparation (`akarios.ps1`, normal boot)
- Checks your internet connection and downloads every file it needs. If any required file is missing, it stops here.
- Installs and configures **7-Zip**.
- Installs the **Visual C++ Redistributables** (2005 through 2015-2022, x86 and x64) and the **DirectX** runtime.
- Sets up **Display Driver Uninstaller (DDU)** and blocks Windows Update from downloading drivers.
- Installs the **Helium** browser with hardware acceleration and background mode off, and removes its auto-start entries, services and scheduled tasks.
- Downloads **[AkariOS Ultimate](https://github.com/isleap9/AkariOS-Ultimate)** to a folder on your Desktop.
- Downloads the AkariOS wallpaper.
- Removes Microsoft Store apps, turns off "open terminal by default", allows password sign-in, then turns on Safe Mode and restarts.

## Phase 2: Safe Mode (`stepone.ps1`)
- Changes Windows Security settings that can only be changed in Safe Mode.
- Turns off **UAC**.
- Uses DDU to uninstall **GPU and audio drivers** (NVIDIA, AMD, Intel, Realtek, Sound Blaster), then leaves Safe Mode and restarts.

## Phase 3: tweaks and cleanup (`steptwo.ps1`, normal boot)
**Removal**
- Uninstalls **Microsoft Edge**, **OneDrive**, Remote Desktop Connection, and other apps, Windows features and legacy components.
- Clears third-party startup apps and scheduled tasks.

**Windows settings** (applied from `reg.reg` plus extra commands)
- Dark theme with transparency off, show file extensions and hidden files, classic right-click menu with the clutter items removed (Share, Send to, Give access to, Scan with Defender and more).
- Cleaner Start menu in list view, taskbar unpinned, show-desktop corner on, Snap settings configured, Recycle Bin on the desktop.
- Black sign-in and lock screen, with the AkariOS wallpaper on the desktop.
- Store updates, app permissions, suggestions, notifications and a long list of telemetry and privacy settings turned off.
- Windows Update paused, driver updates blocked, BitLocker and memory compression disabled, network adapters limited to IPv4.

**Performance**
- **Ultimate Performance** power plan with sleep, hibernate, fast boot, power throttling and CPU core parking off, plus USB, PCI, network adapter and device power saving off.
- A **timer resolution service** for lower input latency.
- Runs Disk Cleanup, then creates a restore point and restarts.

# Security changes
Defender and Windows Security are weakened on purpose so they don't get in the way. The changes include:

- Cloud-delivered protection and automatic sample submission off
- Memory integrity and virtualization-based security off
- LSA protection and the Microsoft vulnerable driver blocklist off
- UAC off
- Other Windows Security settings changed, including real-time and tamper protection, SmartScreen, controlled folder access and exploit protection. `stepone.ps1` has the exact values.

Only use AkariOS if you accept that, and don't use it on a machine that handles anything sensitive.

# Graphics
Because the graphics driver is removed in phase 2, install a fresh one afterwards. These options come from [AkariOS Ultimate](https://github.com/isleap9/AkariOS-Ultimate); paste into an elevated PowerShell window.

- Install updated graphics driver
```
iwr 'https://github.com/isleap9/AkariOS-Ultimate/raw/refs/heads/main/5%20Graphics/2%20Driver%20Updated%20Install.ps1' -useb | iex
```
- Install updated graphics driver & import settings
```
iwr 'https://github.com/isleap9/AkariOS-Ultimate/raw/refs/heads/main/5%20Graphics/3%20Driver%20Updated%20Install%20&%20Settings.ps1' -useb | iex
```
- Install debloated graphics driver & import settings
```
iwr 'https://github.com/isleap9/AkariOS-Ultimate/raw/refs/heads/main/5%20Graphics/4%20Driver%20Debloat%20Install%20&%20Settings.ps1' -useb | iex
```

# Repository contents
| File | Purpose |
| --- | --- |
| `akarios.ps1` | Phase 1. Downloads files, installs essentials, starts Safe Mode |
| `stepone.ps1` | Phase 2. Runs in Safe Mode: security settings, UAC, DDU |
| `steptwo.ps1` | Phase 3. Debloat, settings, power plan, cleanup |
| `reg.reg` | Registry tweaks imported in phase 3 |
| `start2.txt` | Windows 11 Start menu layout |
| `settimerresolutionservice.cs` | Source for the timer resolution service |
| `img.jpg` | AkariOS wallpaper |
| `AllowScripts.cmd` | Turns PowerShell script execution on or off and unblocks downloaded files |

The installers and tools the scripts download come from the [AkariOS-Files](https://github.com/isleap9/AkariOS-Files) release.

# Credits
AkariOS is built on **[WinSux](https://github.com/FR33THYFR33THY/WinSux) by [FR33THY](https://www.youtube.com/fr33thy)**, used under the MIT License. The structure, most of the scripts and many of the tweaks come from his work, with changes and additions by isleap9. Thank you FR33THY.

The tools the scripts install belong to their respective authors: [7-Zip](https://www.7-zip.org), [Display Driver Uninstaller](https://www.wagnardsoft.com/display-driver-uninstaller-ddu), [Helium](https://helium.computer), and Microsoft's Visual C++ and DirectX runtimes.
