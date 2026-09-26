# ESPOTA

This repository contains the ESP32 OTA firmware and update manifest.

## Build and publish a firmware update

1. Push changes to `main/main.ino` or `main/app.ino` on the `main` branch.
2. Open the repository's **Actions** tab and wait for **Build ESP32 firmware** to finish.
3. The workflow automatically increments the patch version, updates the manifest, creates the matching GitHub Release, and attaches `firmware.bin`.
4. Open the release to confirm it completed. The workflow artifact `firmware-vX.Y.Z` is also available to download for 30 days.

The workflow builds the sketch, increments the patch version in `firmware/manifest.json`, calculates the binary size and SHA-256, commits the updated manifest, creates the matching GitHub Release, and uploads the exact binary as `firmware.bin`. It also saves the binary as a downloadable workflow artifact. The manifest URL points to the automatically created release.

The workflow needs permission to push the manifest update. In GitHub, check **Settings > Actions > General > Workflow permissions** and enable **Read and write permissions** if the workflow cannot push its commit.

## Reuse the OTA system in another project

For a new repository such as `HumanPresence`, copy these from this repository:

- `main/` (shared `main.ino` plus an `app.ino` to replace with the new application)
- `.github/workflows/build-firmware.yml`
- `firmware/manifest.json`

In the new `main/main.ino`, change only `OTA_PROJECT_NAME` to the new repository name (for example, `HumanPresence`). `OTA_GITHUB_OWNER` is already set to `smarthomebyvinay`; change it only if the new repository belongs to a different GitHub account. Keep `OTA_DEVICE_MODEL` as `esp32dev` for the same ESP32 device family, and keep that value in the new manifest's `device` field. Initialize the manifest version to `0.0.0`; the first successful workflow build will create version `0.0.1`. The workflow builds and releases in the repository where it is installed.

Implement the product behavior in `main/app.ino` with `app_setup()` and `app_loop()`. Keep the shared OTA `setup()`, `loop()`, Wi-Fi, and web update code in `main/main.ino`. Push the new repository's `main` branch; subsequent changes under `main/` trigger an automatically versioned build, manifest update, and release for that project.
