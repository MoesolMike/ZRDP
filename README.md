# ZRDP
<table>
  <tr>
    <td style="text-align: center; vertical-align: middle; padding: 0;">
      <img src="https://github.com/MoesolMike/ZRDP/blob/main/images/zrdp_logo.png" alt="ZRDP Logo"/>
    </td>
    <td style="text-align: center; vertical-align: middle; padding: 10px;">
      <div style="display: flex; flex-direction: column; justify-content: center; height: 256px;">
        <h1 style="font-size: 128px; font-weight: bold; margin: 0;">ZRDP v1.1.0.4</h1>
        <p style="font-size: 32px; margin: 10px 0 0 0;">XFreeRDP GUI Wrapper with YubiKey & Multi Desktop Management support.</p>
      </div>
    </td>
  </tr>
</table>

## Overview

ZRDP is a powerful **XFreeRDP GUI Wrapper**. A user-friendly graphical interface designed to simplify xfreerdp usage and enhance the management of remote desktop connections. This wrapper provides a professional, secure, and customizable experience for users who need advanced remote desktop functionality, including **smartcard support** and **FIPS compliance**

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

To use ZRDP, ensure the following prerequisites are met:

### Software Requirements
- **Python 3.6+**: The wrapper is written in Python and requires a modern version of Python.
- **Tkinter**: A GUI library for Python, included by default in most Python installations.
- **Redmine Markdown Support**: For documentation purposes.

### System Requirements
- **Linux Environment**: The wrapper is designed for Linux systems.
- **Smartcard Reader**: For smartcard authentication, ensure a compatible smartcard reader is connected.
- **FIPS-Compliant Environment**: For FIPS mode, the server must be configured with Kerberos authentication.

---

## Features

### 1. **Dynamic Appearance Customization**
- **Color Profiles**: Inspired by popular themes like Remmina and Ubuntu, users can select from multiple color profiles, including:
  - Remmina Classic
  - Remmina Dark
  - Remmina Blue
  - Ubuntu Style
  - Professional Dark
  - Arctic Blue
  - Midnight Blue
- **Spacing Profiles**: Adjust layout padding with options like Compact, Standard, and Spacious.

### 2. **Smartcard Support**
- Automatically detects available smartcards using `xfreerdp /list:smartcard`.
- Provides a dropdown menu for selecting smartcards, with white background and black text for clarity.
- Supports smartcard authentication for secure connections.

### 3. **FIPS Compliance**
- Enables FIPS mode for secure remote desktop connections.
- Provides detailed notes on configuring Kerberos authentication for FIPS compliance.

### 4. **Profile Management**
- Save, load, delete, and create new connection profiles.
- Profiles are stored in a JSON configuration file (`~/.xfreerdp_profiles.json`) for easy portability.

### 5. **Connection Management**
- Supports multiple simultaneous connections.
- Provides a "Disconnect All" button to terminate all active connections.
- Displays active connections in a separate window with details like server name, start time, and status.

### 6. **Advanced Connection Settings**
- Configure server, port, username, domain, screen size, color depth, and fullscreen mode.
- Security settings include NLA, TLS, EXT, certificate ignoring, and session timeout.
- Smartcard and FIPS mode options for enhanced security.

### 7. **Command Preview**
- Generates the exact `xfreerdp` command based on user settings.
- Allows users to copy the command to the clipboard for manual execution.

### 8. **Connection Testing**
- Tests server connectivity using `ping` to ensure the server is reachable before attempting a connection.

---

## How It Works

### Workflow
1. **Profile Selection**: Choose or create a connection profile.
2. **Smartcard Detection**: Automatically scans for smartcards and updates the dropdown menu.
3. **Connection Settings**: Configure server, security, and display settings.
4. **Command Generation**: Builds the `xfreerdp` command dynamically based on user input.
5. **Connection Management**: Start, monitor, and terminate remote desktop connections.

### Smartcard Integration
The wrapper uses `xfreerdp /list:smartcard` to detect available smartcards. It parses the output to extract details like the certificate DN, reader name, and slot ID. Users can select a smartcard from the dropdown menu for authentication.

---

## Example Configuration

### Sample Profile
```json
{
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
  "smartcard": "SmartCard Reader 1"
}
```

### Generated Command
```bash
/usr/local/bin/xfreerdp /v:rdp.example.com /port:3389 /u:user1 /d:example.local /size:1920x1080 /bpp:32 /sec:nla /sec:tls /sec:ext /cert:ignore +fipsmode /smartcard:SmartCardReader1 /timeout:1800
```

## Conclusion

ZRDP an **XFreeRDP GUI Wrapper** is a feature-rich tool for managing remote desktop connections with ease and security. Its intuitive interface, smartcard support, and FIPS compliance make it ideal for professional environments. Whether you're connecting to a single server or managing multiple connections, this wrapper streamlines the process while offering advanced customization options.

---

