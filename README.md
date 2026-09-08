<table>
  <tr>
    <td style="text-align: center; vertical-align: middle; padding: 0;">
      <img src="https://github.com/MoesolMike/ZRDP/blob/main/images/zrdp_logo.png" alt="ZRDP Logo"/>
    </td>
    <td style="text-align: center; vertical-align: middle; padding: 10px;">
      <div style="display: flex; flex-direction: column; justify-content: center; height: 256px;">
        <h1 style="font-size: 128px; font-weight: bold; margin: 0;">ZRDP v1.2.1</h1>
        <p style="font-size: 32px; margin: 10px 0 0 0;">XFreeRDP GUI Wrapper with SmartCard, YubiKey, Kerberos, FIPS, and Multi-Desktop Management support.</p>
      </div>
    </td>
  </tr>
</table>

## Overview

ZRDP is a Python-based GUI wrapper for `xfreerdp`. It provides a simple interface for managing multiple RDP connections and supports **SmartCard/YubiKey authentication**, **Kerberos**, and **FIPS mode**.

---

## Screenshots

<table>
  <tr>
    <td style="text-align: center; vertical-align: middle; padding: 0;">
      <img src="https://github.com/MoesolMike/ZRDP/blob/main/images/zrdp_home.png" alt="ZRDP Home" width="400"/>
      <img src="https://github.com/MoesolMike/ZRDP/blob/main/images/zrdp_prefs.png" alt="ZRDP Preferences" width="400"/>
    </td>
  </tr>
</table>

---

## Requirements

### Software Requirements

- **Python 3.10+**
- **Tkinter**
- **XFreeRDP / FreeRDP 3.16+**
- **Kerberos tools**
  - `krb5-user`
  - `krb5-config`
  - `kinit`
  - `klist`

Install common dependencies on Debian/Ubuntu:

```bash
sudo apt update
sudo apt install python3 python3-tk krb5-user krb5-config
```

### System Requirements

- Linux or Unix-like system with X11.
- Smartcard reader for SmartCard/YubiKey authentication.
- Kerberos configuration for FIPS/domain environments.
- FIPS-capable FreeRDP build when using FIPS mode.

---

## Default Paths

Default `xfreerdp` path:

```text
/usr/local/bin/xfreerdp
```

Default ZRDP profile/config file:

```text
~/.zrdp.json
```

Both paths can be changed in **Preferences**.

---

## Features

### Profile Management

- Save, load, delete, and manage RDP profiles.
- Profiles are stored in JSON format.
- Default profile file:

```text
~/.zrdp.json
```

- Profile config file location can be changed in **Preferences**.

### SmartCard / YubiKey Support

- Detects smartcards using:

```bash
xfreerdp /list:smartcard
```

- Provides a dropdown for selecting detected smartcards.
- Supports manual refresh of smartcard readers.

### Kerberos Support

- Enable or disable Kerberos.
- Uses `klist` to check for `KRBTGT` tickets.
- Runs `kinit` if no valid ticket is found.

### FIPS Support

- FIPS Mode checkbox in Security Settings.
- Adds this option to the generated command:

```text
+fipsmode
```

- Includes built-in FIPS/Kerberos configuration notes.

### Connection Management

- Supports multiple active RDP sessions.
- Show active connections.
- Disconnect selected sessions.
- Disconnect all sessions.

### Display Options

- Custom resolution.
- Fullscreen mode.
- Color depth.
- Session timeout.

### Command Preview

- Shows the generated `xfreerdp` command.
- Allows copying the command to the clipboard.

### Cleaner Interface

- Collapsible sections for:
  - Security Settings
  - Kerberos Settings
  - Smartcard Settings
  - Display Settings
  - FIPS Configuration Notes

---

## XFreeRDP Build Prerequisites

For FIPS and smartcard support, build **XFreeRDP 3.16+** into `/usr/local` with:

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

### Required Build Packages

- `libssl-dev`
- `libgssapi-krb5-dev`
- `libpcsclite-dev`
- `libx11-dev`
- `libpulse-dev`
- `libcups2-dev`
- `cmake`
- `gcc` or `clang`
- `make` or `ninja`

Optional:

- `libwayland-dev`
- `libsdl2-dev`

---

## Usage

Run ZRDP:

```bash
python3 zrdp.py
```

Or, if executable:

```bash
./zrdp
```

Basic workflow:

1. Open ZRDP.
2. Create or select a profile.
3. Configure server, username, domain, security, Kerberos, smartcard, and display settings.
4. Save the profile if needed.
5. Click **Connect**.

---

## Example Configuration

Example `~/.zrdp.json` profile:

```json
{
  "preferences": {
    "config_file": "/home/user/.zrdp.json",
    "xfreerdp_path": "/usr/local/bin/xfreerdp",
    "theme": "Dark Professional",
    "log_dir": "~/zrdp.log",
    "log_level": "INFO"
  },
  "Example RDP Server": {
    "server": "rdp.example.com",
    "port": "3389",
    "username": "user1",
    "domain": "example.local",
    "size": "1920x1080",
    "fullscreen": false,
    "bpp": "32",
    "sec_nla": true,
    "sec_tls": true,
    "sec_ext": true,
    "cert_ignore": true,
    "fips": true,
    "session_timeout": "30",
    "kerberos_enabled": true,
    "kerberos_principal": "user1@EXAMPLE.LOCAL",
    "smartcard": "None"
  }
}
```

Example generated command:

```bash
/usr/local/bin/xfreerdp \
  /v:rdp.example.com \
  /u:user1 \
  /d:example.local \
  /sec:nla \
  /sec:tls \
  /sec:ext \
  /cert:ignore \
  +fipsmode \
  /size:1920x1080 \
  /bpp:32 \
  /timeout:1800 \
  /log-level:INFO
```

---

## FIPS Notes

For RDP on Windows systems enforcing FIPS, setting Kerberos/SPN configuration is required on the Windows side.

Example: (Run setspn on a domain controller)

```text
C:> setspn -A TERMSRV/server3.domain.com SERVER3
```

Expected format:

```text
TERMSRV/<SERVER_FQDN> <SERVER_SHORT_NAME>
```

---

## Known Issues

- Theme changes may require restarting the application.
- Default `xfreerdp` path is `/usr/local/bin/xfreerdp`; change it in **Preferences** if needed.
- Modular mode is disabled.

---

## References

[1] Python Software Foundation, Python 3.10 documentation.  
[2] Python Software Foundation, Tkinter documentation.  
[3] FreeRDP project documentation for `xfreerdp`.  
[4] MIT Kerberos documentation for `kinit` and `klist`.
