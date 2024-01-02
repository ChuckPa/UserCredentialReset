
# User Credential Reset


[![GitHub issues](https://img.shields.io/github/issues/ChuckPa/UserCredentialReset.svg?style=flat)](https://github.com/ChuckPa/UserCredentialReset/issues)
[![Release](https://img.shields.io/github/release/ChuckPa/UserCredentialReset.svg?style=flat)](https://github.com/ChuckPa/UserCredentialReset/releases/latest)
[![Download latest release](https://img.shields.io/github/downloads/ChuckPa/UserCredentialReset/latest/total.svg)](https://github.com/ChuckPa/UserCredentialReset/releases/latest)
[![Download total](https://img.shields.io/github/downloads/ChuckPa/UserCredentialReset/total.svg)](https://github.com/ChuckPa/UserCredentialReset/releases)
[![master](https://img.shields.io/badge/master-stable-green.svg?maxAge=2592000)]('')
![Maintenance](https://img.shields.io/badge/Maintained-Yes-green.svg)

User Credential Reset and Server Reclaim Utility
                - for Plex Media Server


# Introduction

This utility script assists and semi-automates recovery of your Plex Media Server's credentials and account binding (claiming)
after a hard password reset or account change.

It runs on the Linux command line with 'root' privilege level so it may edit Plex's files


# Currently supported platforms (more can be added)

1. Linux (workstation & server)
2. Synology (DSM 6 and DSM 7)
3. QNAP (QTS and QuTS)
4. ASUSTOR
5. Netgear ReadyNAS
6. Western Digital (OS5 models – PR,DL, and Ultra)
7. Non-standard Linux installations, including Docker, when path to Preferences.xml is known.
8. Linux Snap  *restrictions apply

# Prerequisites

1. A Plex Media Server account at https://app.plex.tv
2. A valid server Preferences.xml containing the MachineID and ProcessedMachineID fields.
3. 'curl'  (C-url) utility for your host.
4. 'tar' or 'zip' to extract the utility script from the protective wrapper.
5. Plex Media Server stopped.

# Where to place the utility's tar file (Recommendations)
```
Vendor             | Shared folder name  |  directory
-------------------+---------------------+------------------------------------------
ASUSTOR            | Public              |  /volume1/Public
Netgear (ReadyNAS) | "your_choice"       |  "/data/your_choice"
Synology (DSM 6)   | Plex                |  /volume1/Plex             (change volume as required)
Synology (DSM 7)   | PlexMediaServer     |  /volume1/PlexMediaServer  (change volume as required)
QNAP (QTS/QuTS)    | Public              |  /share/Public
Western Digital    | Public              |  /mnt/HD/HD_a2/Public      (Does not support 'MyCloudHome' series)
Docker             | N/A                 |  N/A
Linux (wkstn/svr)  | N/A                 |  N/A
Linux (SNAP)       | N/A                 |  N/A
```

(recommend using Public shared folder)
Manual Path specification (which includes containers and custom) (See below)

# Restrictions

SNAP packages, due to their non-integration with the package managers (RPM & DPKG), can find themselves installed
on a system where another PMS instance already exists.

In consideration of this,  presence of a SNAP package is only tested *after* the mainstream package has been confirmed not to exist.
If both DPKG/RPM package and a snap package are installed concurrently,  the DPKG/RPM instance will be given priority in all cases.

# Download and extraction

1.  If you have a Linux or MacOS computer,  the shell script `UserCredentialReset.sh` can be downloaded directly.
2.  If you have a Windows computer,  you must be very careful not to damage the `.sh` file because Windows 'newline' character is different than Linux.
3.  If you want the `tar.gz` (compressed tar) or `zip` file,  download appropriately.
4.  Extraction is either:
        `tar xf UserCredentialReset.tar.gz`
    -or-
        `unzip UserCredentialReset.zip`

5.  In both cases,  you'll end up with subdirectory `UserCredentialReset-main`.
6.  You will find `UserCredentialReset.sh` there.





# How to use this tool.

### Installing the script

1.  Place the tar file, or just the script, (upload) on the host.  (See recommended locations above)

2.  Open a terminal window or SSH session to the host and sign in.
    (Windows users can use Putty utility)

3.  Elevate command line privileges to 'root'  (`sudo sh`)

4.  Extract the utility from the tar file (if appropriate)
```
    cd DIRECTORY_FROM_ABOVE
    tar xf ./UserCredentialReset.tar.gz
    ls UserCredentialReset.*
```
    You will see file `UserCredentialReset.sh`

5.  Make it executable
```
    chmod +x UserCredentialReset.sh
```

### This script can detect most configurations and therefore stop/start PMS automatically as part of the claiming process.  If it can stop & start plex for you, it will do so.  If it can't then it will make certain PMS is stopped before proceeding.


6.  Invoke the utility  `./UserCredentialReset.sh`
    -- Confirms the host platform type (so it knows what to do)
    -- Confirm you have sufficient user privilege and PMS is stopped
    -- Prompts for a "Plex Claim Token" (unless provided on the invocation command line)

7.  Open a browser tab to:    https://plex.tv/claim

8.  COPY the given token

9.  WITHIN the next 4 Minutes,     PASTE the token on the utility's command line
    (The token expires so we need be quick)

10.  Hit Enter and it will immediately complete the task. (5-20'ish seconds)
     --   Stops PMS (if supported for your configuration)
     --   Obtains new credentials for your server
     --   Writes new credentials in Preferences.xml
     --   Starts PMS (if supported for your configuration)

11.  It will print out your Plex username and email (confirmation which account PMS is claimed).

12.  Utility exits.

13.  Start PMS (if needed). It will be back to normal.


**PLEASE** don't hesitate to ask if questions or issues.


# Special Usage cases

  If your PMS configuration is not using one of the above predefined configurations,
  You may specify the path to the Preferences.xml file using the `-p` option.

  Using `-p "/path/to/Preferences.xml"` bypasses host type checking.

  However, it does confirm PMS is stopped and sufficient "root/admin" privilege is active.

  Example:
  ```
  [/tmp] # ./UserCredentialReset.sh -p "/mnt/docker/Plex/Library/Application Support/Plex Media Server/Preferences.xml"
  ```


# How it looks in use  (as seen on a QNAP)

```
[~] # chmod +x UserCredentialReset.sh
[~] # ./UserCredentialReset.sh

          Plex Media Server user credential reset and reclaim tool (QNAP)


This utility will reset the server's credentials.
It will next reclaim the server for you using a Plex Claim token you provide from https://plex.tv/claim

Using given claim token:  'claim-TE89fVWHtVjqXBPgG8z5'

Stopping PMS
Clearing Preferences.xml
Getting new credentials from Plex.tv
Claim completed without errors.
 Username: ChuckPA
 Email:    ChuckIsCrazy@loonies.com

Starting PMS
Complete.
[~] #
```

## Conclusion

When you start PMS,  it will already be claimed for you.
It will never know it was reset.
