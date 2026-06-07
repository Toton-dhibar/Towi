# Android 10 Rootfs Guide (Open-Source GSI)

This guide explains how to prepare a Towi-compatible Android 10 rootfs from open-source GSI images.

## Prerequisites

- Linux host with `simg2img`, `mount`, `cp`, `tar`, and `7z`
- A legally distributable Android 10 GSI image (AOSP/Lineage-based builds only)
- A working Towi installation on your device

## 1) Get an Android 10 GSI image

Use an open-source Android 10 GSI release and download the `system.img` (or compressed package containing it).

## 2) Convert and extract `system.img`

1. If sparse, convert image:
   - `simg2img system.img system_raw.img`
2. Mount the raw image read-only.
3. Copy mounted contents into a folder named `rootfs`.

The final folder should contain Android system paths such as:
- `rootfs/system`
- `rootfs/vendor` (if present in your source)
- `rootfs/init` (or equivalent init layout used by your base image)

## 3) Add required Towi metadata

Create `rom.ini` in the root of your archive content:

- `author=<your_name_or_project>`
- `version=<android10-build-id>`
- `code=<incrementing_number>`
- `desc=<short description>`

## 4) Package for import

Package the rootfs as either:

- `rootfs.tar.gz` (recommended for compatibility), or
- `rootfs.7z`

Keep the archive top-level layout clean (no extra parent directory wrappers unless intended).

## 5) Import into Towi

1. Open Towi Settings.
2. Use **Import Rootfs**.
3. Select your `rootfs.tar.gz` or `rootfs.7z`.
4. Wait for extraction to finish, then reboot Towi.

## 6) Validate boot

After reboot:

- Confirm Android boots successfully.
- Check app/system stability.
- If boot fails, re-check archive layout and `rom.ini` values.

## Notes

- Prefer open-source GSIs that are known to boot with containerized userspace setups.
- Keep a backup profile before testing a new rootfs build.
- Do not redistribute proprietary ROM contents without permission.
