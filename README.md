# vesktop-patched-git

This package provides a patched version of Vesktop for Arch Linux, built to improve Linux audio integration and user experience.

## What is vesktop-patched-git?

`vesktop-patched-git` is a custom build of [Vesktop](https://github.com/Vencord/Vesktop), a standalone Electron-based Discord client with Vencord, using a custom Electron build (`electron-vesktop`) that includes patches for better identification in Linux audio management tools.

## Why is this needed?

By default, Electron-based applications on Linux do not have unique names or icons in PipeWire and PulseAudio streams. This causes all Electron apps to appear as "Chromium" in audio management applications like pavucontrol, making it impossible to set different default volumes or audio devices for each app. The underlying issue is described in [electron/electron#27581](https://github.com/electron/electron/issues/27581). The custom Electron build used here patches Chromium's `pulse_util.cc` to set a unique name and icon for Vesktop, allowing proper per-app audio management.

## How is it built?

- The PKGBUILD depends on `electron-vesktop`, a custom Electron build with the necessary patches applied (see the [electron-vesktop repository](https://github.com/Lungooo/electron-vesktop) for details).
- The PKGBUILD fetches the latest Vesktop source from GitHub and builds it using the system's patched Electron.
- The build process ensures that Vesktop uses the custom Electron binary, so it appears correctly in audio management tools.

## References

- [electron-vesktop](https://github.com/Lungooo/electron-vesktop)
- [electron/electron#27581](https://github.com/electron/electron/issues/27581)
- [Vesktop GitHub](https://github.com/Vencord/Vesktop)

---

Maintainer: Sophie Langer <sophie@lungoo.dev>
