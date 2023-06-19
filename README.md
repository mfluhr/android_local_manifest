# Android Local Manifests

This repository contains local manifests that add,replace and/or delete projects inside an official AOSP source tree.  
For more information about local manifests, please check the _Local Manifests_ section of the _`repo` Manifest Format_ document [here](https://gerrit.googlesource.com/git-repo/+/master/docs/manifest-format.md#local-manifests).

## Usage

```text
repo init -u https://android.googlesource.com/platform/manifest -b <RELEASE_TAG>
git clone https://github.com/mfluhr/android_local_manifest.git -b <BRANCH_NAME> .repo/local_manifests
repo sync
```

Where:

- `RELEASE_TAG` is an Android release tag (like `android-13.0.0_r42` for example)
- `BRANCH_NAME` is the name of the branch inside this repository corresponding to the Android release tag `RELEASE_TAG`

_Please note that the manifest URL used when calling `repo init` can be replaced with a local mirror URL and the `repo sync` options can be tailored to the build environment using among others the `-j` and `-c` options to speed up the download and sync process._
