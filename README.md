# ESPOTA

This repository contains the ESP32 OTA firmware and update manifest.

## Build and publish a firmware update

1. Push changes to `main/main.ino` or `main/app.ino` on the `main` branch.
2. Open the repository's **Actions** tab and wait for **Build ESP32 firmware** to finish.
3. The workflow automatically increments the patch version, updates the manifest, creates the matching GitHub Release, and attaches `firmware.bin`.
4. Open the release to confirm it completed. The workflow artifact `firmware-vX.Y.Z` is also available to download for 30 days.

The workflow builds the sketch, increments the patch version in `firmware/manifest.json`, calculates the binary size and SHA-256, commits the updated manifest, creates the matching GitHub Release, and uploads the exact binary as `firmware.bin`. It also saves the binary as a downloadable workflow artifact. The manifest URL points to the automatically created release.

The workflow needs permission to push the manifest update. In GitHub, check **Settings > Actions > General > Workflow permissions** and enable **Read and write permissions** if the workflow cannot push its commit.
