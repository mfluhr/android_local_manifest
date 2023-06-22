# Local Manifest for building Android 12

This local manifest can be used with the _android-12.0.0_r32_ release tag (Build ID _SQ1D.220205.004_) when building Android on a workstation running a modern Linux distribution.

It contains the ACME emulators `acme_x86_64` and `acme_car_x86_64`, for both classic Phone & Tablet Android and Android Automotive, that will be used as base for adding and/or replacing components and features.

## Usage

```text
repo init -u https://android.googlesource.com/platform/manifest -b android-12.0.0_r32
git clone https://github.com/mfluhr/android_local_manifest.git -b android-12.0.0_r32-acme .repo/local_manifests
repo sync
```

_Please note that the manifest URL used when calling `repo init` can be replaced with a local mirror URL and the `repo sync` options can be tailored to the build environment using among others the `-j` and `-c` options to speed up the download and sync process._
