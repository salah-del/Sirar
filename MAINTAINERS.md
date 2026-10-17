# Publishing updates only

The Unity project is maintained separately from this distribution repository.

1. Increase PlayerSettings.bundleVersion, build both apps, and verify the change.
2. Run Tools/PackageRelease.ps1 -Version <new>. Complete ZIPs stay LOCAL as reference builds. By default the script generates patches from every retained compatible release baseline (1.0.2 onward), so skipping a release still works. Keep those exact baseline packages; do not silently omit supported installed versions.
3. Run Tools/TestReleasePatch.ps1 for each baseline against the new version. Every patched file must match the local complete target package.
4. Create a draft GitHub release. Run Tools/PublishRelease.ps1 -Version <new>. It uploads ONLY patch ZIPs, their SHA256SUMS and the release-update.json manifest. It refuses a draft containing full application ZIPs. It verifies hashes before publication and updates the legacy branch manifest afterward.
5. Verify automatic discovery and compatible patch selection in a running previous client.

## First installation

README download buttons deliberately point to complete release 1.0.3, the retained starter. After first launch its updater downloads a compatible patch to the latest release. Never change these buttons to latest/download/full.zip: update-only releases have no full ZIPs. Never delete the starter release or its assets.

Clients 1.0.0/1.0.1 only understand full update packages; they need the starter download once. Clients 1.0.2 use the branch manifest, and 1.0.3 onward use the manifest attached to the latest release. Keep patches for 1.0.2 and 1.0.3, as well as all later supported versions.

## Verification and security

The updater polls about every 10 seconds; this is not instantaneous push delivery. Patch installers validate SHA-256 for the archive and baseline files, preserve unrelated files, and back up replacements for rollback. Test compatibility and actual installed-player updates before broader deployment. No GitHub credentials are bundled.

Current executables are unsigned. Removing SmartScreen warnings is not a Unity setting; trusted publisher signing requires an external certificate/service and does not guarantee instant reputation. See https://learn.microsoft.com/en-us/windows/apps/package-and-deploy/smartscreen-reputation .
