# Remove McAfee from Intune Enrolled Devices

This repository is a little mix from another repository that you can use to remove McAfee via Intune. I've created this as the original repo pre-supposes certain 365 license tiers and thus access to certain features that not everyone may have. This repo is a half mirror of https://github.com/nullifyac/McAfee_Removal and is primarily created to help anyone else with my situation.

## Create the Intunewin File

To use this tool, you need to create an Intunewin file. You can obtain the tool from Microsoft's repository -> https://github.com/Microsoft/Microsoft-Win32-Content-Prep-Tool. 

Download the tool, and create your folder structure. I am using: 

```
IntuneWinAppUtil.exe
_IntuneWin/
_Package/
├─ mcafee_remediate.ps1

```

Run the tool, select `_Package` as your input, and `_IntuneWin` as your output. This will create `mcafee_remediate.intunewin` in the `_IntuneWin` folder. Select `mcafee_remediate.ps1` as your input file.

## Create the Application

Log into Intune, and create a new Win32 app. Fill out the details to your liking, then use the following settings:

**Install command:** powershell.exe -executionpolicy bypass -file mcafee_remediate.ps1

**Uninstall command:** powershell.exe exit

**Installation time required (mins):** 60

**Allow available uninstall:** No

**Install behavior:** System

**Device restart behavior:** App install may force a device restart

**Return codes:**
```
0 Success
1707 Success
3010 Soft reboot
1641 Hard reboot
```

Requirements are just sensible common sense - at least late Windows 10 or current Windows 11 (whatever your lowest supported platform is).

## Detection Rules

Use the following detection rules: 

**Rules format**: Manually configure detection rules

**Type:** File

**Path:** C:\ProgramData\Microsoft\IntuneManagementExtension\Logs

**File:** RemoveMcAfee.log

**Detection Method:** File exists

**32bit on 64bit:** No

___

So far deployment has been successful. Realistically this is just a method to run the script on remote endpoints, so uninstall and other app behavior is not required. 
