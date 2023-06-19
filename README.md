# Local Manifest for building Android 8.1

This local manifest can be used with the _android-8.1.0_r67_ release tag (Build ID _OPM8.190605.005_) when building Android on a workstation running a modern Linux distribution based on Linux kernel 5.18+, like Ubuntu 22.04 LTS for example.

It contains the following fixes/changes applied:

- Workaround for mmap error when building (See [here](https://android-review.googlesource.com/c/platform/art/+/2226578) for more details)
- `update-flex-2.5.39.sh` script to rebuild the _flex_ prebuilt binary using the workstation build tools

## Usage

```text
repo init -u https://android.googlesource.com/platform/manifest -b android-8.1.0_r67
git clone https://github.com/mfluhr/android_local_manifest.git -b android-8.1.0_r67 .repo/local_manifests
repo sync
```

_Please note that the manifest URL used when calling `repo init` can be replaced with a local mirror URL and the `repo sync` options can be tailored to the build environment using among others the `-j` and `-c` options to speed up the download and sync process._

## Build Errors Troubleshooting

### `flex` locale errors

```text
[  1% 1697/85715] Lex: applypatch <= bootable/recovery/edify/lexer.ll
FAILED: out/target/product/generic_x86_64/obj/STATIC_LIBRARIES/libedify_intermediates/lexer.cpp
/bin/bash -c "prebuilts/misc/linux-x86/flex/flex-2.5.39 -oout/target/product/generic_x86_64/obj/STATIC_LIBRARIES/libedify_intermediates/lexer.cpp bootable/recovery/edify/lexer.ll"
flex-2.5.39: loadlocale.c:130: _nl_intern_locale_data: Assertion `cnt < (sizeof (_nl_value_type_LC_TIME) / sizeof (_nl_value_type_LC_TIME[0]))' failed.
Aborted (core dumped)
```

This error is usually caused by the `LANG` (or `LC_ALL`) environment variable being set to a locale corresponding to a specific language.
In order to fix it, simply set the `LC_ALL` environment variable to `C`:

```text
$ echo $LANG
en_US.UTF-8
$ export LC_ALL=C
```

In some corner cases, the issue might still persist, even after setting the `LC_ALL` environment variable to `C`. In such cases, the `flex-2.5.39` executable should be rebuilt, using the platform build tools.

This can be achieved using the `vendor/acme/scripts/update-flex-2.5.39/update-flex-2.5.39.sh` script:

```text
$ ./vendor/acme/scripts/update-flex-2.5.39/update-flex-2.5.39.sh
[...]

#### 'flex-2.5.39' binary replaced successfully ####

```

### Build fingerprint errors

```text
[  8% 7700/85715] Target buildinfo: out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop
FAILED: out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop
/bin/bash -c "(echo > out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop ) && (TARGET_BUILD_TYPE=\"userdebug\" TARGET_BUILD_FLAVOR=\"sdk_phone_x86_64-userdebug\" TARGET_DEVICE=\"generic_x86_64\" PRODUCT_NAME=\"sdk_phone_x86_64\" PRODUCT_BRAND=\"Android\" PRODUCT_DEFAULT_LOCALE=\"en-US\" PRODUCT_DEFAULT_WIFI_CHANNELS=\"\" PRODUCT_MODEL=\"Android SDK built for x86_64\" PRODUCT_MANUFACTURER=\"unknown\" PRIVATE_BUILD_DESC=\"sdk_phone_x86_64-userdebug 8.1.0 OPM8.190605.005 \$(cat out/build_number.txt) test-keys\" BUILD_ID=\"OPM8.190605.005\" BUILD_DISPLAY_ID=\"sdk_phone_x86_64-userdebug 8.1.0 OPM8.190605.005 \$(cat out/build_number.txt) test-keys\" DATE=\"date -d @\$(cat out/build_date.txt)\" BUILD_NUMBER=\"\$(cat out/build_number.txt)\" BOARD_BUILD_SYSTEM_ROOT_IMAGE=\"\" AB_OTA_UPDATER=\"\" PLATFORM_VERSION=\"8.1.0\" PLATFORM_SECURITY_PATCH=\"2019-06-05\" PLATFORM_BASE_OS=\"\" PLATFORM_SDK_VERSION=\"27\" PLATFORM_PREVIEW_SDK_VERSION=\"0\" PLATFORM_VERSION_CODENAME=\"REL\" PLATFORM_VERSION_ALL_CODENAMES=\"REL\" BUILD_VERSION_TAGS=\"test-keys\" BUILD_FINGERPRINT=\"\$(cat out/target/product/generic_x86_64/build_fingerprint.txt)\"  TARGET_CPU_ABI_LIST=\"x86_64,x86\" TARGET_CPU_ABI_LIST_32_BIT=\"x86\" TARGET_CPU_ABI_LIST_64_BIT=\"x86_64\" TARGET_CPU_ABI=\"x86_64\" TARGET_CPU_ABI2=\"\" TARGET_AAPT_CHARACTERISTICS=\"emulator\"         bash build/tools/buildinfo.sh >> out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop ) && (if [ -f \"build/target/board/generic_x86_64/system.prop\" ]; then echo \"#\" >> out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop; echo Target buildinfo from: \"build/target/board/generic_x86_64/system.prop\"; echo \"# from build/target/board/generic_x86_64/system.prop\" >> out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop; echo \"#\" >> out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop; cat build/target/board/generic_x86_64/system.prop >> out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop; fi ) && (echo >> out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop; echo \"#\" >> out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop; echo \"# ADDITIONAL_BUILD_PROPERTIES\" >> out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop; echo \"#\" >> out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop ) && (echo \"ro.bionic.ld.warning=1\" >> out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop;  echo \"ro.treble.enabled=true\" >> out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop;  echo \"persist.sys.dalvik.vm.lib.2=libart.so\" >> out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop;  echo \"dalvik.vm.isa.x86_64.variant=x86_64\" >> out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop;  echo \"dalvik.vm.isa.x86_64.features=default\" >> out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop;  echo \"dalvik.vm.isa.x86.variant=x86_64\" >> out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop;  echo \"dalvik.vm.isa.x86.features=default\" >> out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop;  echo \"dalvik.vm.lockprof.threshold=500\" >> out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop;  echo \"net.bt.name=Android\" >> out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop;  echo \"dalvik.vm.stack-trace-dir=/data/anr\" >> out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop ) && (cat out/target/product/generic_x86_64/android-info.txt | grep 'require version-' | sed -e 's/require version-/ro.build.expect./g' >> out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop ) && (build/tools/post_process_props.py out/target/product/generic_x86_64/obj/ETC/system_build_prop_intermediates/build.prop )"
Target buildinfo from: build/target/board/generic_x86_64/system.prop
error: ro.build.fingerprint cannot exceed 91 bytes: Android/sdk_phone_x86_64/generic_x86_64:8.1.0/OPM8.190605.005/mathie05310023:userdebug/test-keys (96)
```

This issue is caused by an overflow when the Android build system is generating the fingerprint string and it has been fixed starting with Android 9 (For more details, please check [here](https://android-review.googlesource.com/c/platform/build/+/510355) and [here](https://android.googlesource.com/platform/build/+/47c4eb468073f6d8f0f2771bcf6b54835c4cff5d)).

The usual workaround to fix this issue is to hardcode a build number:

```text
export BUILD_NUMBER=42
```

### Jack server not starting properly

```text
[  0% 114/85715] Ensuring Jack server is installed and started
FAILED: setup-jack-server
/bin/bash -c "(prebuilts/sdk/tools/jack-admin install-server prebuilts/sdk/tools/jack-launcher.jar prebuilts/sdk/tools/jack-server-4.11.ALPHA.jar  2>&1 || (exit 0) ) && (JACK_SERVER_VM_ARGUMENTS=\"-Dfile.encoding=UTF-8 -XX:+TieredCompilation\" prebuilts/sdk/tools/jack-admin start-server 2>&1 || exit 0 ) && (prebuilts/sdk/tools/jack-admin update server prebuilts/sdk/tools/jack-server-4.11.ALPHA.jar 4.11.ALPHA 2>&1 || exit 0 ) && (prebuilts/sdk/tools/jack-admin update jack prebuilts/sdk/tools/jacks/jack-4.32.CANDIDATE.jar 4.32.CANDIDATE || exit 47 )"
Jack server already installed in "/home/mathieu/.jack-server"
Launching Jack server java -XX:MaxJavaStackTraceDepth=-1 -Djava.io.tmpdir=/tmp -Dfile.encoding=UTF-8 -XX:+TieredCompilation -cp /home/mathieu/.jack-server/launcher.jar com.android.jack.launcher.ServerLauncher
Jack server failed to (re)start, try 'jack-diagnose' or see Jack server log
SSL error when connecting to the Jack server. Try 'jack-diagnose'
SSL error when connecting to the Jack server. Try 'jack-diagnose'
```

This issue is caused by the TLSv1 and TLSv1.1 algorithms being blacklisted inside the OpenJDK Java security configuration file.

To properly fix it, edit (as root) `/etc/java-8-openjdk/security/java.security` and remove `TLSv1, TLSv1.1` from the `jdk.tls.disabledAlgorithms` definition.

Finally, restart the jack server:

```text
croot
cd prebuilts/sdk/tools
./jack-admin kill-server
./jack-admin start-server
```
