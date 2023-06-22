# Local Manifest for building Android 10

This local manifest can be used with the _android-10.0.0_r41_ release tag (Build ID _QQ3A.200805.001_) when building Android on a workstation running a modern Linux distribution.

It contains the ACME emulators `acme_x86_64` and `acme_car_x86_64`, for both classic Phone & Tablet Android and Android Automotive, that will be used as base for adding and/or replacing components and features.

## Usage

```text
repo init -u https://android.googlesource.com/platform/manifest -b android-10.0.0_r41
git clone https://github.com/mfluhr/android_local_manifest.git -b android-10.0.0_r41-acme .repo/local_manifests
repo sync
```

_Please note that the manifest URL used when calling `repo init` can be replaced with a local mirror URL and the `repo sync` options can be tailored to the build environment using among others the `-j` and `-c` options to speed up the download and sync process._
