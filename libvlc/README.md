# libVLC as built for Tivvo

Tivvo plays some streams with libVLC (LGPL 2.1 or later). This folder holds everything needed to
rebuild the exact library the app ships, as the LGPL asks.

- VLC source: https://code.videolan.org/videolan/vlc at commit `c833c4be0`
- Wrapper: SwiftVLC 1.0.0 (MIT), https://github.com/harflabs/SwiftVLC/tree/1.0.0
- `patches/0001`–`0006`: SwiftVLC's own patches to VLC.
- `patches/0007-remove-gpl-parts.patch`: Tivvo's change. It removes the VLC modules that are under
  GPL 2+ (`dummy` control, `file_logger`, `syslog`, `rotate`, `sap`, `stream_out_rtp`,
  `stream_out_cycle`), builds without libzvbi, and drops the Yadif deinterlace modes.
- `patches/0008-libtoolize-auxdir-workaround.patch`: build fix only; it changes no shipped code.
- `build-libvlc.sh`: SwiftVLC's build script, changed to build arm64 only and to be safe to re-run.

## Rebuilding

1. Check out SwiftVLC 1.0.0.
2. Replace its `scripts/build-libvlc.sh` and `scripts/patches` with the files here.
3. Run `./scripts/build-libvlc.sh --ios-only`. The result is `Vendor/libvlc.xcframework`.

In the app, libVLC and SwiftVLC live in a separate dynamic framework (`VLCRuntime.framework`), so a
rebuilt library can replace the shipped one.
