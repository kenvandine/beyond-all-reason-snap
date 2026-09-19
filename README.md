# beyond-all-reason-snap

Snap packaging for [Beyond All Reason](https://www.beyondallreason.info/)
(BAR), a free and open-source real-time strategy game built on the Recoil
(Spring RTS) engine with fully open assets.

## What this packages

This snap packages the official
[BAR Lobby](https://github.com/beyond-all-reason/bar-lobby) Electron
launcher/client, extracted from its upstream AppImage release. BAR Lobby
handles login, matchmaking, and downloading/updating the Recoil engine and
game content (maps, textures, etc.) at runtime — those are updated far more
often than the lobby itself, so packaging the launcher (rather than freezing
an engine/content version) is the approach the upstream project itself
recommends and is the most maintainable long-term.

Because Electron/Chromium's setuid sandbox helper (`chrome-sandbox`) can't
run inside a strict-mode snap, it is removed at build time and the app is
launched with `--no-sandbox` via a small wrapper script
(`snap/local/bar-lobby-wrapper`).

## Interfaces

`opengl`, `wayland`, `x11`, `audio-playback`, `joystick`, `network`,
`network-bind`, `home`, `removable-media`.

## Building locally

```bash
snapcraft --use-lxd
```

To update to a newer BAR Lobby release, bump the `version:` field in
`snap/snapcraft.yaml` to match the desired
[bar-lobby release tag](https://github.com/beyond-all-reason/bar-lobby/releases).

## Testing on a handheld device

```bash
scp beyond-all-reason_*.snap deck@handheld:/tmp/
ssh deck@handheld
sudo snap install --dangerous /tmp/beyond-all-reason_*.snap
sudo snap connect beyond-all-reason:joystick
```

First launch requires network access for login and to download the Recoil
engine + game content.

## QA checklist

- [ ] Gamepad navigates menus; analog sticks register as axes, not buttons
- [ ] Fullscreen Wayland handoff works on the handheld's native panel/orientation
- [ ] Audio survives suspend/resume
- [ ] Engine/content download completes and a skirmish/game launches

## Status

Not yet registered in the Snap Store. This repo is for local
building/testing only — publishing/registration will be handled separately.
