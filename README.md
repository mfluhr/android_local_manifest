# Local Manifest for building Android 9

This local manifest can be used with the _android-9.0.0_r46_ release tag (Build ID _PQ3A.190801.002_) when building Android on a workstation running a modern Linux distribution based on Linux kernel 5.18+, like Ubuntu 22.04 LTS for example.

It contains the following fixes/changes applied:

- Workaround for mmap error when building (See [here](https://android-review.googlesource.com/c/platform/art/+/2226578) for more details)
- `update-flex-2.5.39.sh` script to rebuild the _flex_ prebuilt binary using the workstation build tools

It also contains the ACME emulators `acme_x86_64` and `acme_car_x86_64`, for both classic Phone & Tablet Android and Android Automotive, that will be used as base for adding and/or replacing components and features.

## Usage

```text
repo init -u https://android.googlesource.com/platform/manifest -b android-9.0.0_r46
git clone https://github.com/mfluhr/android_local_manifest.git -b android-9.0.0_r46-acme .repo/local_manifests
repo sync
```

_Please note that the manifest URL used when calling `repo init` can be replaced with a local mirror URL and the `repo sync` options can be tailored to the build environment using among others the `-j` and `-c` options to speed up the download and sync process._

## Build Errors Troubleshooting

### `flex` locale errors

```text
[ 10% 10609/105812] //external/selinux/checkpolicy:checkpolicy lex policy_scan.l [linux_glibc]
FAILED: out/soong/.intermediates/external/selinux/checkpolicy/checkpolicy/linux_glibc_x86_64/gen/lex/external/selinux/checkpolicy/policy_scan.c
prebuilts/misc/linux-x86/flex/flex-2.5.39 -oout/soong/.intermediates/external/selinux/checkpolicy/checkpolicy/linux_glibc_x86_64/gen/lex/external/selinux/checkpolicy/policy_scan.c external/selinux/checkpolicy/policy_scan.l
flex-2.5.39: loadlocale.c:130: _nl_intern_locale_data: Assertion `cnt < (sizeof (_nl_value_type_LC_TIME) / sizeof (_nl_value_type_LC_TIME[0]))' failed.
Aborted (core dumped)
```

This error is usually caused by the `LANG` (or `LC_ALL`) environment variable being set to a locale corresponding to a specific language and can be fixed by setting the `LC_ALL` environment variable to `C`. However, with Android 9 builds, the `flex-2.5.39` executable has to be rebuilt, using the platform build tools.

This can be achieved using the `vendor/acme/scripts/update-flex-2.5.39/update-flex-2.5.39.sh` script:

```text
$ ./vendor/acme/scripts/update-flex-2.5.39/update-flex-2.5.39.sh
[...]

#### 'flex-2.5.39' binary replaced successfully ####

```
