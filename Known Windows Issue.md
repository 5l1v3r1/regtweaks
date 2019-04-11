The following _known issues_ are currently present in Windows 1809 (October Update). Before you submit any bug or feature request I expect that you read this document in order to get a short overview of what is broken and what can be manually fixed or needs an update (file changes). 

This overview is provides 'as it comes' and is not designed to explain every little 'fart', moreover it's designed to show quickly what are the 'urgent' things which are (as time of writing) considerably broken or needing a fix.


## Windows 10 Builds Overview

| Version |        Code name        |     Marketing name     | Build |
| :-----: | ----------------------- | ---------------------- | :---: |
|  1507   | Threshold 1 (TH1)       | Threshold 1            | 10240 |
|  1511   | Threshold 2 (TH2)       | November Update        | 10586 |
|  1607   | Redstone 1 (RS1)        | Anniversary Update     | 14393 |
|  1703   | Redstone 2 (RS2)        | Creators Update        | 15063 |
|  1709   | Redstone 3 (RS3)        | Fall Creators Update   | 16299 |
|  1803   | Redstone 4 (RS4)        | April 2018 Update      | 17134 |
|  1809   | Redstone 5 (RS5)        | October 2018 Update    | 17763 |
|  1903   | 19H1                    | May 2019 Update      | //    |
|  1903   | 20H2                    | //     | //    |
|  1903   | 20H3                    | //      | //   |

The list is checked against:
* Windows 10 19H1 (May Update): 
* Please don't ask for Home/Pro versions!


### I can't upgrade my OS I get the "This PC can't update to Windows 10" error message
* Download the [AppRPS.zip](https://aka.ms/AppRPS) and extract the `appraiser.bat` (right-click on it and run it as admin).
* PowerShell will automatically search for possible problems and fix them.
* (**optional**) You could do mentioned method above manually, search for the `*_APPRAISER_HumanReadable.xml` file once you found the file, open it and search for `DT_ANY_FMC_BlockingApplication` which should be set to `true`.
* Repeat the process with a search for `LowerCaseLongPathUnexpanded`, this returns the path which might causes update problems, remove the path and re-start the update procedure. 


Problem | Description | Workaround | Fix | Additional Information 
--- | --- | --- | --- | --- | 
[Abusing Exchange: One API call away from Domain Admin](https://dirkjanm.io/abusing-exchange-one-api-call-away-from-domain-admin/) | Unpatched 0-Day avbl. the issue is not fixed | reg delete HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa /v DisableLoopbackCheck /f | // | Microsoft needs to fix the hole in Microsoft Exchange Server
False Notifications in Action Center | Windows 10 Notifications & Action Center notification messages may show a mismatch | Via PowerShell `Get-AppxPackage | % { Add-AppxPackage -DisableDevelopmentMode -Register “$($_.InstallLocation)\AppxManifest.xml” -verbose }` or delete `Usrclass.dat` file | // | //
Flac files can't be played | Metadata being cut short in File Explorer and other locations | // | Fixed in 19H1 build | Will be fixed next patchday (unconfirmed?) 
Event ID 1534 | User Profile Service Event 1534 warnings appear in Event Manager | See [here](https://social.technet.microsoft.com/Forums/en-US/50a24520-2ea6-47e7-995b-c2de46d2401d/user-profile-service-event-1534?forum=win10itpronetworking) | // | //
Mapped network drive may fail to reconnect | You're unable to connect or re-connect to your network drive(s) | An [official workaround exist](https://support.microsoft.com/en-us/help/4471218/mapped-network-drive-may-fail-to-reconnect-in-windows-10-version-1809) | KB4469342 (rolled-out Dec. 2018) | //
I'm getting Intel Audio Controller version 9.21.0.3755 Update | You get the Intel Audio Driver without any need via WUS | Uninstall it manually, see [here](https://blogs.msdn.microsoft.com/matthew_van_eerde/2018/10/12/if-windows-update-sent-you-intel-audio-controller-version-9-21-0-3755-by-mistake-uninstall-it/) or exclude the drivers/updates via [WuMgr](https://github.com/DavidXanatos/wumgr) | // | //
HpqKbFiltr.sys BSOD after KB4464330 + KB4462919 | After using the updates while using HP printers/drivers you might receive a Bluescreen | ren C:\Windows\System32\drivers\HpqKbFiltr.sys HpqKbFiltr.sys_old | [x64](http://download.windowsupdate.com/c/msdownload/update/software/crup/2018/10/hpqkbfiltrremove_160aa8311398d21242ef673ff567b30da5a1a315.exe) + [x86](http://download.windowsupdate.com/c/msdownload/update/software/crup/2018/10/hpqkbfiltrremove_160aa8311398d21242ef673ff567b30da5a1a315.exe) | [Reddit](https://www.reddit.com/r/Windows10/comments/9n0bkw/one_of_these_quality_updates_can_cause_an/)
Windows 10 Build 1809 deleted all my libary files | Files located under e.g. /Documents/ getting deleted during a feature upgrade | Make sure [KB4464330](https://support.microsoft.com/en-us/help/4464330/windows-10-update-kb4464330) is installed/integrated (make sure you search for updates during the upgrade). | Not fixable, but you can try to recover your files via e.g. [Recuva](https://www.ccleaner.com/recuva) | Reported over [MS forums](https://answers.microsoft.com/en-us/windows/forum/windows_10-files/windows-10-october-2018-update-version-1809/1a924008-ddba-48db-a96f-7b4bfef9039a) and MS pulled the build in the meantime and released a refresh build.
LIPs (Language Interface Packs) aren't avbl. anymore | Microsoft removed the LIP's function in order to install additional lp's | // | // | LXP (Local Experience Packs) replacing the LIPs 
Handwriting gets stored in a seperate file | Every touch/handwriting input is stored in a index file called '[WaitList.dat](https://b2dfir.blogspot.com/2016/10/touch-screen-lexicon-forensics.html)' which can be recovered (including passwords etc) | Delete `C:\Users\%User%\AppData\Local\Microsoft\InputPersonalization\TextHarvester\WaitList.dat` manually or use [wlrip.exe](https://github.com/B2dfir/wlrip) ([wlrip](https://github.com/B2dfir/wlrip)) | // | MS needs to fix it  
Game stuttering | Game is starting to stutter if your OS is low on memory and doesn't free it's resources | [Script](https://www.reddit.com/r/Windows10/comments/9e1yy4/when_will_the_free_memory_bug_bug_be_fixed/) | Needs confirmation by MS | It's unclear if it's really a 'bug' 
Edge, Apps and Store getting no updates or can't connect | Whenever Ipv6 is disabled the apps might not getting any connection | [Enable IPv6 in your network adapter](https://i.imgur.com/iTiBhqh.png) | // | MS needs to verify and fix it 
My provider doesn't offer Printer Drivers | Since 1809 each print driver comes over WSUS, the print driver standards are now [Mopria](https://mopria.org/de/what-is-mopria) | You can only download the driver online or via Mopria (this is often offered by the manufracturer) as "normal" driver update | // | [This is by design](https://techcommunity.microsoft.com/t5/Windows-IT-Pro-Blog/What-s-new-in-printing-in-Windows-10-version-1809/ba-p/267182) 


## Install Language Packs on Build 1809 or higher

Language packs will still be avbl. via [seperated .iso files](https://www.microsoft.com/de-de/store/collections/localexperiencepacks?rtc=1).

1.) Mount the install.wim via:
`dism.exe /mount-wim /wimfile:C:\install.wim /index:1 /mountdir:C:\Mount`

2.) Mount the registry otherwise you're not able to integrate the new language via 
`Reg Load HKLM\TempImg C:\Mount\Windows\system32\config\SOFTWARE`

3.) Insert the following registry key:
`Reg Add HKLM\TempImg\Policies\Microsoft\Windows\Appx /t REG_DWORD /f /v "AllowAllTrustedApps" /d "1"`

4.) Unmount the registry:
`Reg unload HKLM\TempImg`

5.) Integrated the [LXP.Appx](https://docs.microsoft.com/en-us/powershell/module/dism/add-appxprovisionedpackage?view=win10-ps) for example: 
`Dism.exe /image:C:\Mount /add-provisionedappxpackage /packagepath:C:\LXP_APPX\Microsoft.LanguageExperiencePackde-DE_17761.1.1.0_neutral__8wekyb3d8bbwe.Appx /SkipLicense`

6.) Unmount the image
`Dism.exe /unmount-wim /mountdir:C:\Mount /commit`


## Spectre & Meltdown 

![Spectre and Meltdown on Intel Hardware](https://i.imgur.com/WPiOGpZ.png)

**Overview**:
* [V3a](https://developer.arm.com/support/arm-security-updates/speculative-processor-vulnerability/download-the-whitepaper) (new "Meltdown" variant) (CVE-2018-3640: "Rogue System Register Read (RSRE)")
* [V4](https://software.intel.com/side-channel-security-support) ([CVE 2017-5715](https://www.neowin.net/news/spectre-variant-4-disclosed-mitigations-to-result-in-another-performance-hit)) (CVE-2018-3639: "Speculative Store Bypass (SSB)")
* [L1TF](https://www.intel.com/content/www/us/en/security-center/advisory/intel-sa-00161.html) (CVE-2018-3620, CVE-2018-3646: "L1 Terminal Fault")

[Supported CPU's](https://support.microsoft.com/en-us/help/4100347/intel-microcode-updates-for-windows-10-version-1803-and-windows-server):

+ (106A5) Nehalem EP & Nehalem WS
+ (106E5) Lynnfield
+ (106E5) Lynnfield Xeon
+ (20655) Arrandale
+ (20655) Clarkdale
+ (206A7) Sandy Bridge
+ (206A7) Sandy Bridge Xeon E3
+ (206D7) Sandy Bridge Server EN/EP/EP4S
+ (206E6) Nehalem EX
+ (206F2) Westmere EX (EGL, WSM)
+ (306A9) Gladden
+ (306A9) Ivy Bridge
+ (306A9) Ivy Bridge Xeon E3
+ (306C3) Haswell (including H, S)
+ (306C3) Haswell Xeon E3
+ (306D4) Broadwell U/Y
+ (306E7) Ivy Bridge Server EX
+ (306F4) Haswell Server EX
+ (40651) Haswell ULT
+ (40661) Haswell Perf Halo
+ (40671) Broadwell H 43e
– (406E3) Skylake U/Y & Skylake U23e
+ (50654) Skylake D (Bakerville)
+ (50654) Skylake Server
+ (50654) Skylake W
+ (50654) Skylake X (Basin Falls)
+ (50662) Broadwell DE V1
+ (50663) Broadwell DE V2,V3
+ (50664) Broadwell DE Y0
+ (50665) Broadwell DE A1
+ (506C2) Broxton
+ (506C9) Apollo Lake D0
+ (506CA) Apollo Lake E0
– (506E3) Skylake H/S & Skylake Xeon E3
+ (506F1) Denverton (GLM)
+ (806EA) Coffee Lake U43e
+ (806EA) Kaby Lake Refresh U 4+2
+ (906E9) Kaby Lake G/S/X/G
+ (906E9) Kaby Lake Xeon E3

Old:
+ [14393+] (806E9) Kaby Lake U/Y & Kaby Lake U23e
+ [14393+] (906EA) Coffee Lake H (6+2) & Coffee Lake S (6+2)
+ [14393+] (906EA) Coffee Lake S (6+2) Xeon E
+ [14393+] (906EA) Coffee Lake S (4+2) Xeon E
+ [14393+] (906EA) Coffee Lake S (6+2) x/KBP
+ [14393+] (906EB) Coffee Lake S (4+2)

Patched .dll's:

* mcupdate_GenuineIntel.dll
* mcupdate_AuthenticAMD.dll

Windows has the ability to warm patch microcode on boot using the mcupdate_GenuineIntel.dll and mcupdate_AuthenticAMD.dll drivers (located at C:\Windows\System32) on boot, for Intel and AMD cpu's respectively.

## Activate the Meltdown + Spectre Patches via Registry manually (requires a reboot):

### Meltdown + Spectre Patches activation (Default under Windows 10)
* Meltdown-Fix: Activated (secure)
* Spectre-Fix: Activated (secure)
* Performance: Bad/Medium

```bash
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management]
"FeatureSettingsOverride"=dword:00000000
"FeatureSettingsOverrideMask"=dword:00000003
```

### Deactivate only Spectre Patch
* Meltdown-Fix: Activated (secure)
* Spectre-Fix: Deactivated (secure)
* Performance: Medium

```bash
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management]
"FeatureSettingsOverride"=dword:00000001
"FeatureSettingsOverrideMask"=dword:00000003
```

### Deactivate all Patches
* Meltdown-Fix: Deactivated (insecure)
* Spectre-Fix: Deactivated (insecure)
* Performance: High

```bash
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management]
"FeatureSettingsOverride"=dword:00000003
"FeatureSettingsOverrideMask"=dword:00000003
```

[Speculation Control Validation PowerShell Script](https://gallery.technet.microsoft.com/scriptcenter/Speculation-Control-e36f0050) - Official MS PowerShell script to control the Registry toggles and check the current status.  


**Notice**:

On AMD systems you _can block the update_ via [wushowhide.diagcab](https://support.microsoft.com/de-de/help/3073930/how-to-temporarily-prevent-a-driver-update-from-reinstalling-in-window), since the update will effect your performance. But this is not recommended.  


### Intel microcode updates

OS Version | KB | Patch | Updated
--- | --- | --- | --- 
Windows 10 1507 | [KB4091666](https://support.microsoft.com/en-us/help/4091666/kb4091666-intel-microcode-updates) | [Download](https://www.catalog.update.microsoft.com/Search.aspx?q=KB4091666) v3 (Rev. 7) | 12. Mar. 2019 |
Windows 10 1511 | // | // | // |
Windows 10 1607 | [KB4091664](https://support.microsoft.com/en-us/help/4346087/kb4346087-intel-microcode-updates) | [Download](http://www.catalog.update.microsoft.com/search.aspx?q=4346087) v3 (Rev. 9) | 09. Apr. 2019 |
Windows 10 1703 | [KB4091663](https://support.microsoft.com/en-us/help/4346086/kb4346086-intel-microcode-updates) | [Download](http://www.catalog.update.microsoft.com/search.aspx?q=4346086) v3 (Rev. 8) | 09. Apr. 2019 |
Windows 10 1709 | [KB4090007](https://support.microsoft.com/en-us/help/4346085/kb4346085-intel-microcode-updates) | [Download](http://www.catalog.update.microsoft.com/search.aspx?q=4346085) v3 (Rev. 7) | 09. Apr. 2019 |
Windows 10 1803 | [KB4100347](https://support.microsoft.com/en-us/help/4346084/kb4346084-intel-microcode-updates) | [Download](http://www.catalog.update.microsoft.com/Search.aspx?q=kb4346084) v3 (Rev. 7) | 09. Apr. 2019 |
Windows 10 1809 | [KB4465065](https://support.microsoft.com/en-us/help/4465065/kb4465065-intel-microcode-updates) | [Download](http://www.catalog.update.microsoft.com/Search.aspx?q=kb4465065) v3 (Rev. 4) | 09. Apr. 2019 |


## Acknowledgments and References

* [Windows 10 release information](https://www.microsoft.com/en-us/itpro/windows-10/release-information)
* [How to temporarily prevent a driver update from reinstalling in Windows 10](https://support.microsoft.com/en-us/help/3073930/how-to-temporarily-prevent-a-driver-update-from-reinstalling-in-window)
* [How to keep apps removed from Windows 10 from returning during an update](https://docs.microsoft.com/en-us/windows/application-management/remove-provisioned-apps-during-update#registry-keys-for-provisioned-apps)
* [When Microsoft patches security vulnerabilities and when not](https://www.microsoft.com/en-us/msrc/windows-security-servicing-criteria?rtc=2) - see also [MSRC (.pdf)](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE2A3xt)
* [Intel Microcode Update Guidance (.pdf)](https://newsroom.intel.com/wp-content/uploads/sites/11/2018/04/microcode-update-guidance.pdf)
