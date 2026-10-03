# Zimi-WL Specification

## 1. Purpose

Zimi-WL is a minimal Zig Wayland compositor prioritizing:

- correctness and security
- performance
- minimal code and dependencies
- low runtime overhead
- practical desktop compatibility
- no systemd requirement

Performance should primarily come from minimalism and simple implementation, not complex optimization machinery.

## 2. Project structure

The compositor implementation consists of exactly:

1. One project-owned Zig source file.
2. One runtime configuration file.

No additional `.zig`, `.c`, `.h`, layout, keybinding, configuration, generated, or plugin source files.

The configuration is runtime-readable and does not require recompilation. Restarting the compositor to apply changes is acceptable.

CI, documentation, specifications, tests, and build metadata do not count as compositor source files.

## 3. Required technology

Hard requirements:

- Zig
- wlroots
- libseat
- XWayland
- xdg-desktop-portal-wlr

Use libseat for seat/session/device management without hard-coding a backend. No custom seat-management system.

No requirements for systemd, logind, PAM, or a desktop environment.

## 4. Tiling

Zimi-WL has exactly one layout: automatic equal-width columns.

For `N` visible windows on an output, each receives `1/N` of the available width.

Windows never overlap.

Adding, removing, hiding, showing, or moving windows automatically recalculates the layout.

Moving a window changes its tiling order only.

There is no:

- floating
- manual resizing
- adjustable ratio
- master/stack layout
- per-window size
- fullscreen layout control
- gaps configuration

## 5. Focus

Focus follows the mouse.

Pointer entry determines the focused window according to normal compositor focus rules.

Keyboard input goes to the focused client.

There is no separate keyboard-driven focus-navigation mode or focus-navigation keybinding.

Focus must remain correct across window creation/destruction, workspace changes, output changes, and XWayland lifecycle events.

## 6. Workspaces

There are exactly nine normal workspaces: `1–9`.

They must support:

- switching workspaces
- moving the focused window between workspaces
- independent workspace state
- multiple outputs
- correct handling of window creation/destruction/hiding/movement

Workspace movement is performed only through workspace-number keybindings.

No special workspaces, scratchpads, workspace naming, automatic workspace creation, or mouse drag-to-workspace behavior.

Default bindings:

- `Mod+1` … `Mod+9` → switch workspace
- `Mod+Shift+1` … `Mod+Shift+9` → move focused window to workspace

## 7. Keybindings

Keybindings are runtime-configurable.

There are exactly four bindable action types:

### Command execution

Any number of bindings may execute configured commands, analogous to dwl `spawn` / Hyprland `exec`.

Example:

`Mod+E` → configured application command.

### Workspace switching

Switch to workspace `1–9`.

### Move focused window

Move the focused window to workspace `1–9`.

### Close focused window

Cleanly close the focused window, analogous in purpose to dwl `killclient` / Hyprland `killactive`.

No other compositor actions are bindable.

Specifically no bindings/actions for:

- volume
- mute
- brightness
- compositor restart
- compositor exit
- monitor power
- DPMS
- suspend/hibernate
- screensaver
- screen locking
- fullscreen
- floating
- resizing
- ratio changes
- gaps
- focus navigation
- scratchpads
- special workspaces
- gestures
- IPC

There is no compositor restart or exit mechanism.

## 8. Configuration

The configuration file is:

- runtime-readable
- human-editable
- independent of Zig source
- capable of defining the four allowed keybinding types

It must remain simple and must not become a general-purpose configuration framework.

## 9. Wayland and outputs

Provide the Wayland functionality necessary for normal desktop applications and explicitly required integrations.

Use wlroots infrastructure rather than reimplementing it.

Support:

- keyboard
- pointer
- multiple outputs
- outputs present at compositor startup
- native Wayland clients
- normal client lifecycle
- correct workspace/layout state across outputs

Runtime monitor/connector hotplug is **not required**.

Display scaling is not supported.

## 10. XWayland

XWayland is mandatory.

Support X11 window creation, mapping, destruction, focus, input, disconnects, tiling, workspaces, outputs, and cleanup.

Use wlroots XWayland support.

## 11. xdg-desktop-portal-wlr

Compatibility with xdg-desktop-portal-wlr is mandatory, including supported desktop capture/screencasting functionality.

Do not implement a competing portal backend.

## 12. Pointer constraints and gaming

Support the Wayland pointer-constraints and relative-pointer functionality required for applications that lock/confine the pointer and consume relative mouse motion.

Correctly handle focus/workspace/output changes, unmap/destroy, unlocking, and constraint destruction.

Use wlroots infrastructure.

## 13. IPC

Zimi-WL has no IPC.

Do not implement IPC sockets, protocols, JSON IPC, Unix-socket control, runtime compositor-control APIs, or plugin IPC.

Configuration changes use the configuration file and compositor restart.

## 14. Excluded functionality

Do not implement:

- floating
- animations
- visual effects
- blur/shadows/rounded corners
- bars/panels
- scratchpads
- gestures
- scaling
- IPC
- plugins
- GUI configuration
- built-in launcher
- notifications
- wallpaper daemon
- screensaver
- screen locker
- monitor sleep
- DPMS/power management
- suspend/hibernate
- session/compositor restart
- automatic crash recovery
- volume/brightness controls
- arbitrary compositor keybind actions
- runtime monitor/connector hotplug

The surrounding session infrastructure may restart a crashed compositor; Zimi-WL itself must not.

## 15. Security and reliability

Treat Wayland and X11 clients as untrusted.

Safely handle:

- client-controlled values/sizes
- invalid indexes
- integer overflow/underflow
- allocation failures
- file descriptors
- object lifetimes
- client disconnect/destruction
- workspace changes
- XWayland lifecycle
- pointer-constraint lifecycle
- libseat lifecycle
- shutdown

Avoid use-after-free, double-free, out-of-bounds access, invalid pointers, and avoidable resource leaks.

## 16. Performance

Performance is a priority.

Achieve it primarily through:

- minimal features
- minimal runtime state
- simple data structures
- event-driven operation
- minimal allocations/copies
- minimal background activity
- no unnecessary polling
- no unnecessary threads
- no unnecessary protocols/dependencies

Do not add speculative caches, optimization frameworks, worker threads, or other complexity without demonstrated need.

Do not sacrifice correctness or security for micro-optimizations.

## 17. Testing

Test, where practical:

- compilation
- layout/workspace logic
- configuration parsing
- keybinding dispatch
- input/focus
- startup output handling
- client lifecycle
- XWayland
- pointer constraints/relative pointer
- libseat lifecycle
- error handling
- cleanup

Use appropriate debug builds, sanitizers, static analysis, and fuzzing.

CI must distinguish tests that require a real graphical seat from tests that can run without one. Never report unavailable seat-dependent tests as passed.

## 18. Definition of done

Complete means:

- required technology works
- native Wayland and XWayland work
- xdg-desktop-portal-wlr compatibility requirements are met
- multiple startup-detected outputs work
- workspaces 1–9 work
- switching and focused-window workspace movement work
- focus follows mouse
- pointer locking/confinement and relative pointer work
- all windows use automatic `1/N` equal-width tiling
- moving changes order, not size
- no manual resizing or adjustable ratios
- only the four permitted keybinding actions exist
- configuration is runtime-readable
- no IPC
- exactly one compositor Zig source file and one runtime configuration file exist
- relevant tests pass
- no unnecessary functionality or dependencies were added

## 19. Specification changes

Do not silently remove requirements or add permanent functionality.

If a requirement proves incomplete, incompatible, or ambiguous, document the issue and update this specification before making a major architectural change.