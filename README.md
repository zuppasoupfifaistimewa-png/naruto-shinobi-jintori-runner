# Naruto Shinobi Jintori Runner

A public GitHub Actions runner repository for short Linux emulator smoke tests used by the separate private research project.

## Scope and safety

- Runs standard GitHub-hosted Linux runners from this public repository.
- Starts with emulator/package smoke tests only; it does **not** claim that Naruto boots or that Story Mode was completed.
- Never commit game ROMs, BIOS files, save states, memory dumps, screenshots from copyrighted assets, or credentials here.
- The private research repo remains separate: [naruto-shinobi-jintori-research](https://github.com/zuppasoupfifaistimewa-png/naruto-shinobi-jintori-research).
- Use only publicly downloadable emulator builds in the public workflow. Do not create a secret or token to read the private repo unless a later, separate design explicitly requires one.

## Run the smoke test

Open **Actions → PCSX-Redux runner smoke test → Run workflow**, or push a change to its workflow file. The job:
1. Reports the Ubuntu runner environment.
2. Downloads a pinned public AnyLinux PCSX-Redux AppImage.
3. Verifies the published SHA-256.
4. Extracts the AppImage without mounting through FUSE.
5. Attempts a short GUI launch using Xvfb and software rendering; logs are available in the Actions job.

Latest generic smoke-test report: [2026-10-16 smoke-test report](docs/SMOKE_TEST_2026-10-16.md). That smoke test intentionally does not load the ROM. End-to-end testing is handled separately by the secure private-release workflow documented in [docs/PRIVATE_ROM_SETUP.md](docs/PRIVATE_ROM_SETUP.md); the ROM remains outside this public repository.

## Usage and limits

Standard GitHub-hosted runners are free for public repositories, subject to GitHub's technical limits and policies. Each smoke-test job is capped at five minutes to keep its footprint small. See [GitHub Actions billing](https://docs.github.com/en/billing/concepts/product-billing/github-actions) and [runner reference](https://docs.github.com/en/actions/reference/runners/github-hosted-runners).


## Secure private ROM test

Setup/status guide: [docs/PRIVATE_ROM_SETUP.md](docs/PRIVATE_ROM_SETUP.md). The private release assets and `NARUTO_RESEARCH_READ_TOKEN` secret have been verified by a completed cloud run; do not repeat setup unless a later run identifies a real failure. The workflow defaults to smoke-only mode and downloads private media only when dispatched with `run_game=true`.
