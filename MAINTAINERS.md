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

## Incremental updates and fast discovery (1.0.2+)

Build complete packages for new installs and old updaters, then generate file-level patches from each supported baseline's *exact published ZIP*. Local command: `Tools/PackageRelease.ps1 -Version <new> -BaseVersion <old>`. `Tools/package_patch.py` can add additional supported baselines. Include all patch ZIPs in the release and regenerate SHA256SUMS for every ZIP.

Publish both full packages and all intended patches as one verified release. Then run `Tools/PublishUpdateManifest.ps1 -Version <new>` to update `update.json` in this repository. `Tools/PublishRelease.ps1` does both steps automatically. Never publish a manifest pointing to incomplete, draft or unverified assets.

The clients read the lightweight manifest about every ten seconds with cache revalidation and retry backoff. They do not call the releases REST API at that frequency, because its [unauthenticated limit is 60 requests/hour per IP](https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api). Hosting and network delays still apply; this is not a push-notification service.

Patch installers validate all original packaged files against SHA-256 before changing anything. Modified or mismatched installations are rejected; unrelated user files are preserved. Test real patch application, removed files, base mismatch, corruption, rollback, and equality with the complete target package before publishing.
