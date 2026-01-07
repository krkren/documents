---
layout: default
title: Information for Light Users/Beginners
---

Please also refer to [KiriKiriZ Information](./index.html).

## Character Encoding Conversion
In KiriKiri2, the character encoding for text files (*.ks/*.tjs) was Shift-JIS, so they cannot be read as-is (KiriKiriZ uses UTF-8).
While it is possible to change settings to read Shift-JIS, it is better to convert them to UTF-8 if you are migrating to KiriKiriZ.
For conversion, [KanjiTranslator](http://www.kashim.com/kanjitranslator/) is convenient.
Launch it, set the destination encoding to UTF-8 (without BOM) and the line break code to CR-LF, drag and drop your text files (*.ks/*.tjs), and press the "Convert" button to convert them to UTF-8.
If you want the files to be usable in both KiriKiri2 and KiriKiriZ, you must use the UTF-16LE (with BOM) format.
The tool mentioned above cannot do this, so please look for other tools.

## Editor with Input Completion
For KiriKiri2, there were tools like KKDE and Kaguyahime Studio, but for KiriKiriZ, you can use [KKEFZ](https://github.com/mryp/kkefz).
For now, it is recommended to download the [v1.0.0 FD included version](https://github.com/mryp/kkefz/raw/master/_release/FlashDevelop-4.6.2_kkefz-1.0.0.zip).
If you are already using FlashDevelop, the plugin-only version should be fine.
Note that the version of KiriKiriZ bundled with KKEFZ seems to be old, so it is better to replace it with a newer one.

## Console
In KiriKiriZ, the console has been removed, and if launched from the command line, output is sent to the command line.
The command line refers to Command Prompt or PowerShell.
If you are not familiar with the command line, you can add a KiriKiri2-style console by installing [Krkr2Compat](https://github.com/krkrz/Krkr2Compat).
You can download it by selecting "Download ZIP" from the green "Clone or download" button on the right.
A [direct link is provided here](https://github.com/krkrz/Krkr2Compat/archive/master.zip) as well.
Please read [00README.txt](https://github.com/krkrz/Krkr2Compat/blob/master/00README.txt) for usage instructions.