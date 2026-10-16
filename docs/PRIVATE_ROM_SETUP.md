# Secure private ROM provisioning for the public runner

This setup keeps the Naruto research repository private and prevents the ROM from entering Git history or the public runner repository. **The ROM itself is not yet present as an asset in the private release**, and the workflow cannot work in `run_game=true` mode until the two remaining GitHub UI steps below are completed.

## 1. Upload two files to the existing private research release

Open the existing release in the private repository:

https://github.com/zuppasoupfifaistimewa-png/naruto-shinobi-jintori-research/releases/tag/research-emulator-pcsx-redux-2026-10-15

Use **Edit release**, then attach these files as release assets. Keep the repository private.

| Required asset name | Source / verification |
|---|---|
| `Naruto - Shinobi no Sato no Jintori Gassen (Japan).bin` | ROM already in the user's Library/local copy; expected size 325,989,552 bytes; expected SHA-256 `58c389a794375b7b81b3a18877ef7965356a1332285a3590170efbf2359a3b88` |
| `naruto-shinobi-jintori.cue` | Matching CUE sheet from the research files; must reference the ROM image above |

The release already contains `openbios-chunked.bin`, so do not upload the BIOS again. Do not upload the ROM or CUE to this public runner repository. A private release is still a hosted copy of copyrighted game media; only upload it if you have the right to make that private copy and store it there.

The currently available GitHub connector in this ChatGPT session cannot upload binary release assets. That upload must be completed in the GitHub UI by a repository owner. Uploading from the laptop browser is only for transfer/storage; the emulator itself will run on the cloud runner, not on the laptop.

## 2. Create a fine-grained read-only token

Create a fine-grained personal access token at:

https://github.com/settings/personal-access-tokens/new

Recommended settings:

- Resource owner: `zuppasoupfifaistimewa-png`
- Repository access: **Only select repositories** → `naruto-shinobi-jintori-research`
- Repository permissions: **Contents: Read-only** (Metadata is included as a required read permission)
- Choose an expiration date and keep the token private.

This permission is sufficient for reading private release metadata and downloading release assets through the GitHub REST API. Do not use a classic token or a token with write/admin permissions. Never paste the token into chat, workflow YAML, an issue, or a commit.

## 3. Add the token as a secret to the public runner repository

Open:

https://github.com/zuppasoupfifaistimewa-png/naruto-shinobi-jintori-runner/settings/secrets/actions/new

Set:

- Name: `NARUTO_RESEARCH_READ_TOKEN`
- Secret: paste the fine-grained token value

Only trusted maintainers should have write access to the public runner repository. Anyone with write access can modify a workflow that receives this secret, so keep that access limited. The token itself is read-only and scoped to the private Naruto research repository.

## 4. Run the cloud test

Open:

https://github.com/zuppasoupfifaistimewa-png/naruto-shinobi-jintori-runner/actions/workflows/naruto-private-rom-test.yml

Choose **Run workflow**:
- `run_game = false`: runs a short GUI smoke test without ROM or BIOS.
- `run_game = true`: downloads the exact ROM, matching CUE and existing OpenBIOS from the private release, checks ROM size/SHA-256, then attempts a bounded boot for up to 90 seconds.

The workflow runs on an ephemeral GitHub-hosted Linux runner. It removes the local ROM, CUE, BIOS, profile and logs after the job. It does not upload ROM, screenshots, save states or RAM dumps as artifacts. GitHub logs include only selected diagnostic lines, not a full raw memory dump.

## 5. What a pass means

Even if the boot workflow passes its shell commands, that does **not** automatically prove the game reached a visible title/menu or that controls work. Inspect the boot markers first, then add a controlled screenshot/pixel or debugger-state check that does not publish game images. Only after boot and input are confirmed should Story Mode tests begin.

See the workflow here: `.github/workflows/naruto-private-rom-test.yml`.
