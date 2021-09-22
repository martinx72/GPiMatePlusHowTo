
# GPiMatePlusHowTo
How to make official RetroPie work with RgR GPiMate Plus for CM4,
The RetroPie image I have tested the with this toturial is RetroPie v4.7.1 Released November 4, 2020.

**1.** To have a RetroPie SD card ready. Must be the RetroPie for RPi4/Pi400 version from: https://retropie.org.uk/download/

**2.** Get the dtbo file from RetroFlag official site: https://support.retroflag.com/Products/GPi_Case/GPi_Case_patch.zip

Just **one file** we need from this zip, **do not** run the auto install script of this file from retroflag!!!

unzip the **dpi24.dtbo** of the zip file you just downloaded and rename it to **dpi24_gpi.dtbo**, 

now place this **dpi24_gpi.dtbo** in the **overlays** sub-folder of boot partition of SD card.
 
**3.** add these settings at the end of original **/boot/config.txt**
```markdown
# default CEC name
cec_osd_name=gpimateplus

avoid_safe_mode=1

# Set the bootloader delay to 0 seconds. The default is 1s if not specified.
boot_delay=0

# Disable on-board PCIe
dtoverlay=disable-pcie

# Disable on-board bluetooth
dtoverlay=disable-bt

# Disable on-board Wifi
dtoverlay=disable-wifi

dtoverlay=sdtweak,overclock_50=100

# enable USB for CM4
dtoverlay=dwc2,dr_mode=host

#enable AUDIO
disable_audio_dither=1
dtoverlay=audremap,pins_18_19
audio_pwm_mode=2

hdmi_ignore_hotplug=1
hdmi_ignore_edid=0xa5000080
hdmi_edid_file=1

dtoverlay=dpi24_gpi
overscan_left=0
overscan_right=0
overscan_top=0
overscan_bottom=0
enable_dpi_lcd=1
display_default_lcd=1
dpi_group=2
dpi_mode=87
dpi_output_format=0x6016
hdmi_timings=240 1 38 10 20 320 1 20 4 4 0 0 0 60 0 6400000 1
disable_pvt=1
```
note, to enable the WiFi/BT of CM4 (if yours has these feature builit-in), just remove **dtoverlay=disable-bt** or **dtoverlay=disable-wifi** ,

And also download the **disable-pcie.dtbo** from the current repo, and place this **disable-pcie.dtbo** in the **overlays** sub-folder of boot partition of SD card.

**4.** Now your SD card should be ready to see the video. 

But, EmulationStation and RetroArch won't be rotated correctly, we will take care EmulationStation lanuch script now to make it work right.
modify **/opt/retropie/configs/all/autostart.sh** as below
```markdown
emulationstation --screenrotate 1 --screensize 320 240
```

Now, your EmulationStation should be rotated correctly.

And don't forget to adjust the **Sound Settings** in EmulationStation, to set **Audio Device** to **Headphone**.


**5.** Time to deal with RetroArch cfg file **/opt/retropie/configs/all/retroarch.cfg**
```markdown
#to rotate screen
screen_orientation = "3"
video_allow_rotate = "true"
video_rotation = "3"
video_fullscreen_x = "240"
video_fullscreen_y = "320"

#optional for right ratio, my personal favor, might not be all acceptable for everyone
aspect_ratio_index = "21"
video_scale_integer = "true"
video_aspect_ratio = "1.0000"
```


**6**. Now, let us make even console mode worked in right orientation

add these at the very end of **/boot/cmdline.txt** 
 (please make sure everything is in **one single line**)
```markdown
splash vt.global_cursor_default=0 video=DSI-1:240x320,rotate=270
```

If you don't want any kernel message printed during booting, you can simply change below in **/boot/cmdline.txt**

> console=**tty1**

to 
>console=**tty3**




**Have fun!**
