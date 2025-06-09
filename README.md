# Manifest for building LineageOS 16.0 for Sony Xperia L

Some extremely basic instructions:

- Make a new directory for Lineage sources and enter it:
```
mkdir los
cd los
```

- Install Building Dependencies
```
sudo apt install bc bison build-essential curl flex g++-multilib gcc-multilib git gnupg gperf libxml2 lib32z1-dev liblz4-tool libncurses5-dev libsdl1.2-dev libwxgtk3.0-gtk3-dev imagemagick git lunzip lzop schedtool squashfs-tools xsltproc zip zlib1g-dev openjdk-8-jdk python3 perl xmlstarlet virtualenv xz-utils rr jq libncurses5 pngcrush lib32ncurses5-dev git-lfs libxml2 openjdk-11-jdk-headless rsync
```

- Install repo bin to system:
```
curl https://storage.googleapis.com/git-repo-downloads/repo > repo
chmod a+x repo
mv ./repo /usr/bin
```

- Install ccache
```
sudo apt install ccache
```

- Turn ccache on and set its size to 25GB
```
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
ccache -M 25G
```

- Initialize repo in this directory with the LineageOS 16.0 repository:
```
repo init -u https://github.com/LineageOS/android.git -b lineage-16.0
```

- Clone this repo to .repo/local_manifests for roomservice.xml containing the repositories with the device/vendor/hw trees needed to build for the Xperia L:
```
git clone https://github.com/skye-pa1n/manifest_taoshan.git  --depth=1 -b lineage-16.0 .repo/local_manifests
```

- Sync all of the repositories in manifests:
  
- Tip: Go get some sleep, or sit back and relax, because this will take a while depending on your internet connection.

- Also repo sync is run the second time just in case anything got borked and it didnt manage to fix in the first run, dont worry it wont take long the second time. 
```
repo sync -c -j8 --force-sync --no-clone-bundle --no-tags && repo sync -c -j8 --force-sync --no-clone-bundle --no-tags
```

clone kernel
```
git clone https://github.com/skye-pa1n/android_kernel_sony_msm8930 kernel/sony/msm8930 --depth=1
```

- Finally, build as you like.
```
source build/envsetup.sh
croot
brunch taoshan
```
export ALLOW_MISSING_DEPENDENCIES=true

- External backup, ignore
```
cd /media/skye/GOS/android/external/
rm -rf gemmlowp
rm -rf fonttools
rm -rf freetype
rm -rf fsck_msdos
rm -rf dexmaker

git clone https://android.googlesource.com/platform/external/fonttools -b android-9.0.0_r46 fonttools
git clone https://android.googlesource.com/platform/external/gemmlowp -b android-9.0.0_r46 gemmlowp
git clone https://android.googlesource.com/platform/external/freetype -b android-9.0.0_r46 freetype
git clone https://android.googlesource.com/platform/external/fsck_msdos -b android-9.0.0_r46 fsck_msdos
git clone https://android.googlesource.com/platform/external/error_prone -b android-9.0.0_r46 error_prone
git clone https://android.googlesource.com/platform/external/dexmaker -b android-9.0.0_r46 dexmaker
git clone https://android.googlesource.com/platform/external/esd -b android-9.0.0_r46 esd
```
