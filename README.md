# Local Manifest for building Android 11

This local manifest can be used with the _android-11.0.0_r46_ release tag (Build ID _RQ3A.211001.001_) when building Android on a workstation running a modern Linux distribution.

## Usage

```text
repo init -u https://android.googlesource.com/platform/manifest -b android-11.0.0_r46
git clone https://github.com/mfluhr/android_local_manifest.git -b android-11.0.0_r46 .repo/local_manifests
repo sync
```

_Please note that the manifest URL used when calling `repo init` can be replaced with a local mirror URL and the `repo sync` options can be tailored to the build environment using among others the `-j` and `-c` options to speed up the download and sync process._
