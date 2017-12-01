DRC's TurboVNC Build Scripts
=============================

These scripts are used to build the "official" TurboVNC binaries, which work
on any Linux platform with GLIB 2.5 and later, as well as Windows XP and
later and OS X 10.5 and later.

See BUILDING.md in the TurboVNC source for basic build requirements.
Additional build requirements for these scripts are listed below.


Build Environment: Linux
------------------------

Recommended distro:  Red Hat or CentOS Enterprise Linux 5 64-bit

Install all development kits necessary to build a 32-bit and a 64-bit version
of TurboVNC (both 32-bit and 64-bit libjpeg-turbo SDK's should be installed in
their default locations.  Refer to BUILDING.md for more information.)

The libjpeg-turbo JNI JAR files should be installed under
/opt/libjpeg-turbo-jni.


Build Environment: OS X
-----------------------

OS X 10.7 (Lion) or later required

Xcode 4.1.x or later (available at https://developer.apple.com/downloads --
Apple ID required.)  The build scripts need this in order to produce TurboVNC
binaries that are backward compatible with OS X 10.7.  The Xcode Command Line
Tools should be installed.

CMake should be installed somewhere in the PATH.  The version in MacPorts
(http://www.MacPorts.org) works, or just install the CMake application from
the DMG (http://www.cmake.org) and sym link
/Applications/CMake\ {version}.app/Contents/bin/cmake to a directory in your
PATH.

The libjpeg-turbo SDK should be installed in its default location.


Build Environment: Windows
--------------------------

Windows XP 64-bit or later required

CMake should be installed somewhere in the PATH.

The directory containing the 64-bit Visual C++ compiler
(e.g. c:\Program Files (x86)\Microsoft Visual Studio 10.0\VC\bin\amd64)
should be listed in the PATH environment variable.

The directory containing the 64-bit Windows SDK libraries
(e.g. C:\Program Files\Microsoft SDKs\Windows\v7.0\Lib\x64)
should be listed in the LIB environment variable.

The directories containing the Visual C++ and Windows SDK header files
(e.g. c:\Program Files (x86)\Microsoft Visual Studio 10.0\VC\include and
C:\Program Files\Microsoft SDKs\Windows\v7.0\include)
should be listed in the INCLUDE environment variable.

The official TurboVNC binaries are generated using the Windows SDK for
Windows 7 and .NET Framework 3.5 SP1
(http://www.microsoft.com/en-us/download/details.aspx?id=3138),
which contains Visual C++ 10.0, but any reasonably modern version of Visual
C++ and the Windows SDK should work.

Install all development kits necessary to build a 32-bit and a 64-bit version
of TurboVNC (both 32-bit and 64-bit libjpeg-turbo SDK's for Visual C++ should
be installed in their default locations.  Refer to BUILDING.md for more
information.)


Build Procedure
---------------

Executing

  buildvnc [branch/tag]

(where branch/tag is, for instance, "2.0.x" and defaults to "master") will
generate both a pristine source tarball and binaries for the platform on which
the script is executed.  These are placed under
$HOME/src/vnc.nightly/YYYYMMDD/files, where YYYYMMDD is a build number based
on today's date.  If the build is successful, then a sym link will be created
from $HOME/src/vnc.nightly/latest to $HOME/src/vnc.nightly/YYYYMMDD.

Once a full build is completed on one platform, then you can use the existing
source tarball to build binaries on other platforms by running 'buildvnc -e'
(assuming that $HOME/src is a shared directory.)

NOTE: On Windows, buildvnc should be run from inside an MSYS shell.  If you
are mounting your home directory as a drive letter (e.g. H:), then set the HOME
environment variable to the MinGW path for that drive (e.g. /h) prior to
running buildvnc.

Run 'buildvnc -h' for usage information.


Signing the Linux Packages
--------------------------

To sign the RPMs and DEBs using a GPG key, create a file called "gpgsign" in
the same directory as buildvnc, and include the following contents in the file:

GPG_KEY_NAME={full name of GPG key to use (as listed in 'gpg --list-keys')}
GPG_KEY_ID={key ID of GPG key to use (as listed in 'gpg --list-keys')}
GPG_KEY_PASS={password for GPG key}

debsigs (available at https://gitlab.com/debsigs/debsigs/tags) must be
installed and in the PATH.


Signing the Windows Packages
----------------------------

To sign the Windows installers using a code signing certificate, create a file
called "mssign" in the same directory as buildvnc, and include the following
contents in the file:

MS_KEY_FILE={full path to a .pfx file containing the code signing certificate}
MS_KEY_PASS={password for certificate}

signtool (available in the Windows SDK) must be in the PATH.


Signing the Java TurboVNC Viewer JAR File
-----------------------------------------

To sign the Java TurboVNC Viewer JAR file using an official code signing
certificate, create a file called "jarsign" under setupscripts/,
and include the following contents in the file:

JAVA_KEYSTORE={value of JAVA_KEYSTORE CMake variable}
JAVA_KEYSTORE_PASS={value of JAVA_KEYSTORE_PASS CMake variable}
JAVA_KEYSTORE_TYPE={value of JAVA_KEYSTORE_TYPE CMake variable}
JAVA_KEY_ALIAS={value of JAVA_KEY_ALIAS CMake variable}
JAVA_KEY_PASS={value of JAVA_KEY_PASS CMake variable}
JAVA_TSA_URL={value of JAVA_TSA_URL CMake variable}

The CMake variables in question are documented in java/CMakeLists.txt in the
TurboVNC source.  If the "jarsign" file is not present, then the Java TurboVNC
Viewer JAR will be signed using a self-signed certificate, which is adequate
for development purposes but which cannot be easily used in a production
environment.


Distributing a Pre-Release Build
--------------------------------

Executing

  uploadvnc <SourceForge user name>

will upload the files from $HOME/src/vnc.nightly/latest/files to
http://turbovnc.sourceforge.net/vnc.nightly

Having an SSH key installed is highly recommended to avoid having to enter your
password multiple times.
