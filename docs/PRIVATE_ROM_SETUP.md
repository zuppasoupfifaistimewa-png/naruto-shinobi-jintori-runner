# Secure private ROM provisioning and cloud test status

**Status checked:** 2026-10-17

This workflow keeps the Naruto research repository private and downloads game media only into an ephemeral GitHub-hosted runner. The ROM, CUE, diagnostic OpenBIOS, emulator extraction, settings profile, captured frames, and logs are not committed to this public runner repository.

## Current configuration — verified, do not repeat setup

The private research release is:

https://github.com/zuppasoupfifaistimewa-png/naruto-shinobi-jintori-research/releases/tag/research-emulator-pcsx-redux-2026-10-15

The assets expected by the workflow are already present with the following normalized names:

- `Naruto.-.Shinobi.no.Sato.no.Jintori.Gassen.Japan.bin`
- `Naruto.-.Shinobi.no.Sato.no.Jintori.Gassen.Japan.cue`
- `openbios-chunked.bin`
- `PCSX-Redux-HEAD-x86_64.AppImage`

The release API and the completed runner job verified that the required assets were found. The latest completed boot-test run also verified the ROM's expected size (325,989,552 bytes) and SHA-256 (`58c389a794375b7b81b3a18877ef7965356a1332285a3590170efbf2359a3b88`). The CUE was structurally checked and normalized only in a temporary copy.

The public runner Actions log confirms that the secret `NARUTO_RESEARCH_READ_TOKEN` is present; its value was not printed. Do not paste tokens or secrets into chat, workflow code, issues, or commits. Do not re-upload ROM/CUE or recreate the secret unless a later test explicitly shows a real configuration failure.

## Run the cloud boot test

Workflow:

https://github.com/zuppasoupfifaistimewa-png/naruto-shinobi-jintori-runner/actions/workflows/naruto-private-rom-test.yml

Choose **Run workflow** and set `run_game = true`. The job verifies the pinned AnyLinux PCSX-Redux build, downloads ROM/CUE/OpenBIOS from the private release using the read-only secret, checks the ROM size and hash, then runs bounded headless and GUI probes.

- `run_game = false` is only a short emulator GUI smoke test; it does not download game media.
- The current GUI probe captures the PCSX-Redux window itself when it can find it, not the Xvfb root background. It records image metrics and OCR text in the job log without publishing screenshot files.
- The workflow has a bounded runtime. Its cleanup step removes private game files, BIOS, emulator trees, profiles, temporary frames, and logs from the ephemeral runner.
- Do not commit ROM, BIOS, save states, memory dumps, screenshots, or credentials to this repository.

## What the current evidence means

Completed test: [Actions #38975931679](https://github.com/zuppasoupfifaistimewa-png/naruto-shinobi-jintori-runner/actions/runs/38975931679).

That run verified the release downloads, ROM size/hash, BIOS recognition, disc label `SLPS-03553`, the `SYSTEM.CNF` boot path, completion of 187 executable chunks, and the `EXEC:PC0(80011448)` / `Execute !` markers. Port 1 was set to Analog and the previous `Unknown command for pad: 43` warning no longer appeared. This proves that the loader transferred control to the game executable in the headless probes; it does **not** prove the title/menu appeared or that input/gameplay works.

The previous GUI image analysis captured the Xvfb root rather than the emulator window, so its all-black metrics were not sufficient to describe the game's rendered output. A corrected run is in progress:

- [Actions #39000276365 — corrected GUI window capture](https://github.com/zuppasoupfifaistimewa-png/naruto-shinobi-jintori-runner/actions/runs/39000276365)
- [Actions #39000257139 — source-built CPU trace with runtime dependency check](https://github.com/zuppasoupfifaistimewa-png/naruto-shinobi-jintori-runner/actions/runs/39000257139)

Do not begin Story Mode rule experiments until a recognizable title/menu and a controlled input response are evidenced. A passing shell job, live emulator process, or `EXEC:PC0` marker alone is not enough.
