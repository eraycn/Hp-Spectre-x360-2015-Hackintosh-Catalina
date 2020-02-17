# Hp-Spectre-x360-2015-Hackintosh-Catalina
This is the result of my experiment for creating a fully working dual booting Win/macOS hackintosh out of my 2015 model HP Spectre x360 - 13-4110dx
I am putting this guide together for those who want to walk in my shoes. It is by no means qualified enough to walk you through the whole process but might give you some insights.
I will share links to more comprehensive guides that I have used for HP Spectre's at the bottom.

#Installation

1.Prepare the installation material

  1.a. At the time I chose to download a premade image from the following link, i think at this time it has the only one that had no bug while I tried to boot the installer.
        https://www.olarila.com/topic/6278-new-olarila-images/
  1.b. Find and follow the installation guidelines on the following page.
        https://www.olarila.com/topic/5794-guide-install-macos-with-olarila-image-step-by-step-install-and-post-install-windows-or-mac/
  1.c. I stopped following the guide after step 7. 
  
2.Create your own EFI Folder
  2.a. DSDT
    2.a.1. You have to spend some time on the forums to be able to understand your way around the DSDT file, read through the following link;
            https://www.tonymacx86.com/threads/guide-patching-laptop-dsdt-ssdts.152573/
            
    2.a.2. After you get the basics of decompiling and compiling by using Rehabman's version of the MaciASL, first you have to get rid of the errors, search google franticly to find a similar error to yours, and finally once you get rid of the red error warning and left with yellow exclamation marks of warnings you are good to go with patching your DSDT file, apply the following patches;
        
        [sys] Fix _WAK Arg0 v2
        [sys] Fix Mutex with non-zero SyncLevel
        [bat] Battery Fix (battery_HP-G6-2221ss)
        [sys] HPET Fix
        [sys] SMBUS Fix
        [sys] IRQ Fix
        [sys] RTC Fix
        [sys] OS Check Fix (Windows 10 in my case)
        
    2.a.3. You have two more patches to make;
       2.a.3.a To control brightness keys (F2 and F3) 
         
         into method label _Q10 replace_content
          begin
          // Brightness Down\n
              Notify(\_SB.PCI0.LPCB.PS2K, 0x0205)\n
              Notify(\_SB.PCI0.LPCB.PS2K, 0x0285)\n
          end;
        into method label _Q11 replace_content
        begin
          // Brightness Up\n
              Notify(\_SB.PCI0.LPCB.PS2K, 0x0206)\n
              Notify(\_SB.PCI0.LPCB.PS2K, 0x0286)\n
          end;
       2.a.3.b To enable touchscreen through the VoodooI2C kext, follow the following guide;
           https://github.com/alexandred/VoodooI2C/blob/master/Documentation/Installation.md
    
    2.a.4. Once you are done with the patches compile your file and save it with an aml extension, if you happen to have a trouble with compiling the file while there is no errors, try downloading a newer version of the MaciASL, worked for me.
    2.a.5. Put the patched DSDT.aml under the EFI\Clover\ACPI\patched\ folder
  
  2.b Manage Devices
    2.b.1 Screen Backlight Fix
       https://www.tonymacx86.com/threads/guide-laptop-backlight-control-using-applebacklightfixup-kext.218222/
       
    2.b.2.Broadcom BCM94352Z card
       If you purchased one and replaced it with the existing (to have WiFi), to make it work follow install this kexts 
       BrcmFirmwareRepo.kext
       BrcmFirmwarePatchRam2.kext
       from https://github.com/RehabMan/OS-X-BrcmPatchRAM

    2.b.3 For the audio, use AppleALC
      https://github.com/acidanthera/AppleALC/releases
      Devices>Audio> Inject > 3 (config.plist)
      
    2.b.4 For the touchscreen
      https://github.com/alexandred/VoodooI2C/blob/master/Documentation/Installation.md
   
    2.b.5 For the touchpad
      https://github.com/alex-spataru/hackintosh-spectre-4101dx (of all different versions of VoodooPS2Controller this repository worked the best)
    
   2.c Download Kexts
     2.c.1 For this part I will share my EFI folder, you can download and check it. 
3. Move your EFI folder to your EFI drive
  3.a Replace the files in the bootdrive with the EFI folder that you have created
4. (Optional) Move Kexts under L/E (Library/Extensions) for installing and uninstalling kexts from Windows, your USB installation and macOS terminal, here are some useful links;
    Install Kexts
    https://www.tonymacx86.com/threads/guide-installing-3rd-party-kexts-el-capitan-sierra-high-sierra-mojave-catalina.268964/
    Uninstall Kexts
    https://nektony.com/how-to/remove-kext-on-mac
    How to manage EFI partition from Windows
    https://superuser.com/questions/965751/how-to-access-efi-partition-on-windows-10
 5. Enjoy!
 
 Bonus
 
 Here is some guides that you need to read through before starting this painful process;
 -https://www.vice.com/en_us/article/8xznw4/how-to-make-a-hackintosh-laptop
 -https://www.tonymacx86.com/threads/guide-high-sierra-on-hp-spectre-x360-8th-gen-coffee-lake.251330/
 -https://github.com/alex-spataru/hackintosh-spectre-4101dx
