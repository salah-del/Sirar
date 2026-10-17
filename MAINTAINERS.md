# Publishing a new version

The Unity project is maintained separately from this distribution repository.

1. Set `PlayerSettings.bundleVersion` to a higher three-part version, e.g. `1.0.1`.
2. Keep the updater repository configured as `salah-del/Sirar`.
3. Build and test both Windows applications. Test both correct and retry routes, password lengths, portrait layout, magnifier interactions, and update failure recovery.
4. Package the complete application folders, excluding Unity debug/backup folders. Keep these exact asset names:
   - `PhishingChallenge-windows-x64.zip`
   - `AccountSecurity-windows-x64.zip`
5. Create a **draft** GitHub release with tag `v1.0.1`. Upload both ZIPs plus `SHA256SUMS.txt`.
6. Verify GitHub's SHA-256 asset digests match your local ZIP hashes, then publish the release as the latest stable release. Never publish a partially uploaded release.

The bundled updater checks stable releases only. It does not install a version equal to or older than the installed version. GitHub account credentials are never included in the applications.

Before broader deployment, test an actual previous-build-to-new-build update on a Windows device with the intended installation permissions. Automated helper tests do not replace this deployment check.
