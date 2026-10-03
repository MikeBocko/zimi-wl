# Zimi-WL TODO

Work from top to bottom unless dependencies require otherwise. Mark tasks complete only after implementation and relevant testing.

## Foundation

- [ ] Project/build structure
- [ ] Zig + wlroots integration
- [ ] libseat integration without hard-coded backend
- [ ] Compositor initialization
- [ ] Renderer/backend initialization
- [ ] Wayland display/socket
- [ ] Clean shutdown

## Outputs and input

- [ ] Detect/configure outputs present at startup
- [ ] Multiple outputs
- [ ] Keyboard input
- [ ] Pointer input
- [ ] Focus follows mouse
- [ ] Pointer constraints
- [ ] Pointer locking/confinement
- [ ] Relative pointer

## Window management

- [ ] Native Wayland client lifecycle
- [ ] Automatic `1/N` equal-width column tiling
- [ ] Automatic relayout on window changes
- [ ] Window movement within tiling order
- [ ] Focus management
- [ ] Window cleanup

## Workspaces

- [ ] Nine workspaces (1–9)
- [ ] Workspace state
- [ ] Workspace switching
- [ ] Move focused window to workspace
- [ ] Multi-output workspace behavior
- [ ] Relayout after workspace changes
- [ ] Workspace-number movement bindings only

## Configuration and keybindings

- [ ] Runtime configuration format
- [ ] Runtime configuration loading
- [ ] Configurable command-execution bindings
- [ ] Configurable workspace-switch bindings
- [ ] Configurable focused-window-to-workspace bindings
- [ ] Configurable clean focused-window close binding
- [ ] Reject/omit all other action types
- [ ] Default `Mod+1`–`Mod+9` workspace switching
- [ ] Default `Mod+Shift+1`–`Mod+Shift+9` window movement

## XWayland

- [ ] XWayland initialization
- [ ] XWayland client lifecycle
- [ ] XWayland tiling/workspace integration

## Portal

- [ ] xdg-desktop-portal-wlr compatibility requirements

## Security and reliability

- [ ] Client lifetime/error-path validation
- [ ] Allocation/bounds/overflow validation
- [ ] Workspace/window cleanup
- [ ] Pointer-constraint cleanup
- [ ] Relative-pointer cleanup
- [ ] XWayland cleanup
- [ ] libseat cleanup
- [ ] Clean shutdown validation

## Final validation

- [ ] Build succeeds
- [ ] Relevant tests pass
- [ ] Nine workspaces work
- [ ] Workspace switching works
- [ ] Focused-window workspace movement works
- [ ] Automatic `1/N` equal-width tiling only
- [ ] No floating
- [ ] No manual resizing
- [ ] No adjustable ratios
- [ ] Moving windows changes tiling order only
- [ ] Focus follows mouse
- [ ] Pointer locking/confinement works
- [ ] Relative pointer works
- [ ] XWayland works
- [ ] xdg-desktop-portal-wlr requirements satisfied
- [ ] Only four keybinding action types exist
- [ ] No volume/brightness/power/restart/exit actions
- [ ] No IPC
- [ ] No runtime monitor/connector hotplug
- [ ] Exactly one project-owned Zig source file
- [ ] Exactly one runtime configuration file
- [ ] No unnecessary features/dependencies
- [ ] Performance/minimalism requirements reviewed