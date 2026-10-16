# PCSX-Redux runner smoke test — 2026-10-16

Run: [38900894727](https://github.com/zuppasoupfifaistimewa-png/naruto-shinobi-jintori-runner/actions/runs/38900894727)

## Environment

- Result: success
- Runner OS: Ubuntu 24.04.5 LTS
- Architecture: x86_64
- System glibc: 2.39
- Workflow timeout: five minutes
- GUI smoke test duration: 15 seconds under Xvfb with software rendering

## What passed

1. Public PCSX-Redux AnyLinux AppImage downloaded from the pinned upstream release.
2. SHA-256 verified against the published digest: `c04b3e0a592abbb38bac34fb75551694aaf3a7d4ff18e9fc7b8e2c05bd60fc0c`.
3. AppImage extracted without FUSE; `squashfs-root/AppRun` was present and executable.
4. PCSX-Redux stayed running until the 15-second smoke-test timeout under Xvfb, exit code 124 as expected.
5. The log also contained ALSA warnings because the cloud runner has no physical sound device. Those warnings did not stop the GUI process.

## What this does NOT prove

- No BIOS was loaded.
- No Naruto ROM was loaded.
- No title/menu pixels or input were verified.
- No stability/playthrough/Story Mode test was performed.

This is an emulator package + GUI process smoke test, not a gameplay compatibility test. The next major blocker is provisioning the privately held ROM to a remote runner without putting it in this public repository. The ROM and BIOS must not be committed here.
