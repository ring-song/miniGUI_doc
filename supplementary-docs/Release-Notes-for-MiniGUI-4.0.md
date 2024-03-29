# Release Notes for MiniGUI 4.0

Table of Contents

- [Version 4.0.0](#version-400)
  * [Overview](#overview)
  * [New APIs conforming Unicode 12.0](#new-apis-conforming-unicode-120)
  * [New APIs for complex or mixed scripts](#new-apis-for-complex-or-mixed-scripts)
  * [Enhanced logical font APIs](#enhanced-logical-font-apis)
  * [Support for Linux DRI/DRM](#support-for-linux-dri-drm)
  * [Slice allocator](#slice-allocator)
  * [Other Enhancements](#other-enhancements)
  * [Backward compatibility issues](#backward-compatibility-issues)
  * [Deprecated features](#deprecated-features)

## Version 4.0.0

MiniGUI 4.0.0 was released on July 15, 2019.

### Overview

In this version, we mainly enhanced and tuned the APIs related to text
rendering in order that MiniGUI can handle complex font systems
(scripts) like Arabic and Indic:

* MiniGUI now provides complete APIs for Unicode character processing.
  These APIs conform to Unicode 12.0, including Unicode
  Bidirectional Algorithm (UAX#9), Unicode Line Breaking Algorithm
  (UAX#14), Unicode Normalization Forms (UAX#15), Unicode Script Property
  (UAX#24), Unicode Text Segmentation (UAX#29), Unicode Vertical
  Text Layout (UAX#50), and so on.

* MiniGUI also provides new APIs to lay out, shape, and render glyphs
  from complex and mixed scripts, such as Arabic, Thai, and Indic.
  The new APIs also conform to the specifications of [CSS Text Module Level 3]
  and [CSS Writing Modes Level 3].

* We tuned and optimized MiniGUI's logical and device font interfaces to
  support the new features above.

* MiniGUI now has been enhanced to support input events which may be generated
  by input devices other than standard mouse (or single-touch panel) and
  keyboard, such as multi-touch panel (gesture), joystick, tablet tool,
  and switch. In MiniGUI 4.0, we introduced `MSG_EXIN_XXX` messages to support
  the input events from devices other than standard mouse and keyboard. We
  call these messages as 'extra input messages'.

Other important features of this version are the following new engines:

* The NEWGAL engine of `dri` supporting modern DRM-driven graphics cards. Using
  `dri` engine, a MiniGUI app can now use the hardware-accelerating graphics
  rendering for 2D/3D graphics.
* The IAL engine of `libinput` supporting all modern input devices including
  mouse, keyboard, joystick, switch, multi-touch panel, gesture, tablet tool,
  and table pad.
* The enhanced IAL engine of `random` generating extra input messages
  automatically for testing.

Lastly, we introduced a Slice Memory Allocator for fast concurrent memory
chunk allocation.

The following sections will describe these new features in detail.

### New APIs conforming Unicode 12.0

* New types:
    1. `Uchar32`: the Unicode code point value of a Unicode character.
    1. `Achar32`: the abstract character index value under a certain
        charset/encoding. With Unicode charset or encodings, the
        abstract character index value will be identical to the Unicode
        code point, i.e., Achar32 is equivalent to Uchar32 under this
        situation.
    1. `Glyph32`: the glyph index value in a device font. Note that
        a Glyph32 value is always bound to a specific logfont object.

* New functions to determine Unicode character properties:
    1. `UCharGetCategory` for getting the general category of
        a Unicode character.
    1. `UCharGetBreakType` for getting the breaking type of
        a Unicode character.
    1. `UStrGetBreaks` for getting the breaking types of
        a Unicode string.
    1. `UCharGetBidiType` for getting the bidi type of
        a Unicode character.
    1. `UStrGetBidiTypes` for getting the bidi types of a Unicode
        character string.
    1. `UCharGetBracketType` for getting the bracketed character of a
        Uchar32 character.
    1. `UStrGetBracketTypes` for getting the bracketed characters of a
        Uchar32 string.
    1. `UCharGetMirror` for getting the mirrored character of a Uchar32
        character.
    1. `UCharGetJoiningType` for getting the joining type of a Uchar32
        character.
    1. `UStrGetJoiningTypes` for getting the joining types of a Uchar32
        string.
    1. `UBidiGetParagraphDir` for getting the base paragraph direction
        of a single paragraph.
    1. `UBidiGetParagraphEmbeddingLevels` for getting the bidi embedding
        levels of a paragraph.
    1. `UBidiReorderLine`, `UBidiShapeMirroring`, `UBidiJoinArabic`,
        ` UBidiShapeArabic`, and `UBidiShape` for doing bidi-aware
        mirroring, joining, and shaping.
    1. `UCharGetScriptType` for getting the script type of a Uchar32
        character.

MiniGUI also provides some utilities/helpers for Unicode character
conversion, for example, from lower case to upper case, single width to
full width. Please see MiniGUI API reference documents for detailed
description.

### New APIs for complex or mixed scripts

To lay out, shape, and render a text in mixed scripts, you should call
`GetUCharsUntilParagraphBoundary` function first to convert
a multi-byte string to a Unicode string under the specified white space
rule, breaking rule, and transformation rule. For example, converting a
general C string in UTF-8 or GB18030 to a Uchar32 string by calling this
function. You can call `CreateLogFontForMChar2UChar` function to create
a dummy logfont object for this purpose in order to expense a minimal memory.

If the text is in simple scripts, like Latin or Chinese, you can call
`GetGlyphsExtentFromUChars` function to lay out the paragraph. This function
returns a glyph string which can fit in a line with the specified
maximal extent and rendering flags. After this, you call
`DrawGlyphStringEx` function to draw the glyph string to the
specific position of a DC.

If the text is in complex and/or mixed scripts, like Arabic, Thai,
and Indic, you should create a TEXTRUNS object first by calling
`CreateTextRuns` function, then initialize the shaping engine for
laying out the text.

MiniGUI provides two types of shaping engine. One is the basic
shaping engine. The corresponding function is `InitBasicShapingEngine`.
The other is called complex shaping engine, which is based on HarfBuzz.
The corresponding function is `InitComplexShapingEngine`. The latter
one can give you a better shaping result.

After this, you should call `CreateLayout` to create a layout object
for laying out the text, then call `LayoutNextLine` to lay out the lines
one by one.

You can render the laid out lines by calling `DrawLayoutLine` function.

Finally, you call `DestroyLayout` and `DestroyTextRuns` to destroy
the layout object and text runs object.

Before rendering the glyphs laid out, you can also call `GetLayoutLineRect`
to get the line rectangle, or call `CalcLayoutBoundingRect` to get
the bounding rectangle of one paragraph.

These new APIs provide a very flexible implementation for your apps
to process the complex scripts. The implementation was derived from
LGPL'd Pango, but we optimized and simplified the original implementation
in the following respects:

* Split the layout process into two stages. We get the text runs
  (Pango items) in the first stage, and the text runs will keep as
  constants for subsequent different layouts. In the second stage,
  we create a layout object for a set of specific layout parameters,
  and generates the lines one by one for the caller. This is useful
  for an app like browser, it can reuse the text runs if the output
  width or height have changed, and no need to re-generate the text
  runs because of the output rectangle size change.

* Use MiniGUI's fontname for the font attributes of text, and leave
  the font selection and the glyph generating to MiniGUI's logfont
  module. In this way, we simplify the layout process greatly.

* Always use Uchar32 string for the whole layout process. So the
  code and the structures are clearer than original implementation.

* Provide two shaping engines for rendering the text. One is a
  basic shaping engine and other is the complex shaping engine based
  on HarfBuzz. The former can be used for some simple applications.

### Enhanced logical font APIs

The styles of LOGFONT have changed.

* Add new rendering style:
  1. `FS_RENDER_ANY`: Not specified.
  1. `FS_RENDER_MONO`:
  1. `FS_RENDER_GREY`:
  1. `FS_RENDER_SUBPIXEL`:
* Some old styles are deprecated:
  1. `FS_WEIGHT_BOOK`; use `FS_RENDER_GREY` instead.
  1. `FS_WEIGHT_SUBPIXEL`; use `FS_RENDER_SUBPIXEL` instead.
* Introduced or redefined the weight styles to follow OpenType specification:
  1. `FONT_WEIGHT_ANY`: Not specified.
  1. `FONT_WEIGHT_THIN`: Thin.
  1. `FONT_WEIGHT_EXTRA_LIGHT`: Extra light (Ultra Light).
  1. `FONT_WEIGHT_LIGHT`: Light.
  1. `FONT_WEIGHT_REGULAR`: Regular (Normal).
  1. `FONT_WEIGHT_MEDIUM`: Medium.
  1. `FONT_WEIGHT_DEMIBOLD`: Demi Bold (Semi Bold)
  1. `FONT_WEIGHT_BOLD`: Bold.
  1. `FONT_WEIGHT_EXTRA_BOLD`: Extra Bold (Ultra Bold).
  1. `FONT_WEIGHT_BLACK`: Black (Heavy).
* Introduce the new decoration styles and replace
`FONT_UNDERLINE_LINE` and `FONT_STRUCKOUT_LINE` with them:
  1. `FONT_DECORATE_ANY`: Not specified.
  1. `FONT_DECORATE_NONE`: None.
  1. `FONT_DECORATE_UNDERLINE`: glyphs are underscored.
  1. `FONT_DECORATE_STRUCKOUT`: glyphs are overstruck.
  1. `FONT_DECORATE_US`: Both `FONT_DECORATE_UNDERLINE`
and `FONT_DECORATE_STRUCKOUT`.
  1. `FONT_DECORATE_OUTLINE`: Outline (hollow) glyphs.
  1. `FONT_DECORATE_REVERSE`: Reserved for future use. Glyphs'
foreground and background are reversed.
* The following styles are deprecated:
  1. `FONT_OTHER_LCDPORTRAIT`
  1. `FONT_OTHER_LCDPORTRAITKERN`

For a new app, you should use the new function `CreateLogFontEx` to
create a LOGFONT, and specify the weight and rendering method of the glyph.

For backward compatibility, you can still use `CreateLogFont` to create a new
LOGFONT. However, `FS_WEIGHT_BOOK` will be treated with `FS_WEIGHT_REGULAR` and
`FS_RENDER_GREY`, while `FS_WEIGHT_SUBPIXEL` will be treated with 
`FS_WEIGHT_REGULAR` and `FS_RENDER_SUBPIXEL`.

You can still use `CreateLogFontByName` to create a new LOGFONT.
But the style string in the font name has changed from

    <weight><slant><flipping><other><underline><struckout>

to

    <weight><slant><flipping><other><decoration><rendering>

Note that `<underline>` and `<struckout>` are merged into `<decoration>`
in order to keep the style string 6-character long.

Consequently, if you want to use the rendering method SUPIXEL for a TTF font,
please define the logical font name in the following way:

    ttf-Courier-rrncns-*-16-UTF-8

Moreover, the family name of a DEVFONT supports aliases since 4.0.0:

    <fonttype>-<family[,aliase]*>-<styles>-<width>-<height>-<charset[,charset]*>

for example:

    ttf-Arial,Sans Serif-rrncnn-8-16-ISO8859-1,UTF-8
    ttf-courier,monospace,serif-rrncnn-8-16-ISO8859-1,UTF-8

Note that the length of one DEVFONT name can not exceed 255 bytes.

Since version 4.0.0, you can specify up to 7 family names for a logfont name,
such as:

    ttf-Courier,宋体,Naskh,SansSerif-rrncns-U-16-UTF-8

In this way, you can specify a logfont to use multiple devfonts
to render a complex text. This is useful when different glyphs are
contained in different font files. It is well known that, a font is
often designed for a particular language/script or a few similar
languages/scripts.

Since 4.0.0, the previous width field of a logfont name is used for
glyph orientation:

- 'U': Glyphs stand upright (default).
- 'S': Glyphs are rotated 90 degrees clockwise (sideways).
- 'D': Glyphs are upside-down.
- 'L': Glyphs are rotated 90 degrees counter-clockwise (sideways left).

### Support for Linux DRI/DRM

In order to support modern graphics card or GPU, we introduced a
new NEWGAL engine `dri`. Developers can use this engine to
run MiniGUI apps on a Linux box on which the DRI
(Direct Rendering Infrastructure) is enabled.

The `dri` engine uses `libdrm` developed by Free Desktop project:

https://dri.freedesktop.org/wiki/

Libdrm is a user-space library that implements the Direct Rendering Manager.
MiniGUI mainly uses this library to support dumb frame buffer
(no hardware acceleration). However, you can write a driver for your
graphics card or GPU to implement the hardware accelerating features.

To avoid modifying the MiniGUI source code when supporting a new GPU,
the `dri` engine has adopted a scalable design:

* You can directly use the `dri` engine to run MiniGUI on a GPU
which supports dumb frame buffer.
* If you want to take advantage of hardware acceleration of
your GPU, you can write some code for your GPU as a sub driver
of `dri` engine outside MiniGUI.

Under this situation, you need to configure MiniGUI with the following
option:

    --with-targetname=external

and implement the sub driver in your MiniGUI apps.

The header file `<minigui/exstubs.h>` defines the operations (a set of
callback functions) you need to implement for your GPU externally.

As an example, we implement the sub driver for `i915` graphics card
in `mg-tests/dri-engine/`. Please refer to `mg-tests` repository.

To exploit the GPU's accelerated rendering capabilities, a MiniGUI app
can use `cairo` and/or `OpenGL ES` to assist in rendering 2D/3D graphics
when using the `dri` engine. We will provide some samples in `mg-tests`
or `mg-samples` for this purpose.

Note that for `dri` engine, we introduced a new section in MiniGUI runtime
configuration:

```
[dri]
defaultmode=1024x768-32bpp
pixelformat=XR24
device=/dev/dri/card0
dpi=96
```

You can use the key `dri.device` to specify your DRI device.

You can use the key `dri.pixelformat` to specify the DRM pixel format for the
screen. We use DRM fourcc code to define the pixel format of the screen
surface. For more information, please see `<drm/drm_fourcc.h>` header file.
Note that only 8/16/24/32 bpp RGB formats are supported. For example, `XR24`
means `X8R8G8B8` pixel format.

Currently, the `dri` NEWGAL engine does not provide support for
MiniGUI-Processes run-time mode. We will enhance this in the subsequent
version of MiniGUI.

Also note that when you use the hardware accelerated sub driver, MiniGUI app
may need the root privilege to call `drmSetMaster` to set the video mode.
However, under MiniGUI-Processes run-time mode, only the server (`mginit`) will
need this privilege when you use the future `dri` engine.

### Extra input messages

In MiniGUI 4.0.0, we introduce the extra input messages to support modern
input devices including multiple touch panel, gesture, tablet tool, and
table pad.

The extra input messages have the prefix `MSG_EXIN_`. If a MiniGUI app
needs to handle these extra input events such as gestures, you need
to handle the `MSG_EXIN_XXX` messages in the app. For examples, please
refer to `mg-tests/extra-input/`.

Currently, there are two built-in IAL engines which can generate
extra input messages:

* The IAL engine of `libinput` to support all modern input devices on a
Linux system. This engine runs on `libinput` introduced by Free Desktop project.

* The enhanced IAL engine of `random` to generate extra input messages
  automatically for testing.

You can also write your own IAL engines to generate the extra messages.
Please see the implementation of `libinput` and `random` engines for
details.

For `libinput` engine, we introduced a new section in MiniGUI runtime
configuration:

```
[libinput]
seat=seat0
```

The key `libinput.seat` specifies the seat identifier, the default
is `seat0`.

For `random` engine, we introduced a new section in MiniGUI runtime
configuration:

```
[random]
logfile=events.out
eventtypes=mouse-keyboard-button-gesture-stouch
minkeycode=1
maxkeycode=128
minbtncode=0x100
maxbtncode=0x1ff
```

The MiniGUI runtime configuration key `random.logfile` specifies
the log file which will store the input events generated by this engine.
If MiniGUI failed to open the log file, the log feature will be disabled.

The MiniGUI runtime configuration key `random.eventtypes` specifies
the input event types which are generated by this IAL engine,
in the following pattern:

    <event-type>[-<event-type>]*

The `<event-type>` can be one of the following values:

 - `mouse`: mouse.
 - `keyboard`: keyboard.
 - `button`: buttons.
 - `single_touch`: touch pen or single touch panel.
 - `multi_touch`: multiple touch panel.
 - `gesture`: gesture.
 - `tablet_tool`: tablet tool.
 - `tablet_pad`: tablet pad.
 - `switch`: switch.

The MiniGUI ETC key `random.minkeycode` specifies the minimal key code
which can be generated by the engine if `keyboard` is included.

The MiniGUI ETC key `random.maxkeycode` specifies the maximal key code
which can be generated by the engine if `keyboard` is included.

The MiniGUI ETC key `random.minbtncode` specifies the minimal button code
which can be generated by the engine if `button` is included.

The MiniGUI ETC key `random.maxbtncode` specifies the maximal key code
which can be generated by the engine if `button` is included.

For invalid `random.eventtyps`, the engine uses `mouse` as default.

For invalid `random.minkeycode`, and/or `random.maxkeycode` key values,
the engine uses `SCANCODE_ESCAPE`, and `SCANCODE_MICMUTE` respectively.

For invalid `random.minbtncode`, and/or `random.maxbtncode` key values, use
`0x100` (`BTN_MISC` defined by Linux kernel), and `0x2ff` (`KEY_MAX` defined by
Linux kernel) respectively.

This engine maintains a state machine for each input event type, and
generates a reasonable event sequence for each type. If and only if
an event sequence is finished or cancelled, the engine switches to another
event type randomly.

Note that currently, the following event types (in `random` engine)
are not implemented:

 - `multi_touch`
 - `tablet_tool`
 - `tablet_pad`
 - `switch`

### Slice allocator

MiniGUI now provides an efficient way to allocate groups of equal-sized
chunks of memory.

Memory slices provide a space-efficient and multi-processing scalable
way to allocate equal-sized pieces of memory, just like the
MiniGUI's block data heap. Relative to the standard malloc function
and block data heap, this allocator can avoid excessive memory-waste,
scalability and performance problems.

The following APIs are introduced:

* `mg_slice_alloc` to allocate a given size trunk of memory.
* `mg_slice_free` to free a given size trunk of memory.
* `mg_slice_new` to allocate a memory for a given structure.
* `mg_slice_delete` to free a memory for a given structure.

We use the slice allocator when laying out the text in complex scripts.

Note that this implementation is derived from LGPL'd glib.

Also note that when you use valgrind with a MiniGUI app, you should export
the following environment variable to tell the allocator always uses
`malloc`, or there will be a conflict with valgrind:

    $ export MG_SLICE="always-malloc"

### Other Enhancements

* A new BITMAP type `BMP_TYPE_REPLACEKEY` was added. When `bmType` of
  a BITMAP object has this bit set, any pixel which is equal to
  `bmColorKey` will be replaced by `bmColorRep`.

* The timer implementation has been tuned for MiniGUI-Standalone and
  MiniGUI-Threads runtime modes. Now we can get a more accurate `MSG_TIMER`
  message and the return value of function `GetTickCount`.

* More key scancodes are defined for modern keyboard, and the `NR_KEYS`
  is re-defined to be 250.

* Support for libPNG 1.6.x.

* Support for Ubuntu 18.04 LTS.

* Support for GCC 7.

### Backward compatibility issues

In MiniGUI 4.0.0, we changed some unreasonable APIs which were introduced
in early versions. There are also other changes breaking the backward
compatibility. This section gives you a summary about these changes.

* Rename `UChar32` to `Uchar32` and `UChar16` to `Uchar16` in order to
  avoid the conflict with typedef of UChar32 in the system header
  `<unicode/umachine.h>`.
* Rename `mg_FT_LcdFilter` to `FT2LCDFilter` in order to follow MiniGUI
  naming rules.
* Redefine `Uchar32` and `Glyph32` as `Uint32` instead of `int`.

In early versions, we did not significantly distinguish between
characters and glyphs. This will lead to some confusion. Therefore,
we introduce a new type called `Achar32`, which is the character's
index value in a certain charset/encoding. While the type `Glyph32`
is the index value of a glyph in a font.

In order to reflect the correct character and glyph concepts,
the following functions are removed:

  * GetGlyphType, use GetACharType instead.
  * GetGlyphShape, use GetShapedAChar instead.
  * GetGlyphBIDIType, use GetACharBidiType

The names of the following functions are changed:

  * BIDIGetTextLogicalGlyphs -> BIDIGetTextLogicalAChars
  * BIDIGetTextVisualGlyphs -> BIDIGetTextVisualAChars
  * BIDILogAChars2VisGlyphs -> BIDILogAChars2VisAChars
  * BIDILogAChars2VisGlyphsEx -> BIDILogAChars2VisACharsEx

The following functions are deprecated, you should use
the new Unicode version instead:

  * BIDIGetTextLogicalAChars
  * BIDIGetTextVisualAChars
  * BIDILogAChars2VisAChars
  * BIDILogAChars2VisACharsEx
  * BIDIGetTextRangesLog2Vis
  * BIDIGetLogicalEmbedLevelsEx

The following function is deprecated:

  * GetGlyphBitmap, use GetGlyphInfo instead.

The fields `height` and `descent` have been removed from GLYPHINFO structure.
You should get the font metrics information by calling `GetFontMetrics` function
if you want to get the height and descent data of one font.

The basic glyph type and break type have been removed from GLYPHINFO
structure. You should use `GetACharType` instead.

### Deprecated features

Support for FreeType1 was removed.

You should always use FreeType2 to support vector fonts, such as TrueType
fonts (TTF), TrueType collections (TTC), OpenType fonts (OTF, both TrueType
and CFF variants), OpenType collections (OTC), and Type 1 fonts (PFA and PFB).

[CSS Text Module Level 3]: https://www.w3.org/TR/css-text-3/
[CSS Writing Modes Level 3]: https://www.w3.org/TR/css-writing-modes-3/
