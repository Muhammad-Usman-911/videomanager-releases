# Video Manager — Releases

This repository holds **only the installers** for the Video Manager desktop app. The source code is kept
elsewhere. Installed apps check this repository's [Releases](../../releases) page to find and download updates.

> ⚠️ **This repository is public.** Anyone can see what's uploaded here. Upload only the installer files
> listed below. Never put passwords, `.env` files, IP addresses, settings, logs, screenshots or
> recordings here: not in releases, release notes, issues or commits.

---

## Download

Go to **[Releases → Latest](../../releases/latest)** and download:

| Computer | File | Auto-updates? |
|---|---|---|
| Linux (Ubuntu) | `VideoManager-X.Y.Z-x86_64.AppImage` | ✅ Yes (recommended) |
| Linux (Ubuntu) | `VideoManager-X.Y.Z-amd64.deb` | ❌ No, install new versions by hand |
| Windows | `VideoManager-Setup-X.Y.Z.exe` | ✅ Yes |

The installation steps are in the app's setup guide.

---

## How versions work

- Every release has a **tag** like `v1.0.0`, which is `v` + the app version.
- The tag **must match the app's version exactly**. The installer `VideoManager-1.0.0-…` belongs to tag
  `v1.0.0`. If they don't match, the updater gets confused.
- Version numbers go **up only**: `1.0.0 → 1.0.1` (fixes) → `1.1.0` (new features) → `2.0.0` (big changes).
  The app only updates to a **higher** version than the one installed.
- Installed apps look at the **latest published release** (not drafts, not pre-releases). They check
  when they start, every 6 hours, and when someone clicks *Settings → Check for updates*.

---

## How to publish a new version

### 1. Get the files

The developer builds the installers. They come out of the build together, with these names:

| File | What it's for |
|---|---|
| `VideoManager-X.Y.Z-x86_64.AppImage` | Linux app |
| `latest-linux.yml` | **Needed for Linux auto-update** |
| `VideoManager-X.Y.Z-amd64.deb` | Linux manual install |
| `VideoManager-Setup-X.Y.Z.exe` | Windows installer |
| `VideoManager-Setup-X.Y.Z.exe.blockmap` | Smaller Windows updates |
| `latest.yml` | **Needed for Windows auto-update** |

> ❗ **Don't rename any file.** The `latest*.yml` files list the other files by exact name and checksum.
> A renamed or re-zipped file breaks updates. Upload the `.yml` files from the **same build** as the
> installers.

### 2. Create the release on GitHub

1. Open this repository on GitHub and click **Releases** (right side) → **Draft a new release**.
2. **Choose a tag** → type the new version, e.g. **`v1.0.0`** → click **Create new tag: v1.0.0 on publish**.
   Target: `main`.
3. **Release title:** `v1.0.0`. In the description, say in plain words what changed (e.g. "Fixed a download error").
   Keep internal details out of it.
4. **Attach binaries:** drag all the files from step 1 into the box and wait until every upload finishes.
5. Leave **Set as a pre-release** unticked. Tick **Set as the latest release**.
6. Click **Publish release**.

Until you click *Publish*, it's a draft and installed apps can't see it.

### 3. Check the release

- The release page shows the tag `v1.0.0`, marked **Latest**, with all files listed.
- This link opens or downloads a small text file whose first line is `version: 1.0.0`:
  `https://github.com/Muhammad-Usman-911/videomanager-releases/releases/latest/download/latest-linux.yml`
  - A **404** means the release isn't published or `latest-linux.yml` wasn't uploaded.
  - The wrong version means another release is marked *Latest*.

---

## How to test that updates work

You need **two** versions for this: an older one installed and a newer one published.

1. **Install the old version** (e.g. `v1.0.0`) on a test PC: download the AppImage, then run it.
2. Open the app → **Settings**. The version shows **1.0.0**.
3. **Publish a newer version** (e.g. `v1.0.1`) as described above.
4. In the app click **Settings → Check for updates**. You should see these messages in order:
   1. `Version 1.0.1 found — downloading…`
   2. `Downloading update… NN%`
   3. A blue bar: **"Version 1.0.1 is ready."**
5. Click **Restart to update** (with no download running). The app closes and opens again.
6. **Settings** now shows **1.0.1**. ✅ Updates work.

**Without clicking:** if the app sits in the tray with nothing downloading, it installs the update by
itself and comes back in the tray.

### If it doesn't work

| What you see in Settings | Meaning | Fix |
|---|---|---|
| "You have the latest version." | The app didn't find a higher version | Check that the release is published, marked *Latest*, and its tag is higher than the installed version |
| "No public release found on GitHub yet" | No published release with `latest-linux.yml` / `latest.yml` | Publish the release and upload the `.yml` files |
| "Update check failed: …sha512 checksum mismatch…" | A file was renamed, re-uploaded or mixed from two builds | Delete the release files and upload all files from **one** build again |
| "Updates only work in the installed app" | You're running a developer build | Test with the downloaded AppImage / exe |
| The `.deb` never updates | Expected | Use the AppImage for auto-updates |

The app's own log also shows the update steps: open the **Logs** page → *All activity* and look for lines
starting with **"Update"**.

---

## Fixing a bad release

- **A file is wrong or missing:** edit the release, delete the file, upload the correct one from the same build.
- **The version itself is broken:** publish a **new, higher** version with the fix (e.g. `v1.0.2`).
  Don't reuse or move an existing tag, because installed apps already treat that version as done.
- Delete a release only if no one has installed it yet.
