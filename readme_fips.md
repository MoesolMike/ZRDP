# XfreeRDP v3.16 - How to build xfreerdp to only use FIPS Compatable ciphers

## Overview

When running RDP on or against FIPS MODE enabled systems, it is important to ensure only fips compliant ciphers exist in your xfreerdp build. Here are my notes to successfully compile and a FIPS compatable version of XFreeRDP as of 10/30/2025.

## Key Features

* **FIPS Compatability** Built with `-DWITH_FIPS=ON` to ensure all cryptographic operations use FIPS-approved algorithms.
* **Enhanced Security**: Disabled internal implementations of MD4, MD5, and RC4 in favor of OpenSSL FIPS modules.
* **Enterprise Ready**: Includes GSSAPI, PKCS#11, and smart card (PCSC) support
* **Audio & Printing**: PulseAudio and CUPS support for complete desktop support.

## Build Configuration

The build was configured with the following CMake parameters:

```bash
cmake -DCMAKE_BUILD_TYPE=RelWithDebInfo \
    -DWITH_GSSAPI=ON \
    -DWITH_OPENSSL=ON \
    -DWITH_FIPS=ON \
    -DWITH_X11=ON \
    -DWITH_PULSE=ON \
    -DWITH_CUPS=ON \
    -DWITH_CHANNELS=ON \
    -DWITH_CLIENT=ON \
    -DWITH_SERVER=OFF \
    -DWITH_ALSA=OFF \
    -DWITH_FFMPEG=OFF \
    -DWITH_PKCS11=ON \
    -DWITH_INTERNAL_MD4=OFF \
    -DWITH_INTERNAL_MD5=OFF \
    -DWITH_INTERNAL_RC4=OFF \
    -DWITH_CLIENT_WAYLAND=OFF \
    -DWITH_PCSC=ON \
    -DWITH_SDL3=OFF ..
```

## Security Hardening

### Cryptographic Compliance notes
* **FIPS Mode**: Double check that your download of xfreerdp is current and all cryptographic operations use FIPS 140-2 validated modules.
* **OpenSSL Integration**: Build leverages system OpenSSL for cryptographic functions
* **Disabled Weak Ciphers**: Internal MD4, MD5, and RC4 implementations disabled.

### Authentication Support
* **GSSAPI**: Kerberos authentication support (Needed for Windows)
* **PKCS#11**: Hardware security module integration 
* **Smart Cards**: PCSC support for smart card authentication in ubuntu

## Package Creation (optional)

To keep historical build versions I used the following process in Ubuntu:

```bash
# Compile with parallel processing
make -j$(nproc)

# Create package structure
mkdir -p ~/xfreerdp-fips/usr/local/bin ~/xfreerdp-fips/DEBIAN/

# Install binaries
sudo make install

# Package creation
cd ~/xfreerdp-fips/
dh_make --createorig -s -y --native
dpkg-deb --build ~/xfreerdp-fips
```

## Installation

To install the FIPS-compliant XfreeRDP package:

```bash
sudo dpkg -i ~/xfreerdp-fips-*.deb
sudo apt-get install -f  # Install any missing dependencies
```

## Theoretical Use Cases

This FIPS compatable builds would be a good place to start for those seeking:
* Any organization requiring validated cryptographic modules

## Once installed you can copy `zrdp` to /usr/local/bin

## Troubleshooting w/Fixes

* ERROR: ...Server not found in kerberos database.

 Windows FIPS RDP requires servers have TERMSRV added to their ServicePrincipalNames(spn)

 SPN Update Examples to be run from Elevated Command line on the Domain Controller.
<pre>
C:> setspn -A TERMSRV/winsrv1.domain.com  WINSRV1
C:> setspn -A TERMSRV/winsrv2.domain.com  WINSRV2
C:> setspn -A TERMSRV/rhelsrv1.domain.com RHELSRV1
</pre>

---
