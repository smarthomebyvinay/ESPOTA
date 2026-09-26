# ESPOTA

This repository contains the ESP32 OTA firmware and update manifest.

## Build and publish a firmware update

1. Push changes to `main/main.ino` or `main/app.ino` on the `main` branch.
2. Open the repository's **Actions** tab and wait for **Build ESP32 firmware** to finish.
3. The workflow builds only if `OTA_FIRMWARE_VERSION` in `main/main.ino` changed compared with the previous push. It uses that version, updates the manifest, creates the matching GitHub Release, and attaches `firmware.bin`.
4. Open the release to confirm it completed. The workflow artifact `firmware-vX.Y.Z` is also available to download for 30 days.

Before each firmware release, change `OTA_FIRMWARE_VERSION` in `main/main.ino` to a new `MAJOR.MINOR.PATCH` value (for example, `0.1.7`) and push the change. Changes to `main/` without a version change will skip building and releasing. The workflow builds the sketch, calculates the binary size and SHA-256, commits the updated manifest, creates the matching GitHub Release, and uploads the exact binary as `firmware.bin`. It also saves the binary as a downloadable workflow artifact. The manifest URL points to the automatically created release.

The workflow needs permission to push the manifest update. In GitHub, check **Settings > Actions > General > Workflow permissions** and enable **Read and write permissions** if the workflow cannot push its commit.

## Reuse the OTA system in another project

For a new repository such as `HumanPresence`, copy these from this repository:

- `main/` (shared `main.ino` plus an `app.ino` to replace with the new application)
- `.github/workflows/build-firmware.yml`
- `firmware/manifest.json`

In the new `main/main.ino`, change only `OTA_PROJECT_NAME` to the new repository name (for example, `HumanPresence`). `OTA_GITHUB_OWNER` is already set to `smarthomebyvinay`; change it only if the new repository belongs to a different GitHub account. Keep `OTA_DEVICE_MODEL` as `esp32dev` for the same ESP32 device family, and keep that value in the new manifest's `device` field. Set `OTA_FIRMWARE_VERSION` and the initial manifest version to `0.0.0`; change the code version to `0.0.1` for the first build. The workflow builds and releases in the repository where it is installed, and only when the code version changes.

Implement the product behavior in `main/app.ino` with `app_setup()` and `app_loop()`. Keep the shared OTA `setup()`, `loop()`, Wi-Fi, and web update code in `main/main.ino`. Push the new repository's `main` branch; subsequent changes under `main/` trigger an automatically versioned build, manifest update, and release for that project.
