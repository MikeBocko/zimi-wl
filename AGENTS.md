# Zimi-WL Agent Instructions

1. Read `SPEC.md` and `TODO.md` before working.
2. `SPEC.md` is authoritative.
3. Complete the next unfinished `TODO.md` task, test it, then update `TODO.md`.
4. Keep implementation minimal; do not add unspecified features.
5. Performance is a priority, achieved primarily through minimalism: simple data structures, event-driven operation, minimal allocations/work, and few dependencies. Do not add speculative optimization machinery.
6. Use Zig, wlroots, libseat, XWayland, and xdg-desktop-portal-wlr.
7. Use libseat without hard-coding a backend. Do not implement custom seat management.
8. The compositor consists of exactly one project-owned Zig source file and one runtime configuration file. Do not create additional compositor source/config/layout/keybinding files.
9. Configuration must be runtime-readable; changing bindings must not require recompilation.
10. Required behavior includes automatic equal-width column tiling, workspaces 1–9, multiple startup-detected outputs, focus-follows-mouse, pointer constraints/locking, relative pointer support, native Wayland, XWayland, and xdg-desktop-portal-wlr compatibility.
11. Tiling is always automatic: N visible windows on an output each occupy `1/N` of the available width. No floating, manual resizing, adjustable ratios, master/stack layouts, or per-window sizing.
12. Moving a window means changing its tiling order, not changing its size.
13. There is no separate keyboard focus-navigation system.
14. Workspace switching and moving the focused window are separate operations. Workspace movement is only through workspace-number keybindings.
15. Only these keybinding actions exist:
    - execute a configured command, with any number of such bindings
    - switch to workspace 1–9
    - move focused window to workspace 1–9
    - close the focused window cleanly
16. No keybindings/actions for volume, brightness, restart, exit, power management, DPMS, fullscreen, floating, resizing, ratios, gaps, focus navigation, scratchpads, special workspaces, gestures, or IPC.
17. Default workspace bindings should be:
    - `Mod+1` … `Mod+9`: switch workspace
    - `Mod+Shift+1` … `Mod+Shift+9`: move focused window to workspace
18. Command bindings should execute trusted configured commands, analogous to dwl `spawn` / Hyprland `exec`.
19. Closing a window should use the appropriate normal Wayland/XWayland close mechanism, analogous to dwl `killclient` / Hyprland `killactive`.
20. Do not implement compositor restart, self-restart, automatic crash recovery, power management, screensaving, or locking.
21. Support multiple outputs detected at compositor startup. Runtime monitor/connector addition or removal is not required and should not be implemented merely for completeness.
22. Treat clients as untrusted. Carefully handle lifetimes, disconnects, allocations, bounds, file descriptors, workspaces, XWayland, pointer constraints, libseat, and shutdown.
23. Use debugging, sanitizers, static analysis, fuzzing, and appropriate tests where practical.
24. Never claim a test passed if it was not actually run. Clearly distinguish seat-dependent tests from CI tests that can run without a real graphical seat.
25. Do not introduce unnecessary dependencies, protocols, threads, polling, background work, abstractions, or services.