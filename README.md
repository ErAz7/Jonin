# Jonin [![version-shield]][jonin-release] [![cross-platform-shield]](#platforms)

> A fully customizable CLI application to coummunicate with and send commands to **[Ninja][ninja]** running behind any NAT, firewall and proxy! Providing secure shell access, file transfer and shell stream (stream shell output from remote to a local file). Jonin has no prerequisites, you can just [download the release][jonin-release] and use it right away!

**Please note that Jonin is the controller (commander). You would need [Ninja][ninja] on target (remote) computer(s) to host and execute the Jonin's commands**

![logo]

# [Download][jonin-release]

You can download latest release from [here][jonin-release]

# Features

- Secure shell access to remote ([Ninja][ninja]) computer
- File upload/download (Multiple files at once) to/from remote ([Ninja][ninja])
- Shell stream, run command on remote ([Ninja][ninja]) and stream output to a file on local ([Jonin][jonin])

# Demo

![jonin](https://user-images.githubusercontent.com/46329768/179629372-c2ee8fab-8a48-4557-b049-5e99f9d3f9e4.gif)

https://user-images.githubusercontent.com/46329768/179628114-5f6d4307-4bd8-419f-8c85-873e790e440f.mp4

# Platforms

| ![windows]   | ![macos]            | ![linux]           |
| ------------ | ------------------- | ------------------ |
| Windows 10 ✔ | macOS 12 Monterey ✔ | Parrot OS 4.11.2 ✔ |

# Overview

This CLI application will provide a fully customizable tool to manage **[Ninja][ninja]** instances, connect to them, execute commands, download and upload files and use shell stream feature (stream shell output from Ninja to Jonin/local computer).

- [Download](#download)
- [Features](#features)
- [Platforms](#platforms)
- [Overview](#overview)
- [Ninja](#ninja)
- [Setup](#setup)
  - [Fix `No Ninjas` Error](#fix-no-ninjas-error)
- [Usage](#usage)
  - [Control Commands](#control-commands)
    - [change](#change)
    - [clear](#change)
    - [exit](#change)
  - [Command Types](#command-types)
    - [manage](#manage)
    - [cmd](#cmd)
    - [cmd-stream](#cmd-stream)
    - [upload](#upload)
    - [download](#download)
    - [tray](#tray)
- [Configuration File](#configuration-file)
- [Host Setup](#host-setup)
  - [Dynamic DNS](#dynamic-dns)
  - [VPS](#vps)
  - [Static IP Address](#static-ip-address)
  - [Domain Name](#domain-name)
- [Use As Spyware](#use-as-spyware)

# [Ninja][ninja]

**[Ninja][ninja]** should be running on the target computer in order to control it with Jonin. **PORT** configuration should be same on Ninja and Jonin and **HOST** should point to Jonin to establish connection

# Setup

Follow these steps to setup Jonin and Ninja:

- Download **[Jonin Release][jonin-release]** and **[Ninja Release][ninja-release]**
- Change **PORT** in `config/constants.json` for both Ninja and Jonin, the ports should be the same. Please read [This Guide](#host-setup) about how to setup **HOST** to never lose access to Ninja
- Forward configured port on **Jonin**'s router. There are lots of guides out there for port forwarding. [This one][noio-port-forwarding] from noip is a nice one
- Done! Now run **[Ninja][ninja]** on the target/remote computer, run Jonin on controller/local computer and wait for some report from your **[Ninja][ninja]** !

**For usage guide and list of commands, [check here][jonin-usage]**

**Note: `NO_LOG` in config file SHOULD be set to `true` when you want to use Ninja as a service, otherwise, the log file might grow larger forever (up to the limit)**

### Fix `No Ninjas` Error

If you did all above and got `No Ninjas`, then the chances are your ISP is putting you behind a NAT. To check this, you can find your router's WAN IP address (can be found on router's homepage) and then compare it to the actual IP address that you have on the internet (can be found by searching `my ip` on google); If these IP addresses were NOT identical, then your router is behind a NAT. To fix this, you should ask your ISP to change your NAT type to `OPEN`

# Usage

There are few command types available that each of them has their own commands. Here's a full instruction:

**(Note that you can customize `all` these commands in `config/constants.json`)**

### Control Commands

- #### `change`
  use this command to change command type ([list of all types](#command-types))
- #### `cls`, `clear`
  clear console
- #### `/exit`
  cose Jonin

### Command Types

- #### `manage`

  **Ninja** management command. using this command type, you can
  see list of your **Ninja** s, check their details and connect to them

  **note: you should list **Ninja** s first in order to use commands that
  require index**

  **Usage:**

  - `list`

    List all Ninjas

  - `expand <index>`

    display all details of the **Ninja** with index < index > from Ninja list

  - `connect <index>`

    connect to the **Ninja** with index < index > from Ninja list

  - `disconnect`

    disconnect from Ninja

  **Example:**

  - `list`
    show list of Ninjas

  - `expand 1`

    display all details of Ninja **Ninja** with index 1 from **Ninja** list

  - `connect 1`

    connect to Ninja **Ninja** with index 1 from **Ninja** list

- #### `cmd`

  This is a direct shell access to **Ninja**.
  You can type any command and see the output

  **note: you can press ESC at any time to kill current shell and start a new one**

  **Usage:**

  `any valid command`

  **Example:**

  - `ping 8.8.8.8`
  - `diskpart`
  - `traceroute`

- #### `cmd-stream`

  Same as `cmd`, except this one takes 2 local file paths and pipes the command output and error from **Ninja** to these files respectively

  **note: you can press Esc at any time to end transfer**

  **Usage:**

  `@<command>@<output_file>@[<error_file>@]`

  - `<command>` any valid command

  - `<output_file>` a valid local file path to pipe process **output**

  - `<error_file>` a valid local file path to pipe process **error**

  **Example:**

  - `@tracert 8.8.8.8@G:/trace.txt@`

    pipe trace route result from Ninja to G:/trace.txt)

  - `@ffmpeg -f gdigrab -framerate 30 -i desktop -f matroska -@G:/desktop.mkv@G:/desktop-err.txt@`
    record **Ninja** desktop and pipe it to G:/desktop.mkv, also pipe any process error to G:/desktop-err.txt

- #### `upload`

  Upload one or more files from local to **Ninja**

  **note: you can press Esc at any time to end transfer**

  **Usage:**

  `@<local_file_path>|<remote_file_path>@[...]`

  - `<local_file_path>` a valid local (source) file path

  - `<remote_file_path>` a valid file path on **Ninja** (destination)

  - `[...]` add more path pairs to upload multiple files at once

  **Example:**

  - `@G:/movie.mkv|C:/movie.mkv@`

    upload movie.mkv from local's G drive to **Ninja**'s C drive

  - `@G:/movie1.mkv|C:/movie1.mkv@G:/movie2.mkv|C:/movie2.mkv@`
    upload movie1.mkv and movie2.mkv from local's G drive
    to **Ninja**'s C drive both together

- #### `download`

  Download one or more files from **Ninja** to local

  **note: you can press Esc at any time to end transfer**

  **Usage:**

  `@<local_file_path>|<remote_file_path>@[...]`

  - `<local_file_path>` a valid local (destination) file path

  - `<remote_file_path>` a valid file path on **Ninja** (source)

  - `[...]` add more path pairs to download multiple files at once

  **Example:**

  - `@G:/movie.mkv|C:/movie.mkv@`

    download movie.mkv from **Ninja**'s C drive to local's G drive

  - `@G:/movie1.mkv|C:/movie1.mkv@G:/movie2.mkv|C:/movie2.mkv@`
    download movie1.mkv and movie2.mkv from **Ninja**'s C drive
    to local's G drive both together

- #### `tray`

  To have some fun, you can control **Ninja**'s disk tray

  **Usage:**

  `eject`
  `close`

  **note: close command is only available for Linux for now**

# Configuration File

You can find this file in `config/constants.json`:

```jsonc
{
  // connection port
  "PORTS": {
    "DATA": 3707
  },

  // connection config. any valid Socket.io option
  "CONNECTION": {
    "RECONNECTION_DELAY_MAX": 5000,
    "RECONNECTION_DELAY": 1000,
    "TIMEOUT": 20000,

    "rejectUnauthorized": false
  },

  // interval in which file receiver party will send ack to sender
  "FILE_TRANSFER": {
    "ACK_INTERVAL": 2000
  },

  // command type names
  "COMMAND_TYPES": {
    "CMD": "cmd",
    "TRAY": "tray",
    "INSTANCE_MANAGEMENT": "manage",
    "UPLOAD": "upload",
    "DOWNLOAD": "download",
    "CMD_STREAM": "cmd-stream"
  },

  "INSTANCE_MANAGEMENT_COMMANDS": {
    "LIST": "list",
    "CONNECT": "connect",
    "DISCONNECT": "disconnect",
    "EXPAND": "expand"
  },

  "TRAY_COMMANDS": {
    "EJECT": "eject",
    "CLOSE": "close"
  },

  "CONTROL_COMMANDS": {
    "CHANGE_COMMAND_TYPE": "change",
    "CLEAR": ["cls", "clear"],
    "EXIT": ["/exit"],
    "HELP": "#help"
  },

  "CONTROL_KEYS": {
    // control key to force end streams, including
    // upload, download and shell stream
    "END_STREAM": {
      // key display name, can be anything
      "NAME": "Esc",
      // JavaScript keypress event key name
      "CODE": "escape"
    },
    // control key to restart remote shell process
    "RESTART_REMOTE_SHELL": {
      "NAME": "Esc",
      "CODE": "escape"
    }
  },

  // separators used in commands like download and upload
  "PRIMARY_SEPARATOR": "@",
  "SECONDARY_SEPARATOR": "|",

  "PROGRESS_BAR": {
    // progress bar colors
    "COLOR_MAP": {
      "FAILED": ["red", "red"],
      "INVALID": ["red", "red"],
      "DONE": ["gray", "green"],
      "IN_PROGRESS": ["gray", "cyan"]
    },
    // progress bar name/label maximum length
    "MAX_NAME_LENGTH": 10
  }
}
```

# Host Setup

When configuring **[Ninja][ninja]**, you should use a **HOST** that points to **[Jonin][jonin]** computer and also will always be available. So you'll have to use one of the following options:

### Dynamic DNS

This is the best way I can suggest since It's free and easy. you just need to create an account in one of DDNS services (like [Duck DNS][duckdns] and [No-Ip][noip]), create a domain name and set it to point to your dynamic IP address. If your ISP changed your IP, then just simply change it on DDNS website or install a Dynamic Update Client (DUC) to do this for you automatically.

### VPS

You can purchase a VPS and use its IP or hostname as **HOST** in config file. However you'll always have to control your Ninjas from this VPS. Another downside is that this is paid.

### Static IP Address

You can use an IP address for your Jonin and set this IP in **[Ninja][ninja]**'s configuration. This is not recommended since you'll have to spend money while there are easy free ways, unless you have a static IP already

### Domain Name

Just like static IP, you can use a domain name for your Jonin and set this name in **[Ninja][ninja]**'s configuration. For same reason as static IP, this also is not a recommended way

# Record Ninja Camera And Mic

One way to do this is to use **FFmpeg**. You'll need to either copy **FFmpeg** files into target computer when setting up **[Ninja][ninja]**, upload it to an already set up **Ninja** using `upload` command or order Ninja to download it itself using shell commands
Then, on windows for example, you can run this:

```bash
ffmpeg -list_devices true -f dshow -i dummy
```

to see list of DirectShow devices and then use the following `cmd-stream` command to record it on Jonin computer:

```bash
@ffmpeg -f dshow -i video="DIRECT_SHOW_CAMERA_FROM_LIST":audio="DIRECT_SHOW_MIC_FROM_LIST" -f matroska -@G:/cam.mkv@G:/cam-err.txt@
```

which will save video to **G:/cam.mkv** and errors to **G:/cam-err.txt**

# Use As Spyware

Please note that Ninja can be easily used as a spyware, especially when installed as a service, it will open full access to the target computer for the **[Jonin][jonin]** controlling it. So use it carefully and don't leave the **Ninja** process running on a computer that is connected to the internet

# Source Code

Source code will be open soon, after some refactoring and improvements

[version-shield]: https://img.shields.io/badge/Version-1.1.0-blue?style=flat-square
[cross-platform-shield]: https://img.shields.io/badge/Cross-Platform-brightgreen?style=flat-square
[logo]: https://user-images.githubusercontent.com/46329768/120117984-597c1200-c1a5-11eb-8190-2dac8b7cbe8d.jpg
[ninja]: https://github.com/ErAz7/Ninja
[jonin]: https://github.com/ErAz7/Jonin
[ninja-release]: https://github.com/ErAz7/Ninja/releases
[jonin-release]: https://github.com/ErAz7/Jonin/releases
[noio-port-forwarding]: https://www.noip.com/support/knowledgebase/general-port-forwarding-guide/
[noip]: https://www.noip.com
[duckdns]: https://www.duckdns.org
[windows]: https://user-images.githubusercontent.com/46329768/141021000-3fe223be-f648-4aaf-8a2a-3a5d84f95d50.png
[macos]: https://user-images.githubusercontent.com/46329768/141021007-c2075401-e0e0-4451-8668-77da557bbe9b.png
[linux]: https://user-images.githubusercontent.com/46329768/142761409-badaec5e-7f02-4280-9dfb-294adc305f56.png
