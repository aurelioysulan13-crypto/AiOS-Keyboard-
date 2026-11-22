# AyOS Keyboard ZIP-to-APK Automation

This repository is a separate build runner for the AyOS Keyboard v3.85 source ZIP. It does not modify the original `Keyboard` repository.

## What it does

1. Watches for `AyOS-Keyboard-repo-root-v3.85.zip` uploaded to the repository root.
2. Safely extracts the Android project into a temporary GitHub Actions workspace.
3. Sets up Java 17 and Gradle 8.9.
4. Runs `gradle assembleDebug --no-daemon`.
5. Uploads the resulting debug APK as the `AyOS-Keyboard-v3.85-APK` workflow artifact.

The source ZIP is not extracted into or committed back to this automation repository. The APK is stored as a GitHub Actions artifact for 14 days.

## One-time setup

1. Create a new **private** GitHub repository named `AyOS-Keyboard-ZIP-Automation`.
2. On your phone or computer, download this setup ZIP and extract it.
3. Upload the contents of the extracted `ayos_zip_automation_setup` folder to the root of the new GitHub repository. Make sure the `.github/workflows/build-from-zip.yml` file is included. If GitHub's upload page hides `.github`, create that workflow file manually using the supplied file.
4. Commit the files to the `main` branch.
5. Upload `AyOS-Keyboard-repo-root-v3.85.zip` to the root of this new repository and commit it. Keep the filename exactly as written.
6. Open the repository's **Actions** tab. Select **Build AyOS APK from Uploaded ZIP** and open the run.
7. When the run completes successfully, open the run, scroll to **Artifacts**, and download **AyOS-Keyboard-v3.85-APK**.
8. Extract the downloaded artifact ZIP. It contains `app-debug.apk`.

## Run it again

To rebuild the same source ZIP, open **Actions** → **Build AyOS APK from Uploaded ZIP** → **Run workflow**.

To build a different source snapshot, replace the root ZIP with the new archive and update the watched filename in `.github/workflows/build-from-zip.yml` if the filename changes.

## Build details and limitations

- JDK: 17
- Gradle: 8.9
- Build task: `assembleDebug`
- Expected APK: `app/build/outputs/apk/debug/app-debug.apk`
- This produces a debug APK, not a Play Store release APK.
- The v3.85 source notes say its Gradle build had not yet been run. A successful workflow run is required to confirm compilation; this automation does not guarantee that the app source itself is error-free.
- Android debug APKs built with different signing keys may not update an existing installation. If Android rejects an update because of a signature mismatch, uninstall the older app before installing the new debug APK.
