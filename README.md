# Local Manifest for building Android 13

This local manifest can be used with the _android-13.0.0_r52_ release tag (Build ID _TQ3A.230605.012_) when building Android on a workstation running a modern Linux distribution.

## Usage

```text
repo init -u https://android.googlesource.com/platform/manifest -b android-13.0.0_r52
git clone https://github.com/mfluhr/android_local_manifest.git -b android-13.0.0_r52 .repo/local_manifests
repo sync
```

_Please note that the manifest URL used when calling `repo init` can be replaced with a local mirror URL and the `repo sync` options can be tailored to the build environment using among others the `-j` and `-c` options to speed up the download and sync process._
