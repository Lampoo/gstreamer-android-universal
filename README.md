# QT(qmlglsink)+GSTREAMER+ANDROID

Official release does not precompile qmlglsink as part of gst-plugins-good due to QT dependency reason and qmlglsink has be built from source.

# Install QT creator

Visit https://www.qt.io/download to download the QT Online Installer. The installer will guide you to install the qt creator. Make sure all platform variants (ARMv7/ARMv8/x86/x86\_64) are selected.

# Build steps

```
$ git clone https://gitlab.freedesktop.org/gstreamer/cerbero/
$ cd cerbero
$ git checkout -b 1.16.0 1.16.0
$ export QT5_PREFIX=/path/to/qt5
$ ./cerbero-uninstalled -c config/cross-android-universal.cbc bootstrap
$ ./cerbero-uninstalled -v qt5 -c config/cross-android-universal.cbc package gstreamer-1.0
```

# Patch

Apply the patch if compile on Ubuntu 19.04.

```
diff --git a/cerbero/enums.py b/cerbero/enums.py
index 694038ef..e61705b5 100644
--- a/cerbero/enums.py
+++ b/cerbero/enums.py
@@ -86,6 +86,7 @@ class DistroVersion:
     UBUNTU_XENIAL = 'ubuntu_16_04_xenial'
     UBUNTU_ARTFUL = 'ubuntu_17_10_artful'
     UBUNTU_BIONIC = 'ubuntu_18_04_bionic'
+    UBUNTU_DISCO = 'ubuntu_19_04_disco'
     FEDORA_16 = 'fedora_16'
     FEDORA_17 = 'fedora_17'
     FEDORA_18 = 'fedora_18'
diff --git a/cerbero/utils/__init__.py b/cerbero/utils/__init__.py
index 5f1fb69f..359ee58f 100644
--- a/cerbero/utils/__init__.py
+++ b/cerbero/utils/__init__.py
@@ -208,6 +208,8 @@ def system_info():
                 distro_version = DistroVersion.UBUNTU_ARTFUL
             elif d[2] in ['bionic', 'tara', 'tessa']:
                 distro_version = DistroVersion.UBUNTU_BIONIC
+            elif d[2] in ['disco']:
+                distro_version = DistroVersion.UBUNTU_DISCO
             elif d[1].startswith('6.'):
                 distro_version = DistroVersion.DEBIAN_SQUEEZE
             elif d[1].startswith('7.') or d[1].startswith('wheezy'):
```
