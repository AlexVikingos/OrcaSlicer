# Snapmaker Orca — Catalina Intel build kit

This kit adds a GitHub Actions workflow that builds Snapmaker Orca as an **Intel
x86_64 macOS app** with a **macOS 10.15 (Catalina)** deployment target.

## Why this approach

The uploaded source is Snapmaker Orca. Its CMake configuration defaults to
macOS 12, but the project's own macOS CI explicitly builds with `-t 10.15`.
This workflow follows that route on a modern macOS runner.

I cannot compile a genuine macOS `.app` in this Linux environment. The workflow
builds it on GitHub's macOS machine instead, so you do not need Homebrew, CMake,
or Xcode on your Catalina Mac.

## Steps

1. Open https://github.com/Snapmaker/OrcaSlicer and click **Fork**. This preserves
   the repository's submodule metadata, which is needed for a successful build.
2. In your fork, create the file
   `.github/workflows/build-catalina.yml`.
3. Copy the contents of the `build-catalina.yml` included in this kit into it,
   then commit the file.
4. Open the **Actions** tab in your fork. If prompted, enable GitHub Actions.
5. Select **Build Snapmaker Orca for macOS Catalina** → **Run workflow**.
6. When it finishes, open that run and download the artifact
   `Snapmaker-Orca-Catalina-Intel`.
7. Unzip it and move `Snapmaker Orca.app` to `/Applications`.

## First launch on Catalina

If Gatekeeper blocks the app, Control-click it, choose **Open**, and confirm.
Alternatively, in Terminal run:

```bash
xattr -dr com.apple.quarantine "/Applications/Snapmaker Orca.app"
```

## Compatibility notes

- Catalina runs on Intel Macs, so this workflow builds `x86_64`. Apple Silicon
  Macs cannot run Catalina.
- The app is built with a 10.15 deployment target. A dependency or source change
  could still introduce a newer runtime requirement, so test it on Catalina.
- Building takes a while and requires substantial free disk space.
- The artifact is not Developer ID signed or notarized, so Gatekeeper may warn.
