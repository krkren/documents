---
layout: default
title: Command Line Options
---

# Command Line Options
Kirikiri's command line options can be specified from a standard command line, or saved in a configuration file using [Releaser]() ( krkrrel.exe ) or [Kirikiri Configuration]() ( -userconf ).
The order in which options are loaded is:

1. Options embedded in the Kirikiri executable itself
2. The .cf file located in the same directory as the Kirikiri core (filename is the same as the Kirikiri core)
3. The .cfu file in the data storage location output by "Engine Configuration" (-userconf) (filename is the same as the Kirikiri core)
4. Options specified on the command line

If the .cf or .cfu files do not exist, they are simply ignored. Options loaded later take higher priority.

Command line options basically consist of a '-' (hyphen) followed by the option name. This is followed by '=' and the value of the option.
For example, if the value of the -cdvol option is direct, you specify it as -cdvol=direct.

Except for "Startup Options," "Debug-related Options," and "System Compatibility Options," most are fine-tuning options to resolve environment-dependent issues.
For resolving environment-dependent issues, please also refer to [About Environment-Dependent Bugs]().


> Note
> With Releaser or -userconf, you can change these options by modifying the Kirikiri executable or external configuration files, but *normally it is fine to leave them at their defaults. It is not recommended to distribute executables or configuration files to the general public with these options changed from their defaults just because there is an issue in the creator's specific environment* (of course, there are options like -datapath that should be set according to the distribution or usage format).

Items in the list below marked as "Can be changed dynamically" are those that can be modified using the [System.setArgument]() method. Other options cannot be changed dynamically.

## Startup Options
The following options are available to call and use specific functions of Kirikiri.
* __-userconf (Launch end-user configuration tool)__
    Launches the end-user configuration tool built into the core.
* __-about (Display copyright information dialog box)__
    Displays the "Version, Copyright, and Environment Information" dialog box.
* __-sel (Display "Select Folder/Archive" dialog box)__
    Displays the "Select Folder/Archive" dialog box. Data such as data.xp3 will not be automatically detected.
    If you specify a folder as a command line parameter (without a leading hyphen), you can open the "Select Folder/Archive" dialog box with that folder selected by default.
* __-printdatapath (Output data storage location)__
    Outputs the setting of the data storage location (-datapath option) and a newline to standard output, then exits. This option is intended for use by external applications that manage save data in cooperation with the Kirikiri core.
    Special strings such as $(exepath) in the data storage location will be output in their replaced state.
    Since the Kirikiri core is a GUI application, nothing will be displayed if you simply launch the Kirikiri executable from a command prompt with the -printdatapath option. Use pipes or redirection to capture the output.
* __-startup (Specify startup script)__
    Specifies the filename of the script to be executed first.
    If not specified, startup.tjs is executed.

## General System Options
* __-datapath (Data storage location)__
    Sets the location (folder) where Kirikiri saves various data.
    The value is specified as a string.
    You can simply specify the folder name as a full path, but usually, the following special strings are embedded and used.
    * __$(exepath)__
        Replaced with System.exePath (the folder name where the Kirikiri core is located).
    * __$(appdatapath)__
        Replaced with System.appDataPath (the user's home folder). This folder is usually a hidden folder.
    * __$(personalpath)__
        Replaced with System.personalPath (the My Documents folder).
    * __$(vistapath)__
        Replaced with $(appdatapath) if the OS is Vista or later, and $(exepath) if it is earlier than Vista.
    * __$(savedgamespath)__
        Replaced with System.savedGamesPath (the folder for game save data). (Ver 1.1.0 or later)

    The default is "$(exepath)\savedata". This setting is suitable for a distribution format where the program is compressed/archived in zip etc. without using an installer, and the user extracts it and executes the program immediately.
    However, with this default setting, if the program is placed under Program Files and launched by a "Limited User" in Windows XP etc. who does not have permission to write under Program Files, an error may occur because files cannot be written.
    If you use a name like "$(appdatapath)\ApplicationName" or "$(personalpath)\ApplicationName", files will be written to a folder for each user, making such problems less likely to occur, but users might be confused because the save data location becomes less obvious.

    Kirikiri will attempt to create the data storage location specified by this option if it does not exist at startup. Even if creation fails, processing continues without exiting, so please handle errors within the user's script (by catching exceptions such as data being unable to be saved).

    Settings made in "Engine Configuration" are created inside the folder specified by this data storage location. In addition, various logs are also created in this folder by default.
* __-contfreq (Processing weight)__
    Sets whether to reduce CPU usage by calling processes such as transitions at a specified cycle while applying weights.
    Possible values are *'0' (no weight)* or a positive integer; if this option is not specified, '0' is assumed. If a positive integer is specified, you can specify the cycle in Hz.
    This option affects transitions and Continuous handlers registered with System.addContinuousHandler.
    If set to '0', the CPU is fully utilized during transitions etc.
    If a numerical value is specified, processing will occur at 그 cycle, and the remaining time will let the CPU rest. This can reduce the impact on other applications, CPU temperature rise, and computer power consumption. The lower the value specified, the higher this effect. However, specifying a low value may cause transitions etc. to become less smooth.
    If waiting for vertical synchronization is performed with the waitvsync option, the Continuous handler will be driven according to the timing of the vertical synchronization, and the setting of the contfreq option will be ignored.
    This option can be changed dynamically, but the change will take effect the next time the transition or Continuous handler operation is interrupted.
* __-memusage (Memory usage)__
    Sets the memory usage.
    Possible values are *'normal'* or *'low'*; if this option is not specified, 'normal' is assumed.
    Selecting "low" will save more memory than selecting "normal". However, selecting "low" will decrease performance because various internal cache mechanisms in Kirikiri are restricted and the size of TJS2 hash tables is limited. Also, if "low" is selected, "Graphics - Image Cache Limit" is forced to the "Do not cache" setting.
* __-timerprec (Timer precision)__
    Sets the level of timer precision.
    Possible values are *'normal'*, *'higher'*, or *'high'*; if this option is not specified, 'normal' is assumed.
    Specifying 'higher' or 'high' increases the overall precision of timers (including most things related to time and timing), which may resolve sluggishness in character display in KAG or MIDI playback, but may also decrease performance.
* __-laxtimer (Timer event capacity limit)__
    Sets whether to limit the number of timer events that can be stored in the system at once (maximum generation capacity) to avoid situations where too many timer events accumulate and cannot be processed.
    Possible values are *'no'* or *'yes'*; if this option is not specified, 'no' is assumed.
    On very slow computers or in situations where very heavy processes are driven by timers, Kirikiri may become unresponsive to operations because it cannot keep up with events generated by the timer. If 'yes' is specified for this option, the maximum generation capacity of timer events stored in the system is always fixed to 1 (the state where the capacity property of the Timer class is 1). This can suppress the generation of timer events that the system cannot process, but normally, timer precision and accuracy are lost.
* __-lowpri (Low priority)__
    Sets whether to lower the priority during transitions etc.
    Possible values are *'no'* or *'yes'*; if this option is not specified, 'no' is assumed.
    If set to 'yes', the execution priority of the main thread will be lowered when the main thread of Kirikiri uses the CPU continuously, such as during transitions. This may improve symptoms such as sound skipping during transitions or other applications becoming difficult to operate during transitions.
* __-exceptionexe (Editor to launch on exception)__
    Specifies the editor to launch when an exception occurs.
* __-exceptionarg (Editor options on exception)__
    Specifies the options to be specified when launching the editor when an exception occurs.
    You can simply specify arguments, but usually, the following special strings are embedded and used.
    * __%filepath%__
        Replaced with the file path of the script where the exception occurred.
    * __%line%__
        Replaced with the line number where the exception occurred.


## Input-related Options
* __-wheel (Mouse wheel rotation detection method)__
    Sets how to detect mouse wheel rotation.
    Possible values are *'no' (do not use)*, *'dinput' (DirectInput)*, or *'message' (window message)*; if this option is not specified, 'dinput' is assumed.
    Selecting "do not use" makes the mouse wheel unusable. Selecting "DirectInput" uses DirectInput. Selecting "window message" detects mouse wheel rotation without using DirectInput. Changing the setting may improve mouse wheel-related bugs.
* __-joypad (Gamepad availability)__
    Sets whether to use a gamepad (joystick).
    Possible values are *'no' (do not use)* or *'dinput' (use)*; if this option is not specified, 'dinput' is assumed.
    Selecting "do not use" makes the pad unusable. Set to "do not use" if the pad cannot be detected correctly or if the pad cannot be used normally.
* __-paddelay (Pad key repeat delay)__
    Specifies the time until key repeat for the gamepad (joystick) in milliseconds.
    Possible values are a positive number or -1; specifying -1 disables key repeat. If this option is not specified, 500 is assumed.
    This option can be changed dynamically.
* __-padinterval (Pad key repeat interval)__
    Specifies the interval for key repeat for the pad (joystick) in milliseconds. The smaller the value, the faster the repeat.
    Possible values are a positive number; if this option is not specified, 30 is assumed.
    This option can be changed dynamically.
* __-controlime (IME state control)__
    Sets whether to perform state control (such as enabling or disabling) of the IME (conversion input software for Japanese etc.).
    Possible values are *'yes' (perform)* or *'no' (do not perform)*; if this option is not specified, 'yes' is assumed.
    Selecting "do not perform" may avoid bugs such as "unable to input languages like Japanese that are input through IME."

## Sound-related Options
* -wsdecpri (PCM decode thread priority)
    The priority of the thread that performs PCM decoding.
    Possible values are '0' (Idle (lowest)), '1' (Low), '2' (Below Normal), '3' (Normal), '4' (Above Normal), '5' (High). If this option is not specified, '1' is assumed.
    Increasing the priority may reduce sound skipping during playback of PCM (uncompressed wave, OggVorbis, etc.), but transitions may become less smooth or responsiveness to operations may worsen.
    By the way, what is specified here is the priority for decoding during normal times; in emergencies (when the remaining data in the buffer becomes short), the necessary priority is automatically secured.
* -wssoft (DirectSound software mixing)
    Sets whether to perform mixing using software in DirectSound.
    Possible values are 'yes' (perform software mixing) or 'no' (do not perform software mixing). If this option is not specified, 'yes' is assumed.
    In the standard setting, mixing is performed in software, so the CPU load is higher, but the possibility of avoiding hardware-specific bugs is higher. If there is no problem even if 'no' is specified for this option (even if mixing is performed in hardware), the CPU load may be lowered. With USB audio or inexpensive sound cards, mixing may always be performed by the CPU, so changing this option may have no effect.
* -wsrecreate (DirectSound secondary buffer recreation)
    Sets whether to always recreate the secondary buffer in DirectSound.
    Possible values are 'yes' (always recreate) or 'no' (recreate as needed). If this option is not specified, 'no' is assumed.
    In Kirikiri, a secondary buffer once created is reused if conditions such as the number of channels and sampling frequency are the same, but if 'yes' is specified, it will always be recreated without being reused. Depending on the environment, instabilities such as sound skipping or cutting at the start of playback may be improved.
* -wsl1len (DirectSound secondary buffer length)
    Sets the length of the DirectSound secondary buffer.
    Possible values are integers, specified in milliseconds. A minimum of 250ms is required. If this option is not specified, 1000 is assumed.
    What is specified here is the length of the buffer actually secured as the DirectSound secondary buffer.
    Generally, taking it longer makes playback stable but consumes memory.
* -wsl2len (DirectSound secondary auxiliary buffer length)
    Sets the length of the secondary buffer for the DirectSound secondary buffer.
    Possible values are integers, specified in milliseconds. A minimum of 250ms is required. If this option is not specified, 1000 is assumed.
    Kirikiri creates an auxiliary buffer for each DirectSound secondary buffer, accumulates decoded data in this auxiliary buffer once, and then transfers it to the secondary buffer. The buffer length specified here is the length of that auxiliary buffer.
    Normally, decoding and accumulation in the auxiliary buffer are performed in a low-priority thread, but transfer from the auxiliary buffer to the secondary buffer is performed in a high-priority thread.
    Generally, taking it longer makes playback stable but consumes memory. Also, if control that adds changes to the decoding process is performed, the delay until it is actually sounded becomes longer.
* -wsmute (DirectSound mute)
    Sets whether to mute (lower the volume) when the application is inactive or minimized in DirectSound.
    Possible values are 'never' (do not mute), 'minimize' (when minimized), or 'deactive' (when inactive). If this option is not specified, 'never' is assumed.
    If 'never' is selected, muting is not performed. With 'minimize' or 'deactive', it is muted when the application is minimized or becomes inactive, respectively.
    Only things played with WaveSoundBuffer (in the case of KAG, when 'Wave' is used for BGM, and sound effects) are muted; MIDI and CDDA playback are not muted.
* -wsmutevol (DirectSound mute volume)
    Sets the volume when muted with -wsmute (DirectSound mute).
    Possible values are integers, specified in %.
    Specifying "0%" results in complete silence, and specifying "50%" results in half volume (approx. -6dB).
* -wsforcecnv (DirectSound forced format conversion)
    Sets whether to forcibly convert PCM data to be played by DirectSound into a specified format.
    Possible values are 'none' (do not convert), 'i16' (convert to 16-bit integer), or 'i16m' (convert to 16-bit integer mono). If this option is not specified, 'none' is assumed.
    Changing the setting may improve playback failures. If 'i16m' is selected, the -wsexpandquad option (DirectSound forced 4ch playback) setting is ignored.
* -wsuse3d (DirectSound 3D control)
    Sets whether to perform 3D control in DirectSound.
    Possible values are 'no' (do not), 'yes' (do). If this option is not specified, 'no' is assumed.
    If 'yes' is selected, 3D control of sound is enabled, and WaveSoundBuffer.posX, WaveSoundBuffer.posY, and WaveSoundBuffer.posZ properties become effective (these properties are already implemented in the current version but are unsupported).
    Also, in many environments, if 'yes' is selected, stereo or mono sound will be expanded and played on surround speakers by DirectSound3D (for example, sound that was only played on the front speakers will be played on all speakers in a 5.1ch environment).
    If 'yes' is selected, the -wsexpandquad option (DirectSound forced 4ch playback) setting is ignored.
* -wsexpandquad (DirectSound forced 4ch playback)
    Sets whether to forcibly play stereo or mono sound on 4 channels including rear speakers when playing in DirectSound.
    Possible values are 'no' (do not), 'yes' (do). If this option is not specified, 'no' is assumed.
    If 'yes' is set, sound can be played on both front and rear speakers even in environments where stereo or mono sound is only played on front speakers.
* -wsfreq (DirectSound primary buffer frequency)
    Sets the playback frequency of the DirectSound primary buffer.
    Possible values are positive natural numbers representing the frequency in Hz. If this option is not specified, '44100' is assumed.
    Especially in environments using WDM-based sound drivers (Windows 2000, XP or later, etc.), there may be no change in the playback state even if the setting is changed.
* -wsbits (DirectSound primary buffer bit depth)
    Sets the playback bit depth of the DirectSound primary buffer.
    Possible values are 'i8' (8-bit integer), 'i16' (16-bit integer), 'i24' (24-bit integer), 'i32' (32-bit integer), 'f32' (32-bit floating point). If this option is not specified, 'i16' is assumed.
    Especially in environments using WDM-based sound drivers (Windows 2000, XP or later, etc.), there may be no change in the playback state even if the setting is changed.
* -wscontrolpri (DirectSound primary buffer playback control)
    Sets whether to perform play/stop control for the DirectSound primary buffer.
    Possible values are 'yes' (perform), 'no' (do not perform). If this option is not specified, 'yes' is assumed.
    In rare cases, there seem to be environments where sound skipping or cutting is improved by changing the setting.
* -wspritry (DirectSound primary buffer setting trial level)
    Sets how many settings to try when specifying the format of the DirectSound primary buffer.
    Possible values are '0' - '2' (Level 0 - Level 2) or 'all' (all). If this option is not specified, 'all' is assumed.
    If Level 0 is specified, it tries to set the format to stereo 16-bit integer.
    If Level 1 is specified, before trying Level 0, it tries to set the format to 16-bit integer with the number of channels according to the system speaker settings.
    If Level 2 is specified, before trying Level 1, it tries to set the format using the WAVEFORMATEX structure with the bit depth specified in "DirectSound primary buffer bit depth" and the number of channels according to the system speaker settings.
    If "all" is specified, before trying Level 2, it tries to specify the format using the WAVEFORMATEXTENSIBLE structure with the same settings as Level 2.

## Graphic-related Options
* -gclim (Image cache limit)
    Sets the maximum value of memory used for image cache.
    Possible values are 'auto' (automatic) or an integer value; if an integer value is specified, specify the memory used for image cache in MB. If this option is not specified, 'auto' is assumed.
    Kirikiri has a mechanism to cache images so that images once read can be accessed quickly. Specify the limit value of memory used for that.
    If 'auto' is specified, the value is automatically determined by the amount of physical memory installed in the computer.
    If '0' is specified, caching is not performed.
    If swapping occurs frequently during Kirikiri execution, specifying a small value or '0' may improve it.
* -fsbpp (Color mode in fullscreen)
    Sets the color mode in fullscreen.
    Possible values are 'nochange' (do not change), '16' (16-bit color), '24' (24-bit color), or '32' (32-bit color). If this option is not specified, 'nochange' is assumed.
    If 'nochange' is specified, it will be the same color mode as the non-fullscreen color mode.
    This option can be changed dynamically, but the value will take effect the next time you try to go fullscreen.
* -fsres (Screen resolution in fullscreen)
    Sets the screen resolution in fullscreen.
    Possible values are 'auto' (automatic), 'proportional' (resolution with the same aspect ratio), 'nearest' (closest resolution), or 'nochange' (do not change resolution). If this option is not specified, 'auto' is assumed.
    If 'auto' is selected, the most suitable screen resolution is automatically selected and used. In this case, among resolutions with the same aspect ratio, if there is a resolution that fits the resolution specified in the program, it is selected, but if there is no such resolution, the engine performs enlarged display without changing the resolution. In the case of this setting, even if 'no' (do not) is specified for the -fszoom (enlarged display by engine in fullscreen) option, it is always considered to be 'outer' (fit within the monitor).
    If 'proportional' is selected, among resolutions with the same aspect ratio as the non-fullscreen state, the resolution that is the same as or larger than the resolution specified in the program and closest to it is selected.
    If 'nearest' is selected, the resolution that is the same as or larger than the resolution specified in the program and closest to it is selected, but there is no guarantee that a resolution with the same aspect ratio as the non-fullscreen state will be selected. This setting may be suitable for CRT monitors or LCD monitors that support enlarged display while maintaining the screen aspect ratio.
    If 'nochange' is selected, the resolution will not be changed, remaining at the non-fullscreen resolution.
    This option can be changed dynamically, but the value will take effect the next time you try to go fullscreen.
* -fszoom (Enlarged display by engine in fullscreen)
    Specifies how the engine performs screen enlargement in fullscreen.
    Possible values are 'inner' (fit within the monitor), 'outer' (expand to fill the monitor), or 'no' (do not). If this option is not specified, 'inner' is assumed.
    If 'inner' is selected, enlargement by the engine is performed if necessary. "If necessary" means when the screen resolution is different from the resolution specified in the program (if the screen resolution is lower than the resolution specified in the program, it will be a reduction process). At this time, enlargement is performed while maintaining the aspect ratio of the resolution specified in the program, but if the aspect ratio of the monitor and this aspect ratio are different, gaps may appear at the top and bottom or left and right. These gaps are always displayed in a completely black state.
    If 'outer' is specified, as with 'inner', enlargement by the engine is performed if necessary. However, unlike 'inner', if the aspect ratio of the monitor and the aspect ratio specified in the program are different, enlargement is performed to the maximum extent so that gaps do not appear at the top and bottom or left and right. For this reason, gaps do not appear, but the screen may protrude outside the monitor. With this setting, for example, when displaying 16:9 content on a 16:10 monitor, it becomes possible to display it enlarged to the maximum extent without showing gaps. Of course, this results in areas protruding to the left and right, so if you are producing content assuming such a display, measures such as not displaying important UI or characters in the protruding parts are necessary.
    If 'no' is selected, enlargement by the engine is not performed even if necessary. In this case, even if the screen resolution is different from the resolution specified in the program, enlargement by the engine side is not performed (as a result, the image may be displayed small in the center of the screen).
    If the native resolution of the monitor and the resolution of the signal output by the graphics card are different, LCD monitors etc. may perform enlarged display on the monitor side, but if enlarged display is performed on the engine side after enlargement processing on the monitor side, enlargement will be performed twice, and the image may become dirty, so please be careful (the "auto" option of -fsres automatically selects a combination that prevents such double enlarged display).
    This option can be changed dynamically, but the value will take effect the next time you try to go fullscreen.
* -gsplit (Split processing of image operations)
    Sets whether to perform image operations by splitting them finely.
    Possible values are 'yes' (perform), 'int' (interlace split), 'bidi' (bidirectional split), or 'no' (do not perform). If this option is not specified, 'yes' is assumed.
    Kirikiri performs operations while splitting into fine areas when drawing images in order to use the CPU's memory cache effectively. Depending on the environment, this seems to lead to screen flickering. In such environments, specifying 'no' may suppress flickering, but drawing performance may also decrease. If double buffering is enabled, the meaning of not performing split processing is thin, so it is recommended to set it to perform split processing.
    If 'int' is specified, image operation units are processed every other one, but stripes may be visible when the screen is updated. If 'bidi' is selected, the order of image operations repeats top-to-bottom and bottom-to-top (in the case of 'yes', it is always top-to-bottom).
* -smoothzoom (Smoothing during enlarged display)
    Sets whether to perform smoothing (interpolation during enlargement) when performing enlarged display of display content with Window.setZoom etc., or when Kirikiri performs enlarged (reduced) display of the screen with the -fsres option (this is unrelated to enlargement/reduction with Layer.affineCopy etc.).
    Possible values are 'no' (do not perform) or 'yes' (perform). If this option is not specified, 'yes' is assumed.
    Performing smoothing makes the image smooth but slightly blurred. Not performing smoothing makes the image sharp but jaggedness becomes noticeable.
    Depending on the environment, performance may decrease if smoothing is not performed. Also, there may be environments where smoothing does not work.
    Some third-party drawing devices (devices set with the Window.drawDevice property) may not be affected by this option.
    This option can be changed dynamically, but there is no guarantee that the value will take effect immediately.
* -aamethod (Anti-aliased character drawing method)
    Sets the anti-aliased character drawing method.
    Possible values are 'auto' (automatic), 'res4' (resampling 4x4), 'res8' (resampling 8x8), or 'api' (Windows API). If this option is not specified, 'auto' is assumed.
    In the case of 'auto', 'api' is automatically selected in the current version.
    With 'res4' or 'res8', characters are drawn at several times the size (4x4 or 8x8) and then reduced to achieve anti-aliasing. res4 is faster than res8, but precision is lower.
    With 'api', anti-aliased characters are drawn using the GetGlyphOutline API, but it seems to be an API with many inconveniences and may not draw correctly depending on the environment.
* -jpegdec (JPEG image decoding precision)
    Sets the precision of decoding (expansion) of JPEG images.
    Possible values are 'high' (high), 'normal' (standard), or 'low' (low). If this option is not specified, 'normal' is assumed.
    Specifying 'high' makes decoding slow but image quality high. Specifying 'low' makes decoding fast but image quality low. However, there is almost no difference in appearance.
* -drawthread (Number of drawing threads)
    Sets the number of threads to use during drawing processing.
    Possible values are any numerical value or 'auto' (automatic). If this option is not specified, '1' is assumed.
    If 'auto' is specified, the same number of threads as the number of processors recognized by the OS is automatically assigned.
    By setting multiple drawing threads, drawing performance in a multi-core environment may be improved, but conversely, performance may also decrease.
    Good results may be obtained by applying it to processes with large drawing areas, high-load Affine-related processes, layer synthesis processes with heavy operations, etc.
    Even if set to use multi-threading, if the system determines that the drawing processing load is light and the effect of multi-threading cannot be obtained, it may not be executed in multi-threading.
* -bitmapallocator (Bitmap memory allocation method Ver 1.1 or later)
    Specifies how to allocate memory for bitmaps.
    Possible values are 'globalalloc' (allocate with GlobalAlloc), 'separateheap' (use separate heap), or 'malloc' (use malloc). If this option is not specified, 'globalalloc' is assumed.
    Using separateheap may reduce memory fragmentation and avoid memory shortage error problems.
* -bitmapheapsize (Initial separate heap size Ver 1.1 or later)
    Specifies the initial size when "use separate heap" is selected for the bitmap memory allocation method.
    Possible values are 'auto' (automatic (recommended)), '0' (expand automatically), '64' (64MB), '128' (128MB), '256' (256MB), '512' (512MB), '1024' (1024MB), or '2048' (2048MB). If this option is not specified, 'auto' is assumed.
    Normally auto is fine, but adjusting the initial value may reduce memory fragmentation and avoid memory shortage error problems.

## CPU Feature-related Options
For all the following options, possible values are 'yes' (use if available), 'no' (do not use even if available), or 'force' (force use). If the option is not specified, 'yes' is assumed.
If a CPU recognition problem occurs, setting it to 'no' will disable that feature.
'force' will force the use of that CPU feature even if it is not detected, but of course, it will not work correctly if the CPU does not have that feature.
Only the -cpummx -cpucmov -cpusse -cpuemmx options affect the Kirikiri core itself. The OggVorbis decoder (wuvorbis.dll) is affected by the -cpusse, -cpummx, and -cpu3dn options. Other (third-party) plugins may also be affected by CPU feature settings.
* __-cpummx (MMX)__
* __-cpu3dn (3DNow!)__
* __-cpusse (SSE)__
* __-cpucmov (CMOVcc)__
* __-cpue3dn (Enhanced 3DNow!)__
* __-cpuemmx (EMMX (MMX2))__
* __-cpusse2 (SSE2)__
* __-cpusse3 (SSE3)__
* __-cpussse3 (SSSE3)__
* __-cpusse41 (SSE4.1)__
* __-cpusse42 (SSE4.2)__
* __-cpusse4a (SSE4a)__
* __-cpuavx (AVX)__
* __-cpuavx2 (AVX2)__
* __-cpufma3 (FMA3)__
* __-cpuaes (AES)__

## Debug-related Options
* -debug (Debug mode)
    Sets whether to operate Kirikiri in debug mode ( -> Debug ).
    Possible values are 'no' (disabled) or 'yes' (enabled). If this option is not specified, 'no' is assumed.
    When enabled, Kirikiri operates in debug mode and several debug support functions are enabled, but execution speed is lower than in normal mode.
* -forcelog (Log to file)
    Sets whether to output console logs to a file.
    Possible values are 'no' (do not output), 'yes' (append to existing file), or 'clear' (clear existing file before outputting). If this option is not specified, 'no' is assumed.
* -logerror (Log to file on error)
    Sets whether to output console logs to a file when an error occurs.
    Possible values are 'no' (do not output), 'yes' (append to existing file), or 'clear' (clear existing file before outputting). If this option is not specified, 'yes' is assumed.

## System Compatibility-related Options
* -arcdelim (Archive delimiter)
    Specifies the archive delimiter (the character that separates the archive storage name and the in-archive storage name).
    Possible values are '>' (use '>') or '#' (use '#'). If this option is not specified, '>' is assumed.
    The archive delimiter was changed from the traditional '#' to '>' in Kirikiri 2 2.19 beta 14.
    Applications that operated in versions lower than 2.19 beta 14 may stop working due to this change, but they can be made to work by changing the delimiter to '#' with this option.
* -evalcontext (Behavior of postfix '!' operator)
    Specifies the behavior of the TJS2 postfix '!' operator.
    Possible values are 'this' (evaluate expression on this) or 'global' (evaluate expression on global). If this option is not specified, 'this' is assumed.
    The TJS2 postfix '!' operator used to execute expressions on the global context, but from 2.21 beta 9, it executes on the this context.
    Applications assuming versions lower than 2.21 beta 9 may not work unless this setting is set to "evaluate expression on global."
* -holdalpha (Default value of Layer.holdAlpha property)
    Specifies the default value of the Layer.holdAlpha property.
    Possible values are 'false' (false) or 'true' (true). If this option is not specified, 'false' is assumed.
    In Kirikiri 2 2.23 beta 4, the hda (whether to protect the alpha channel) option specified in various operation functions was removed, and the Layer.holdAlpha property was created instead. At this point, the default value of Layer.holdAlpha was true. If Layer.holdAlpha is true, it does not affect the behavior of past applications.
    In Kirikiri 2 2.23 beta 5, this default value became false. If you want to run an application assuming a version lower than Kirikiri 2 2.23 beta 5, it may not work correctly unless "true" is specified for this option.
* -unaryaster (Behavior of prefix '*' operator)
    Specifies the behavior of the TJS2 prefix '*' operator.
    Possible values are 'default' (behavior of 2.25 or later) or 'compat' (behavior before 2.25). If this option is not specified, 'default' is assumed.
    The TJS2 prefix '*' operator was an operator to retrieve the property object itself without going through the property handler, but from 2.25 beta 1, the operator with this function became the prefix '&', and the prefix '*' operator became an operator to operate the property handler of the property object. Applications assuming versions lower than 2.25 beta 1 may not work correctly unless this setting is set to "compatible with versions lower than 2.25."
* -wsvolfactor (DirectSound volume curve)
    Possible values are '3322' (behavior of 2.31 2011/6/14 or later) or '5000' (behavior before 2.31 2011/6/14). If this option is not specified, '3322' is assumed.
    The DirectSound volume curve became a more intuitive curve from 2.31 2011/6/14.
* __-readencoding (Script reading character code)__
    Possible values are 'Shift_JIS' (Kirikiri 2 compatible behavior) or 'UTF-8' (Kirikiri Z behavior). If this option is not specified, 'UTF-8' is assumed.
    The character code used to read scripts.
* __-ignoretouch (Disable touch events Ver 1.3.0 or later)__
    Possible values are 'false' (false) or 'true' (true). If this option is not specified, 'false' is assumed.
    If 'true' is specified, touch events are disabled and mouse events are generated instead.