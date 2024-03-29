# Button and Derived Control Classes

- [Brief Introduction of Button Series Control](#brief-introduction-of-button-series-control)
- [Button Class Renderer](#button-class-renderer)
- [`mButton`](#mbutton)
   + [Style of `mButton`](#style-of-mbutton)
   + [Property of `mButton`](#property-of-mbutton)
   + [Event of `mButton`](#event-of-mbutton)
   + [Renderer of `mButton`](#renderer-of-mbutton)
   + [`mButton` Skin Renderer](#mbutton-skin-renderer)
   + [Programming Example of `mButton`](#programming-example-of-mbutton)
- [`mCheckButton`](#mcheckbutton)
   + [Property of `mCheckButton`](#property-of-mcheckbutton)
   + [Event of `mCheckButton`](#event-of-mcheckbutton)
   + [Renderers of `mCheckButton`](#renderers-of-mcheckbutton)
   + [Programming Example of `mCheckbutton`](#programming-example-of-mcheckbutton)
- [`mRadioButton`](#mradiobutton)
   + [Property of `mRadiobutton`](#property-of-mradiobutton)
   + [Event of `mRadiobutton`](#event-of-mradiobutton)
   + [Renderers of `mRadiobutton`](#renderers-of-mradiobutton)
   + [Programming Example of `mRadiobutton`](#programming-example-of-mradiobutton)
- [`mMenuButton`](#mmenubutton)
   + [Property of `mMenuButton`](#property-of-mmenubutton)
   + [Event of `mMenuButton`](#event-of-mmenubutton)
   + [Programming Example of `mMenuButton`](#programming-example-of-mmenubutton)
- [`mColorButton`](#mcolorbutton)
   + [Property of `mColorButton`](#property-of-mcolorbutton)
   + [Event of `mColorButton`](#event-of-mcolorbutton)
   + [Programming Example of `mColorButton`](#programming-example-of-mcolorbutton)
- [Appendix:A](#appendixa)
   + [`GrandientMode`](#grandientmode)
   + [`ButtonState`](#buttonstate)
   + [`ButtonCheckState`](#buttoncheckstate)


## Brief Introduction of Button Series Control

Button is an essential control of human computer interaction in the user
interface, and all the human computer interaction interfaces include button. In
the design of mGNCS, button use conditions under different use environments are
fully considered, and different properties and interfaces etc. are provided for
the convenience of user componentization development. Buttons in mGNCS
reconstruct built-in button control in MiniGUI 3.0 and conduct big adjustment.
Two button types are added, greatly strengthening button control. Buttons in
`MiniGNCS` include five types, common button, check box, single selection
button, menu button and color selection button. Users can select or switch
status of buttons through keyboard or mouse. Input of users will make buttons
generate notification message, and applications can send messages to the
buttons to change the status of the buttons. Each button type corresponds to
one class, which contains style, property, event, method and renderer.
Functions of the controls provided in mGNCS are usually stronger than the
functions of the controls used on `PC`, providing more configurable
information, and renderer editing is added.

In this document, button class and the properties in it will be introduced in
detail, making it convenient for the users to further understand the details of
buttons, thus buttons can be used more flexibly.

Class hierarchical relation related to buttons:
- [`mWidget`](MiniGUIProgGuidePart2Chapter04.md#mwidget)
   - [`mButton`](MiniGUIProgGuidePart2Chapter06.md#mbutton)
      - [`mCheckButton`](MiniGUIProgGuidePart2Chapter06.md#mcheckbutton)
      - [`mRadioButton`](MiniGUIProgGuidePart2Chapter06.md#mradiobutton)
      - [`mMenuButton`](MiniGUIProgGuidePart2Chapter06.md#mmenubutton) (mPopMenuMgr is used)
      - [`mColorButton`](MiniGUIProgGuidePart2Chapter06.md#mcolorbutton)

Control creating methods:
- Automatic creation: through interface designer in miniStudio, corresponding
button control is dragged. miniStudio will automatically create control and
provide visual control configuration, and at the same time, creation codes are
generated automatically.
- Manual generation: according to mGNCS control creation process, through
programming, corresponding control window class ID is imported and control is
generated. Manual programming sets control property and event handling.


## Button Class Renderer

mGNCS provides four types of renderers and defines renderer property setting
set. The four types of renderers correspond to four kinds of renderer property
setting set. For button class control, the four kinds of renderers defines
which renderer property can be set in the button control and which effect can
it be set to. mGNCS provides default renderer property. Of course, users can
change these properties, thus different effects can be set. That is to say,
users can change different renderer properties and create numerous renderer
examples based on the four types of renderers.

## `mButton`

- *Control window class*: `NCSCTRL_BUTTON`
- *Control English name*: Push Button
- *Brief introduction*: It is button of press down mode. On the button, the
simple ones can be represented by literals and can be marked by images, and
they can be displayed by mixture of images and literals.

![alt](figures/push_button.png)


To understand programming of buttons, firstly, it is necessary to understand
the status of buttons. In mGNCS, the status of buttons is divided into four
kinds, refer to appendix definition and note: `ButtonState`
[ButtonState](MiniGUIProgGuidePart2Chapter06.md#buttonstate)

### Style of `mButton`

Style of buttons includes two categories:
- Firstly, it is defining button `CHECK` status, which is press down status,
and setting is carried out through Checkable/Autocheck/ThreeDCheck in the
property;
- Secondly, it is setting button panel as displaying text or image or
coexistence if image and text, which is done mainly through options in Label
Type in property. Text by default, while layout of images and texts is set
through other properties.

`mButton` is the foundation class of all buttons, which implements `PushButton`
by default. Buttons of all other types are derived from it.

It is inherited from the style of [mWidget](MiniGUIProgGuidePart2Chapter04.md).

| *Style ID* | *miniStudio property name* | *Comments* |
|------------|----------------------------|------------|
| `NCSS_BUTTON_CHECKABLE` | Checkable | Set if the button can carry out `CHECKED` status conversion; if `FALSE`, `CHECK` status conversion is not carried out, and autocheck and `ThreeDCheck` are invalid |
| `NCSS_BUTTON_AUTOCHECK` | Autocheck | When `CHECKED` status conversion can be carried out, set if clicking can be carried out to automatically switch `CHECK` status |
| `NCSS_BUTTON_3DCHECK` | `ThreeDCheck` | Set button `CHECK` status as three status (unchecked-halfchecked-checked) or two status (unchecked-checked) switch |
| `NCSS_BUTTON_IMAGE` | `LabelType->Image`| Image button style, the button is label literal mode in `LabelType` by default |
| `NCSS_BUTTON_IMAGELABEL` | `LabelType->ImageLabel` | Button of horizontally arranged image text |
| `NCSS_BUTTON_VERTIMAGELABEL` | `LabelType->VertImageLabel` | Button of vertically arranged image text |

### Property of `mButton`

It is inherited from the property of [mWidget](MiniGUIProgGuidePart2Chapter04.md).

| *Property ID name* | *miniStudio property name* | *Type* | *Permission* | *Comments* |
|--------------------|----------------------------|--------|--------------|------------|
| `NCSP_BUTTON_ALIGN` | Align | [Alignment enumeration value](MStudioMGNCSV1dot0PGENAppC#AlignValues) | `RW` | Content on the button (image or literal) is aligned with the button according to the set mode in horizontal direction |
| `NCSP_BUTTON_VALIGN` | `VAlign` | [Alignment enumeration value](MStudioMGNCSV1dot0PGENAppC#AlignValues) | `RW` | Content on the button (image or literal) is aligned with the button according to the set mode in vertical direction, and the alignment value is top – 0, bottom – 1, center – 2 |
| `NCSP_BUTTON_WORDWRAP` | Wordwrap | int | `RW` | Automatic wrap property of the literals on the button, when the single line length of the literal content is bigger than the control width, automatic wrap can be selected. `TRUE`: automatic wrap `FALSE`: single line, and the exceeding part is not displayed |
| `NCSP_BUTTON_IMAGE` | Image | `PBITMAP` | `RW` | Image, corresponding to button property pbmp, which is image pointer, it must be valid when `NCSS_BUTTON_IMAGE` or `NCSS_BUTTON_IMAGELABEL` or `NCSS_BUTTON_VERTIMAGELABEL` is set |
| `NCSP_BUTTON_CHECKSTATE`| None | [mButtonCheckState](MiniGUIProgGuidePart2Chapter06.md#buttoncheckstate)| `RW` | Set or get check status, valid when `NCSS_BUTTON_CHECKABLE` is set, `NCS_BUTTON_HALFCHECKED` is regarded as valid value when `NCSS_BUTTON_3DCHECK` is set |
| `NCSP_BUTTON_IMAGE_SIZE_PERCENT`| `ImageSizePercent` | int | `RW` | Under the style of coexistence of image and text, set proportion of image on the control (take value 15~85, representing 15% to 85%), and the remaining is occupied by the text |
| `NCSP_BUTTON_GROUPID`| `GroupID` | int | `RW` | When there is `NCSS_BUTTON_CHECKABLE` style, ID of a `mButtonGroup` can be set to the window, it will automatically get a corresponding `mButtonGroup` object and set |
| `NCSP_BUTTON_GROUP`| None | `mButtonGroup*` |RW| When there is `NCSS_BUTTON_CHECKABLE` style, set a `mButtonGroup` object pointer, and all the buttons adding to the group has the automatic switching ability of Radio style |

### Event of `mButton`

It is inherited from the event of [mWidget](MiniGUIProgGuidePart2Chapter04.md).

| *Event notification code* | *Explanation* | *Parameter* |
|---------------------------|---------------|-------------|
| `NCSN_BUTTON_PUSHED` | Key pressed down | |
| `NCSN_BUTTON_STATE_CHANGED` | After status changes | New status |

Note, the control inherits `NCSN_WIDGET_CLICKED` event of the parent class.

### Renderer of `mButton`

*For the usage of renderer, see [look and feel renderer](MiniGUIProgGuidePart2Chapter03.md)*.

#### `mButton` Classic renderer

The basic style under classic renderer is as shown in the figure below:

Drawing of `mButton` Classic renderer is as below: for the drawing of non
client area, please refer to the renderer of
[mWidget](MiniGUIProgGuidePart2Chapter04.md#mwidget).

| *Property ID* | *Meaning* | *miniStudio property name* | *Value type* | *Schematic diagram of the valid region* | *Value range* |
|---------------|-----------|----------------------------|--------------|-----------------------------------------|---------------|
| `NCS_FGC_3DBODY` | Text foreground color | `FgColor3DBody` | `DWORD` (`ARGB`) |<img src="figures/push-button-classic-fg3dcolor.png" alt="push-button-classic-fg3dcolor.png"/>| |
| `NCS_BGC_3DBODY` | Background color | `BgColor3DBody` | `DWORD` (`ARGB`) | <img src="figures/push-button-classic-bg3dcolor.png" alt="push-button-classic-bg3dcolor.png"/>| |
| `NCS_FGC_DISABLED_ITEM` | Text foreground color when the window is invalid | `TextDisableColor` | `DWORD` (`ARGB`) |<img src=figures/push-button-classic-fgdisable.png" alt="push-button-classic-fgdisable.png"/>| |
| `NCS_BGC_DISABLED_ITEM` | background color when the window is invalid | `BgColorDisable` | `DWORD` (`ARGB`) |<img src=figures/push-button-classic-bgdisable.png" alt="push-button-classic-bgdisable.png"/>| |


- High light effect: through enhancing the brightness of the background color
(`NCS_BGC_3DBODY`), as the high light color
- check effect: implement through drawing invaginated border
- Three status Check effect: implement half selection through drawing high
light color in the invaginated border

Schematic diagram:<br/>

![alt](figures/push-button-classic-hilight-check-halfchk.png)


### `mButton` Skin Renderer

Refer to [Appendix B: Specification for the Image Resources Used by Skin Renderer](MiniGUIProgGuideAppendixB.md#mbutton).

#### `mButton` Fashion Renderer

For the drawing of non client area, please refer to the drawing of Fashion
renderer of [mWidget](MiniGUIProgGuidePart2Chapter04.md#mwidget).

| *Property ID* | *Meaning* | *miniStudio property name* | *Value type* | *Schematic diagram of the valid region* | *Value range* |
|---------------|-----------|----------------------------|--------------|-----------------------------------------|---------------|
| `NCS_FGC_3DBODY` | Text foreground color | `FgColor3DBody` | `DWORD` (`ARGB`) | Same as Classic renderer | |
| `NCS_FGC_DISABLED_ITEM` | Text foreground color when the window is invalid| `TextDisableColor` | `DWORD` (`ARGB`) | Same as Classic renderer | |
| `NCS_BGC_3DBODY` | Background color | `BgColor3DBody` | `DWORD` (`ARGB`) |<img src="figures/push-button-fashion-bgcolor.png" alt="push-button-fashion-bgcolor.png"/> | |
| `NCS_BGC_DISABLED_ITEM` | Text background color when the window is invalid | `BgColorDisable` | `DWORD` (`ARGB`) |<img src="figures/push-button-fashion-bgdisalbe.png" alt="push-button-fashion-bgdisalbe.png"/>| |
| `NCS_MODE_BGC` | Gradual change fill mode | `GradientMode` | int |<img src="figures/push-button-fashion-gradientmode.png" alt="push-button-fashion-gradientmode.png" />| [GradientMode](MiniGUIProgGuidePart2Chapter06.md#grandientmode) |
| `NCS_METRICS_3DBODY_ROUNDX` | Window rectangle round corner X radius | `RoundX` | int | | 0 to 1/2 of the window width |
| `NCS_METRICS_3DBODY_ROUNDY` | Window rectangle round corner Y radius | `RoundY` | int| | 0 to 1/2 of the window height |

- Gradual change color: the gradual change color gives different brightness
factor from the ground color (`NCS_BGC_3DBODY` or `NCS_BGC_DISABLED_ITEM`), and
calculates the two objective colors. Gradually change from the center (ground
color) to the two sides or top and bottom (highlighted color achieved through
calculation)
- High light color: after the ground color is highlighted, the gradual change
color calculates to obtain on the foundation of this
- Representing method of check status: after the ground color undergoes
darkening processing, calculate the gradual change color
- Representing method of three status check: besides the method is the same as
above in check status, in half selection status, use the window ground color
(`NCS_BGC_WINDOW`) as the whole ground color to draw

Schematic diagram: <br />

![alt](figures/push-button-fashion-hilight-check-halfchk.png)

#### `mButton` Flat Renderer

For the drawing of non client area, please refer to the drawing of Flat
renderer of [mWidget](MiniGUIProgGuidePart2Chapter04.md#mwidget).

| *Property ID* | *Meaning* | *miniStudio property name* | *Value type* | *Schematic diagram of the valid region* | *Value range* |
|---------------|-----------|----------------------------|--------------|-----------------------------------------|---------------|
| `NCS_FGC_3DBODY` | Text foreground color | `FgColor3DBody` | `DWORD` (`ARGB`) | Same as Classic renderer | |
| `NCS_FGC_DISABLED_ITEM` | Text foreground color when the window is invalid | `TextDisableColor` | `DWORD` (`ARGB`) | Same as Classic renderer | |
| `NCS_BGC_3DBODY` | background color | `BgColor3DBody` | `DWORD` (`ARGB`) | Same as Classic renderer |
| `NCS_METRICS_3DBODY_ROUNDX` | Window rectangle round corner X radius | `RoundX` | int | | 0 to 1/2 of the window width |
| `NCS_METRICS_3DBODY_ROUNDY` | Window rectangle round corner Y radius | `RoundY` | int| | 0 to 1/2 of the window height |

- High light: draw small prompting symbols to implement around the control
- check status: draw lines at right-bottom edition, and compose inverted “L”
realization
- check representing method of three status: in half selection status, draw
lines at left-top, forming fell flat “L”

Schematic diagram: <br />

![alt](figures/push-button-flat-hilight-check-halfchk.png)



### Programming Example of `mButton`

- Operation screen shot: <br />

![alt](figures/push_button.png)

- This program uses Fashion renderer to create buttons of four different
styles, which are common button, image button, Autocheck style button and three
status button.
- Complete example code of common button:

__List 1__ button.c

```cpp
/*
** button.c: Sample program for mGNCS Programming Guide
**      The first mGNCS application.
**
** Copyright (C) 2009 ~ 2019 FMSoft Technologies.
*/

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// START_OF_INCS
#include <minigui/common.h>
#include <minigui/minigui.h>
#include <minigui/gdi.h>
#include <minigui/window.h>

#include <mgncs/mgncs.h>
// END_OF_INCS

#define ID_BTN  101
#define ID_BTN1 102
#define ID_BTN5 106
#define ID_BTN6 107
#define ID_BTN7 108
#define ID_BTN8 109

// START_OF_HANDLERS
static BITMAP bmp;
static BOOL mymain_onCreate(mWidget* self, DWORD add_data)
{
    //set image
//START_SET_IMAGE
    if(LoadBitmapFromFile(HDC_SCREEN, &bmp, "icon_button.png")!=0)
    {
        printf("cannot load image file \"icon_button.png\"\n");
    }

    mButton *mb1 = (mButton*)ncsGetChildObj(self->hwnd, ID_BTN1);

    if(mb1)
        _c(mb1)->setProperty(mb1, NCSP_BUTTON_IMAGE, (DWORD)&bmp);
//END_STE_IMAGE
    return TRUE;
}

static void mymain_onClose(mWidget* self, int message)
{
    DestroyMainWindow(self->hwnd);
    PostQuitMessage(0);
}

static NCS_EVENT_HANDLER mymain_handlers[] = {
    {MSG_CREATE, mymain_onCreate},
    {MSG_CLOSE, mymain_onClose},
    {0, NULL}
};
// END_OF_HANDLERS

// START_OF_RDRINFO
NCS_RDR_ELEMENT btn_rdr_elements[] =
{
    { NCS_MODE_USEFLAT, 1},
    { -1, 0 }
};
static NCS_RDR_INFO btn_rdr_info[] =
{
    {"fashion","fashion", btn_rdr_elements}
};
// END_OF_RDRINFO

// START_OF_TEMPLATE
static NCS_WND_TEMPLATE _ctrl_templ[] = {
//START_DCL_DEF_PUSHBUTTON
    {
        NCSCTRL_BUTTON,
        ID_BTN,
        30, 30, 80, 25,
        WS_BORDER | WS_VISIBLE,
        WS_EX_NONE,
        "button",
        NULL,         //props,
        btn_rdr_info, //rdr_info
        NULL,         //handlers,
        NULL,         //controls
        0,
        0             //add data
    },
//END_DCL_DEF_PUSHBUTTON
//START_DCL_IMAGEBUTTON
    {
        NCSCTRL_BUTTON,
        ID_BTN1,
        150, 30, 80, 25,
        WS_VISIBLE | NCSS_BUTTON_IMAGE,
        WS_EX_NONE,
        "Image",
        NULL,         //props,
        btn_rdr_info, //rdr_info
        NULL,         //handlers,
        NULL,         //controls
        0,
        0             //add data
    },
//END_DCL_IMAGEBUTTON
//START_DCL_AUTOCHECKBTN
    {
        NCSCTRL_BUTTON,
        ID_BTN1,
        30, 80, 80, 25,
        WS_VISIBLE  | NCSS_BUTTON_AUTOCHECK | NCSS_BUTTON_CHECKABLE,
        WS_EX_NONE,
        "Auto button",
        NULL,         //props,
        btn_rdr_info, //rdr_info
        NULL,         //handlers,
        NULL,         //controls
        0,
        0             //add data
    },
//END_DCL_AUTOCHECKBTN
//START_DCL_3DAUTOCHECKBTN
    {
        NCSCTRL_BUTTON,
        ID_BTN1,
        150, 80, 80, 25,
        WS_VISIBLE | NCSS_BUTTON_AUTOCHECK | NCSS_BUTTON_CHECKABLE \
            | NCSS_BUTTON_3DCHECK,
        WS_EX_NONE,
        "3D button",
        NULL,         //props,
        btn_rdr_info, //rdr_info
        NULL,         //handlers,
        NULL,         //controls
        0,
        0             //add data
    },
//END_DCL_3DAUTOCHECKBTN
};

static NCS_MNWND_TEMPLATE mymain_templ = {
    NCSCTRL_MAINWND,
    1,
    0, 0, 260, 180,
    WS_CAPTION | WS_BORDER | WS_VISIBLE,
    WS_EX_NONE,
    "Push Button",
    NULL,
    NULL,
    mymain_handlers,
    _ctrl_templ,
    sizeof(_ctrl_templ)/sizeof(NCS_WND_TEMPLATE),
    0,
    0, 0,
};
// END_OF_TEMPLATE

int MiniGUIMain(int argc, const char* argv[])
{
    ncsInitialize();

    mDialogBox* mydlg = (mDialogBox *)ncsCreateMainWindowIndirect
                (&mymain_templ, HWND_DESKTOP);

    _c(mydlg)->doModal(mydlg, TRUE);

    MainWindowThreadCleanup(mydlg->hwnd);

    ncsUninitialize ();

    return 0;
}
```

- Define common button
```cpp
    {
        NCSCTRL_BUTTON,
        ID_BTN,
        30, 30, 80, 25,
        WS_BORDER | WS_VISIBLE,
        WS_EX_NONE,
        "button",
        NULL,         //props,
        btn_rdr_info, //rdr_info
        NULL,         //handlers,
        NULL,         //controls
        0,
        0             //add data
    },
```

- Define Image button, generally it is handled in `onCreate` event of the main
window
```cpp
    {
        NCSCTRL_BUTTON,
        ID_BTN1,
        150, 30, 80, 25,
        WS_VISIBLE | NCSS_BUTTON_IMAGE,
        WS_EX_NONE,
        "Image",
        NULL,         //props,
        btn_rdr_info, //rdr_info
        NULL,         //handlers,
        NULL,         //controls
        0,
        0             //add data
    },
```
- Load and set Image
```cpp
    if(LoadBitmapFromFile(HDC_SCREEN, &bmp, "icon_button.png")!=0)
    {
        printf("cannot load image file \"icon_button.png\"\n");
    }

    mButton *mb1 = (mButton*)ncsGetChildObj(self->hwnd, ID_BTN1);

    if(mb1)
        _c(mb1)->setProperty(mb1, NCSP_BUTTON_IMAGE, (DWORD)&bmp);
```

- Define button of Autocheck style
```cpp
    {
        NCSCTRL_BUTTON,
        ID_BTN1,
        30, 80, 80, 25,
        WS_VISIBLE  | NCSS_BUTTON_AUTOCHECK | NCSS_BUTTON_CHECKABLE,
        WS_EX_NONE,
        "Auto button",
        NULL,         //props,
        btn_rdr_info, //rdr_info
        NULL,         //handlers,
        NULL,         //controls
        0,
        0             //add data
    },
```

- Define button of three status check style
```cpp
    {
        NCSCTRL_BUTTON,
        ID_BTN1,
        150, 80, 80, 25,
        WS_VISIBLE | NCSS_BUTTON_AUTOCHECK | NCSS_BUTTON_CHECKABLE \
            | NCSS_BUTTON_3DCHECK,
        WS_EX_NONE,
        "3D button",
        NULL,         //props,
        btn_rdr_info, //rdr_info
        NULL,         //handlers,
        NULL,         //controls
        0,
        0             //add data
    },
```

## `mCheckButton`
- *Control window class*: `NCSCTRL_CHECKBUTTON`
- *Control English name*: `CheckButton`
- *Brief introduction*: Check box button, mainly used on multi-selection
occasion. The common pattern is a box, and the user checks or unchecks the box.

![alt](figures/check_button.png)


### Property of `mCheckButton`
It is inherited from the property of `mButton`.

### Event of `mCheckButton`
It is inherited from the event of `mButton`.

### Renderers of `mCheckButton`
- For the usage of renderers, see [look and feel renderer](MiniGUIProgGuidePart2Chapter03.md)

#### `mCheckButton` Classic Renderer

- Drawing of renderer in the `mCheckButton` Classic text region is as below:
for the drawing of non client area, please refer to the renderer of
[mWidget](MiniGUIProgGuidePart2Chapter04.md#mwidget).

| *Property ID* | *Meaning* | *miniStudio property name* | *Value type* | *Schematic diagram of the valid region* | *Value range* |
|---------------|-----------|----------------------------|--------------|-----------------------------------------|---------------|
| `NCS_FGC_3DBODY` | Text foreground color | `FgColor3DBody` | `DWORD` (`ARGB`) | <img src="figures/checkbutton_classic_fgc.png" alt="checkbutton_classic_fgc.png"/> | |
| `NCS_FGC_DISABLED_ITEM` | Text foreground color when the window is invalid | `TextDisableColor` | `DWORD` (`ARGB`) | <img src="figures/checkbutton_classic_textdisable.png" alt="checkbutton_classic_textdisable.png"/> | |

- Classic renderer of Check box fills and draws through loading image resource.
(image resource is minigui-res by default, and installed in
`/usr/local/share/minigui/res/bmp/classic_check_button.bmp`）by default
- Specification of image: the image is composed of eight parts from left to
right, and each part is a square (13x13), corresponding to eight statuses of
checkbutton:
* 0~3: common, highlight, pressed down and banned statuses when not selected
* 4~7: common, highlight, pressed down and banned statuses when selected
- If the image is bigger than the drawing region, the image will be reduced to
draw; if the image is smaller or equal to the drawing region, the image actual
size will be adopted to draw
- Example

![alt](figures/classic_check_button.png)


#### `mCheckButton` Skin Renderer

Refer to [Appendix B : Specification for the Image Resource Used by Skin
Renderer](MStudioMGNCSV1dot0PGENAppB#mButton)

#### `mCheckButton` Fashion Renderer
- Drawing of `mCheckButton` Fashion text region renderer is as follows: for the
drawing of non client area, please refer to the renderer of
[mWidget](MiniGUIProgGuidePart2Chapter04.md#mwidget).

| *Property ID* | *Meaning* | *miniStudio property name* | *Value type* | *Schematic diagram of the valid region* | *Value range* |
|---------------|-----------|----------------------------|--------------|-----------------------------------------|---------------|
| `NCS_FGC_3DBODY` | Text foreground color | `FgColor3DBody` | `DWORD` (`ARGB`) | <img src="figures/checkbtn_fashion_fgc.png" alt="checkbtn_fashion_fgc.png"/> | |
| `NCS_FGC_DISABLED_ITEM` | Text foreground color when the window is invalid | `TextDisableColor` | `DWORD` (`ARGB`) | <img src="figures/checkbtn_fashion_disable.png" alt="checkbtn_fashion_disable.png"/> | |

- Fashion renderer of Check box fills and draws through loading image resource.
(Note: the image resource is `minigui-res` by default, and installed in
`/usr/local/share/minigui/res/bmp/fashion_check_btn.bmp` by default)
- Specification of the image: the image is composed of eight parts from top to
bottom, and each part is a square (13x13), corresponding to eight statuses of
checkbutton:
* 0~3: common, highlight, pressed down and banned status when not selected
* 4~7: Common, highlight, pressed down and banned status when selected
- If the image is bigger than the drawing region, the image will be reduced to
draw; if the image is smaller or equal to the drawing region, the image actual
size will be adopted to draw
- Example

![alt](figures/fashion_check_btn.png)


#### `mCheckButton` Flat Renderer

- Drawing of text region and check box of `mCheckButton` Flat renderer is as
below: for the drawing of non area region, please refer to the renderer of
[mWidget](MiniGUIProgGuidePart2Chapter04.md#mwidget).

| *Property ID* | *Meaning* | *miniStudio property name* | *Value type* | *Schematic diagram of the valid region* | *Value range* |
|---------------|-----------|----------------------------|--------------|-----------------------------------------|---------------|
| `NCS_FGC_3DBODY` | Text foreground color | `FgColor3DBody` | `DWORD` (`ARGB`) | <img src="figures/checkbtn_flat_fgc.png" alt="checkbtn_flat_fgc.png"/> | |
| `NCS_BGC_3DBODY`| Background color | `BgColor3DBody` | `DWORD` (`ARGB`) | <img src="figures/checkbtn_flat_bgc.png" alt="checkbtn_flat_bgc.png"/> | |

### Programming Example of `mCheckbutton`

- Operation screen shot: <br />

![alt](figures/checkbutton.png)

- Check box example code

__List 2__ checkbutton.c

```cpp
/*
** checkbutton.c: Sample program for mGNCS Programming Guide
**      The first mGNCS application.
**
** Copyright (C) 2009 ~ 2019 FMSoft Technologies.
*/

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// START_OF_INCS
#include <minigui/common.h>
#include <minigui/minigui.h>
#include <minigui/gdi.h>
#include <minigui/window.h>

#include <mgncs/mgncs.h>
// END_OF_INCS

// START_OF_HANDLERS
static BOOL mymain_onCreate(mWidget* _this, DWORD add_data)
{
    return TRUE;
}

static void mymain_onClose(mWidget* _this, int message)
{
    DestroyMainWindow(_this->hwnd);
    PostQuitMessage(0);
}

static NCS_EVENT_HANDLER mymain_handlers[] = {
    {MSG_CREATE, mymain_onCreate},
    {MSG_CLOSE, mymain_onClose},
    {0, NULL}
};
// END_OF_HANDLERS

// START_OF_RDRINFO
NCS_RDR_ELEMENT btn_rdr_elements[] =
{
    { NCS_MODE_USEFLAT, 1},
    { -1, 0 }
};

static NCS_RDR_INFO btn_rdr_info[] =
{
    {"flat","flat", btn_rdr_elements}
};
// END_OF_RDRINFO

// START_OF_TEMPLATE
#define ID_BTN  101
#define ID_BTN1 102
#define ID_BTN2 103
static NCS_WND_TEMPLATE _ctrl_templ[] = {
    {
        NCSCTRL_CHECKBUTTON,
        ID_BTN,
        20, 30, 120, 25,
        WS_BORDER | WS_VISIBLE,
        WS_EX_NONE,
        "option1",
        NULL,         //props,
        btn_rdr_info, //rdr_info
        NULL,         //handlers,
        NULL,         //controls
        0,
        0             //add data
    },
    {
        NCSCTRL_CHECKBUTTON,
        ID_BTN1,
        20, 60, 120, 25,
        WS_BORDER | WS_VISIBLE | NCSS_BUTTON_AUTOCHECK,
        WS_EX_NONE,
        "option2",
        NULL,         //props,
        btn_rdr_info, //rdr_info
        NULL,         //handlers,
        NULL,         //controls
        0,
        0             //add data
    },
    {
        NCSCTRL_CHECKBUTTON,
        ID_BTN2,
        20, 90, 120, 25,
        WS_BORDER | WS_VISIBLE |NCSS_BUTTON_AUTOCHECK | NCSS_BUTTON_3DCHECK,
        WS_EX_NONE,
        "option3",
        NULL,         //props,
        btn_rdr_info, //rdr_info
        NULL,         //handlers,
        NULL,         //controls
        0,
        0             //add data
    }
};

static NCS_MNWND_TEMPLATE mymain_templ = {
    NCSCTRL_DIALOGBOX,
    1,
    0, 0, 260, 180,
    WS_CAPTION | WS_BORDER | WS_VISIBLE,
    WS_EX_NONE,
    "Check button",
    NULL,
    NULL,
    mymain_handlers,
    _ctrl_templ,
    sizeof(_ctrl_templ)/sizeof(NCS_WND_TEMPLATE),
    0,
    0, 0,
};
// END_OF_TEMPLATE

int MiniGUIMain(int argc, const char* argv[])
{
    ncsInitialize();

    mDialogBox* mydlg = (mDialogBox *)ncsCreateMainWindowIndirect
                (&mymain_templ, HWND_DESKTOP);

    _c(mydlg)->doModal(mydlg, TRUE);

    ncsUninitialize ();

    return 0;
}

```

```cpp
#define ID_BTN  101
#define ID_BTN1 102
#define ID_BTN2 103
static NCS_WND_TEMPLATE _ctrl_templ[] = {
    {
        NCSCTRL_CHECKBUTTON,
        ID_BTN,
        20, 30, 120, 25,
        WS_BORDER | WS_VISIBLE,
        WS_EX_NONE,
        "option1",
        NULL,         //props,
        btn_rdr_info, //rdr_info
        NULL,         //handlers,
        NULL,         //controls
        0,
        0             //add data
    },
    {
        NCSCTRL_CHECKBUTTON,
        ID_BTN1,
        20, 60, 120, 25,
        WS_BORDER | WS_VISIBLE | NCSS_BUTTON_AUTOCHECK,
        WS_EX_NONE,
        "option2",
        NULL,         //props,
        btn_rdr_info, //rdr_info
        NULL,         //handlers,
        NULL,         //controls
        0,
        0             //add data
    },
    {
        NCSCTRL_CHECKBUTTON,
        ID_BTN2,
        20, 90, 120, 25,
        WS_BORDER | WS_VISIBLE |NCSS_BUTTON_AUTOCHECK | NCSS_BUTTON_3DCHECK,
        WS_EX_NONE,
        "option3",
        NULL,         //props,
        btn_rdr_info, //rdr_info
        NULL,         //handlers,
        NULL,         //controls
        0,
        0             //add data
    }
};

static NCS_MNWND_TEMPLATE mymain_templ = {
    NCSCTRL_DIALOGBOX,
    1,
    0, 0, 260, 180,
    WS_CAPTION | WS_BORDER | WS_VISIBLE,
    WS_EX_NONE,
    "Check button",
    NULL,
    NULL,
    mymain_handlers,
    _ctrl_templ,
    sizeof(_ctrl_templ)/sizeof(NCS_WND_TEMPLATE),
    0,
    0, 0,
};
```

## `mRadioButton`
- *Control window class*: `NCSCTRL_RADIOBUTTON`
- *Control English name*: `RadioButton`
- *Brief introduction*: Single selection button, matched with `mButtonGroup`.
`mButtonGroup` concludes multiple single selection buttons into one group.
Among the buttons in the group, only one is in the selected status. After a
button is selected, the originally selected button will cancel the selection.
When the single selection button is used independently, its behavior is similar
to that of a <a ref="#m_Checkbutton">mCheckbutton</a>.

![alt](figures/radio_button.png)


### Property of `mRadiobutton`
It is inherited from the property of <a href="#Property of
`mButton"/>mButton</a>`.

### Event of `mRadiobutton`

It is inherited from the event of `mButton`.

### Renderers of `mRadiobutton`

- For the usage of renderers, see [look and feel renderer](MiniGUIProgGuidePart2Chapter03.md).

#### `mRadiobutton` Classic Renderer

- For the drawing of `mRadiobutton` Classic non client area, please refer to
the renderer of [mWidget](MiniGUIProgGuidePart2Chapter04.md#mwidget).

| *Property ID* | *Meaning* | *miniStudio property name* | *Value type* | *Schematic diagram of the valid region* | *Value range* |
|---------------|-----------|----------------------------|--------------|-----------------------------------------|---------------|
| `NCS_FGC_3DBODY` | Text foreground color | `FgColor3DBody` | `DWORD` (`ARGB`) | <img src="figures/radiobutton_classic_fgc.png" alt="radiobutton_classic_fgc.png"/> | |
| `NCS_FGC_DISABLED_ITEM` | Text foreground color when the window is invalid | `TextDisableColor` | `DWORD` (`ARGB`) | <img src="figures/radiobutton_classic_textdisable.png" alt="radiobutton_classic_textdisable.png"/> | |

- Classic renderer of Check Box of Radiobutton fills and draws through loading
image resource. (the image resource is minigui-res by default, and installed in
`/usr/local/share/minigui/res/bmp/classic_radio_button.bmp` by default)
- Specification of the image: the image is composed of eight parts from left to
right, and each part is a square (13x13), corresponding to eight statuses of
checkbutton:
* 0~3: common, highlight, pressed down and banned status when not selected
* 4~7: common, highlight, pressed down and banned status when selected
- If the image is bigger than the drawing region, the image will be reduced to
draw; when the image is smaller than or equal to the drawing region, the image
actual size is adopted to draw
- Example

![alt](figures/classic_radio_button.png)


#### `mRadiobutton` Skin Renderer

Refer to [Appendix B : Specification for the Image Resource Used by Skin
Renderer](MStudioMGNCSV1dot0PGENAppB#mButton).

#### `mRadiobutton` Fashion Renderer
- For the drawing of `mRadiobutton` Fashion non client area, please refer to
the renderer of [mWidget](MiniGUIProgGuidePart2Chapter04.md#mwidget).

| *Property ID* | *Meaning* | *miniStudio property name* | *Value type* | *Schematic diagram of the valid region* | *Value range* |
|---------------|-----------|----------------------------|--------------|-----------------------------------------|---------------|
| `NCS_FGC_3DBODY` | Text foreground color | `FgColor3DBody` | `DWORD` (`ARGB`) | <img src="figures/radiobtn_fashion_fgc.png" alt="radiobtn_fashion_fgc.png"/> | |
| `NCS_FGC_DISABLED_ITEM` | Text foreground color when the window is invalid | `TextDisableColor` | `DWORD` (`ARGB`) | <img src="figures/radiobtn_fashion_disable.png" alt="radiobtn_fashion_disable.png"/> | |

- Fashion renderer of Check Box of Radiobutton fills and draws through loading
image resource. (The image resource is minigui-res by default, and installed in
/usr/local/share/minigui/res/bmp/fashion_radio_btn.bmp by default)
- Specification of the image: the image is composed of eight parts from top to
bottom, and each part is a square (13x13), corresponding to eight statuses of
checkbutton:
* 0~3: common, highlight, pressed down and banned status when not selected
* 4~7: common, highlight, pressed down and banned status when selected
- If the image is bigger than the drawing region, the image will be reduced to
draw; when the image is smaller than or equal to the drawing region, the image
actual size is adopted to draw
- Example

![alt](figures/fashion_radio_btn.png)


#### `mRadiobutton` Flat Renderer
- For the drawing of `mRadiobutton` Flat non client area, please refer to the
renderer of [mWidget](MiniGUIProgGuidePart2Chapter04.md#mwidget).

| *Property ID* | *Meaning* | *miniStudio property name* | *Value type* | *Schematic diagram of the valid region* | *Value range* |
|---------------|-----------|----------------------------|--------------|-----------------------------------------|---------------|
| `NCS_FGC_3DBODY` | Text foreground color | `FgColor3DBody` | `DWORD` (`ARGB`) | <img src="figures/radiobtn_flat_fgc.png" alt="radiobtn_flat_fgc.png"/> | |
| `NCS_BGC_3DBODY`| Background color | `BgColor3DBody` | `DWORD` (`ARGB`) | <img src="figures/radiobtn_flat_bgc.png" alt="radiobtn_flat_bgc.png"/> | |

### Programming Example of `mRadiobutton`

- Operation screen shot: <br />

![alt](figures/radiobutton.png)


- Single selection button example code

```cpp
static NCS_PROP_ENTRY radioGroup_props [] = {
    {NCSP_BUTTON_GROUPID, 200},
    {0, 0}
};

static NCS_WND_TEMPLATE _ctrl_templ[] = {
    {
        NCSCTRL_BUTTONGROUP ,
        ID_GROUP,
        5, 10, 200, 120,
        WS_VISIBLE|NCSS_NOTIFY,
        WS_EX_NONE,
        "Button Group",
        NULL,          //props,
        btn_rdr_info,  //rdr_info
        NULL,          //handlers,
        NULL,          //controls
        0,
        0              //add data
    },
    {
        NCSCTRL_RADIOBUTTON,
        ID_BTN1,
        20, 30, 80, 25,
        WS_VISIBLE,
        WS_EX_NONE,
        "option1",
        radioGroup_props, //props,
        btn_rdr_info,     //rdr_info
        NULL,             //handlers,
        NULL,             //controls
        0,
        0                 //add data
    },
    {
        NCSCTRL_RADIOBUTTON,
        ID_BTN2,
        20, 60, 80, 25,
        WS_VISIBLE,
        WS_EX_NONE,
        "option2",
        radioGroup_props, //props,
        btn_rdr_info,     //rdr_info
        NULL,             //handlers,
        NULL,             //controls
        0,
        0                 //add data
    },
    {
        NCSCTRL_RADIOBUTTON,
        ID_BTN2,
        20, 90, 80, 25,
        WS_VISIBLE,
        WS_EX_NONE,
        "option3",
        radioGroup_props, //props,
        btn_rdr_info,     //rdr_info
        NULL,             //handlers,
        NULL,             //controls
        0,
        0                 //add data
    },
};

static NCS_MNWND_TEMPLATE mymain_templ = {
    NCSCTRL_DIALOGBOX,
    1,
    0, 0, 260, 180,
    WS_CAPTION | WS_BORDER | WS_VISIBLE,
    WS_EX_NONE,
    "Radio button",
    NULL,
    NULL,
    mymain_handlers,
    _ctrl_templ,
    sizeof(_ctrl_templ)/sizeof(NCS_WND_TEMPLATE),
    0,
    0, 0,
};
```

## `mMenuButton`
- *Control window class*: `NCSCTRL_MENUBUTTON`
- *Control English name*: `MenuButton`
- *Brief introduction*: Menu button. Click the button, and the menu will pop
up. Select the menu item through keyboard or mouse, and the selected menu item
will display on the button.

![alt](figures/menu_button.png)


`mMenuButton` is inherited from
[mPopMenuMgr](MiniGUIProgGuidePart2Chapter17.md#mpopmenumgr) Class.

### Property of `mMenuButton`

It is inherited from the property of <a href="#Property of
`mButton"/>mButton</a>`.

| *Property name* | *Type* | *Permission* | *Explanation* |
|-----------------|--------|--------------|---------------|
| `NCSP_MNUBTN_POPMENU` | `mPopMenuMgr*` | `RW` | Set `PopMenu` object pointer |
| `NCSP_MNUBTN_CURITEM` | int | `RW` | Get or set the currently selected id of Menu Item |


### Event of `mMenuButton`

It is inherited from the event of <a href="#Event of `mButton"/>mButton</a>`.

| *Event notification code* | *Explanation* | *Parameter* |
|---------------------------|---------------|-------------|
| `NCSN_MNUBTN_ITEMCHANGED`| When `MenuItem` changes | id value of the new Item |

### Programming Example of `mMenuButton`

`mMenuButton` is mainly implemented through creating
[mPopMenuMgr](MiniGUIProgGuidePart2Chapter17.md#mpopmenumgr) object.

- Operation screen shot: <br />

![alt](figures/menubutton1.png)


![alt](figures/menubutton2.png)

- Menu button example code:

```cpp
/*
** menubutton.c: Sample program for mGNCS Programming Guide
**      The first mGNCS application.
**
** Copyright (C) 2009 ~ 2019 FMSoft Technologies.
*/

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// START_OF_INCS
#include <minigui/common.h>
#include <minigui/minigui.h>
#include <minigui/gdi.h>
#include <minigui/window.h>

#include <mgncs/mgncs.h>
// END_OF_INCS

#define ID_BTN  101

// START_OF_HANDLERS
static void menubutton_onitemchanged(mMenuButton *self, int id, int nc,
                                     DWORD add_data)
{
    char szText[100];
    sprintf(szText,"id=%d",id);
    MessageBox(self->hwnd, szText,"Menu ID",0);
}

static NCS_EVENT_HANDLER menubutton_handlers[] = {
    NCS_MAP_NOTIFY(NCSN_MNUBTN_ITEMCHANGED, menubutton_onitemchanged),
    {0, NULL}
};

static BOOL mymain_onCreate(mWidget* self, DWORD add_data)
{
    mPopMenuMgr * popmenu = NEW(mPopMenuMgr);

    _c(popmenu)->addItem(popmenu,0, "menuitem 1", NULL, 200, 0, NULL, 0);
    _c(popmenu)->addItem(popmenu,0, "menuitem 2", NULL, 201, 0, NULL, 0);
    _c(popmenu)->addItem(popmenu,0, "menuitem 3", NULL, 202, 0, NULL, 0);

    mButton *mb1 = (mButton*)ncsGetChildObj(self->hwnd, ID_BTN);
    if(mb1)
    {
        _c(mb1)->setProperty(mb1, NCSP_MNUBTN_POPMENU, (DWORD)popmenu);
    }

    return TRUE;
}

static void mymain_onClose(mWidget* self, int message)
{
    DestroyMainWindow(self->hwnd);
    PostQuitMessage(0);
}

static NCS_EVENT_HANDLER mymain_handlers[] = {
    {MSG_CREATE, mymain_onCreate},
    {MSG_CLOSE, mymain_onClose},
    {0, NULL}
};
// END_OF_HANDLERS

// START_OF_RDRINFO
NCS_RDR_ELEMENT btn_rdr_elements[] =
{
    { NCS_MODE_USEFLAT, 1},
    { -1, 0 }
};
static NCS_RDR_INFO btn_rdr_info[] =
{
    {"skin", "skin", NULL},
};
// END_OF_RDRINFO

// START_OF_TEMPLATE
static NCS_WND_TEMPLATE _ctrl_templ[] = {
    {
        NCSCTRL_MENUBUTTON,
        ID_BTN,
        40, 40, 100, 30,
        WS_BORDER | WS_VISIBLE,
        WS_EX_NONE,
        "menu button",
        NULL,                //props,
        btn_rdr_info,        //rdr_info
        menubutton_handlers, //handlers,
        NULL,                //controls
        0,
        0                    //add data
    },
};

static NCS_MNWND_TEMPLATE mymain_templ = {
    NCSCTRL_DIALOGBOX,
    1,
    0, 0, 260, 180,
    WS_CAPTION | WS_BORDER | WS_VISIBLE,
    WS_EX_NONE,
    "Menu button",
    NULL,
    NULL,
    mymain_handlers,
    _ctrl_templ,
    sizeof(_ctrl_templ)/sizeof(NCS_WND_TEMPLATE),
    0,
    0, 0,
};
// END_OF_TEMPLATE

int MiniGUIMain(int argc, const char* argv[])
{
    ncsInitialize();

    mDialogBox* mydlg = (mDialogBox *)ncsCreateMainWindowIndirect
                (&mymain_templ, HWND_DESKTOP);

    _c(mydlg)->doModal(mydlg, TRUE);

    MainWindowThreadCleanup(mydlg->hwnd);

    ncsUninitialize ();

    return 0;
}

```

## `mColorButton`
- *Control window class*: `NCSCTRL_COLORBUTTON`
- *Control English name*: `ColorButton`
- *Brief introduction*: Color selection button, which is used to select color.
When clicking the button, color selection box pops up, and the user selects the
color. After confirmation, the selected color displays effect on the button
panel.

![alt](figures/color_button.png)


`mColorButton` uses `ColorSelectDialog` of mgutils to select color, and the
selected color displays in the middle box.

### Property of `mColorButton`

It is inherited from the property of `mButton`.

| *Property name* | *Type* | *Permission* | *Explanation* |
|-----------------|--------|--------------|---------------|
| `NCSP_CLRBTN_CURCOLOR`| `DWORD` (`ARGB`) | `RW` | Set or get Color value |

### Event of `mColorButton`

It is inherited from the event of `mButton`.

| *Event notification code* | *Explanation* | *Parameter* |
|---------------------------|---------------|-------------|
| `NCSN_CLRBTN_COLORCHANGED` | Update after being selected by the user | `DWORD` (`ARGB`) |

### Programming Example of `mColorButton`

- Operation screen shot <br />

![alt](figures/colorbutton.png)

- Color button example code:

```cpp
/*
** colorbutton.c: Sample program for mGNCS Programming Guide
**      The first mGNCS application.
**
** Copyright (C) 2009 ~ 2019 FMSoft Technologies.
*/

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// START_OF_INCS
#include <minigui/common.h>
#include <minigui/minigui.h>
#include <minigui/gdi.h>
#include <minigui/window.h>

#include <mgncs/mgncs.h>
// END_OF_INCS

#define ID_BTN  101

// START_OF_HANDLERS
static BOOL mymain_oncolorchanged(mMainWnd* self, mColorButton *sender,
        int id, DWORD param)
{
    SetWindowBkColor(self->hwnd, DWORD2PIXEL(HDC_SCREEN, param));
    InvalidateRect(self->hwnd, NULL, TRUE);

    return FALSE;
}

static BOOL mymain_onCreate(mWidget* self, DWORD add_data)
{
    mColorButton *btn = (mColorButton*)_c(self)->getChild(self, ID_BTN);
    if(btn)
    {
        _c(btn)->setProperty(btn, NCSP_CLRBTN_CURCOLOR, PIXEL2DWORD(HDC_SCREEN,
            GetWindowBkColor(self->hwnd)));
        ncsAddEventListener((mObject*)btn, (mObject*)self,
            (NCS_CB_ONPIECEEVENT)mymain_oncolorchanged,
            NCSN_CLRBTN_COLORCHANGED);
    }

    return TRUE;
}

static void mymain_onClose(mWidget* self, int message)
{
    DestroyMainWindow(self->hwnd);
    PostQuitMessage(0);
}

static NCS_EVENT_HANDLER mymain_handlers[] = {
    {MSG_CREATE, mymain_onCreate},
    {MSG_CLOSE, mymain_onClose},
    {0, NULL}
};
// END_OF_HANDLERS

// START_OF_RDRINFO
static NCS_RDR_INFO btn_rdr_info[] =
{
      {"fashion", "fashion", NULL},
};
// END_OF_RDRINFO

// START_OF_TEMPLATE
static NCS_WND_TEMPLATE _ctrl_templ[] = {
    {
        NCSCTRL_COLORBUTTON,
        ID_BTN,
        40, 40, 80, 30,
        WS_VISIBLE|NCSS_NOTIFY,
        WS_EX_NONE,
        "button",
        NULL,         //props,
        btn_rdr_info, //rdr_info
        NULL,         //handlers,
        NULL,         //controls
        0,
        0             //add data
    }
};

static NCS_MNWND_TEMPLATE mymain_templ = {
    NCSCTRL_MAINWND,
    1,
    0, 0, 180, 140,
    WS_CAPTION | WS_BORDER | WS_VISIBLE,
    WS_EX_NONE,
    "Color button",
    NULL,
    NULL,
    mymain_handlers,
    _ctrl_templ,
    sizeof(_ctrl_templ)/sizeof(NCS_WND_TEMPLATE),
    0,
    0, 0,
};
// END_OF_TEMPLATE

int MiniGUIMain(int argc, const char* argv[])
{
    ncsInitialize();

    mDialogBox* mydlg = (mDialogBox *)ncsCreateMainWindowIndirect
                (&mymain_templ, HWND_DESKTOP);

    _c(mydlg)->doModal(mydlg, TRUE);

    MainWindowThreadCleanup(mydlg->hwnd);

    ncsUninitialize ();

    return 0;
}

```

- Definition processing function
```cpp
static BOOL mymain_oncolorchanged(mMainWnd* self, mColorButton *sender,
        int id, DWORD param)
{
    SetWindowBkColor(self->hwnd, DWORD2PIXEL(HDC_SCREEN, param));
    InvalidateRect(self->hwnd, NULL, TRUE);

    return FALSE;
}

static BOOL mymain_onCreate(mWidget* self, DWORD add_data)
{
    mColorButton *btn = (mColorButton*)_c(self)->getChild(self, ID_BTN);
    if(btn)
    {
        _c(btn)->setProperty(btn, NCSP_CLRBTN_CURCOLOR, PIXEL2DWORD(HDC_SCREEN,
            GetWindowBkColor(self->hwnd)));
        ncsAddEventListener((mObject*)btn, (mObject*)self,
            (NCS_CB_ONPIECEEVENT)mymain_oncolorchanged,
            NCSN_CLRBTN_COLORCHANGED);
    }

    return TRUE;
}

static void mymain_onClose(mWidget* self, int message)
{
    DestroyMainWindow(self->hwnd);
    PostQuitMessage(0);
}

static NCS_EVENT_HANDLER mymain_handlers[] = {
    {MSG_CREATE, mymain_onCreate},
    {MSG_CLOSE, mymain_onClose},
    {0, NULL}
};
```

- Define the window template
```cpp
static NCS_WND_TEMPLATE _ctrl_templ[] = {
    {
        NCSCTRL_COLORBUTTON,
        ID_BTN,
        40, 40, 80, 30,
        WS_VISIBLE|NCSS_NOTIFY,
        WS_EX_NONE,
        "button",
        NULL,         //props,
        btn_rdr_info, //rdr_info
        NULL,         //handlers,
        NULL,         //controls
        0,
        0             //add data
    }
};

static NCS_MNWND_TEMPLATE mymain_templ = {
    NCSCTRL_MAINWND,
    1,
    0, 0, 180, 140,
    WS_CAPTION | WS_BORDER | WS_VISIBLE,
    WS_EX_NONE,
    "Color button",
    NULL,
    NULL,
    mymain_handlers,
    _ctrl_templ,
    sizeof(_ctrl_templ)/sizeof(NCS_WND_TEMPLATE),
    0,
    0, 0,
};
```

## Appendix:A

### `GrandientMode`
It must be one of the values below:
- `MP_LINEAR_GRADIENT_MODE_HORIZONTAL`: horizontal gradual change, gradual
change from the center to the left and right
- `MP_LINEAR_GRADIENT_MODE_VERTICAL`: vertical gradual change, gradual change
from the center to the top and bottom
The two values are defined in mGPlus (`<mgplus/mgplus.h>`). Although mGPlus
defines other gradual change modes as well, it is not supported in mGNCS.

### `ButtonState`
One of the following four statuses:
- Normal: normal status.
- Hilight: the status when the mouse is moved to the button and not clicked,
also referred to as highlight status.
- Checked: the button is in the status of being clicked, also referred to as
checked status. In this status, different statuses are subdivided, refer to the
definition of `ButtonCheckState` status below.
- Disabled: the button is in disabled status.


### `ButtonCheckState`

```cpp
/* Button CHECK status subdivided definition */
enum mButtonCheckState{
        /* Button CHECK status subdivided definition */
        NCS_BUTTON_UNCHECKED = 0,

        /* Under three status condition, the status is valid, which can be understood as the middle transition status */
        NCS_BUTTON_HALFCHECKED,

        /* Represent pressed down status */
        NCS_BUTTON_CHECKED
};
```

[&lt;&lt; Static Box and Derived Control Classes](MiniGUIProgGuidePart2Chapter05.md) |
[Table of Contents](README.md) |
[Panel and Derived Classes &gt;&gt;](MiniGUIProgGuidePart2Chapter07.md)

[Release Notes for MiniGUI 3.2]: /supplementary-docs/Release-Notes-for-MiniGUI-3.2.md
[Release Notes for MiniGUI 4.0]: /supplementary-docs/Release-Notes-for-MiniGUI-4.0.md
[Showing Text in Complex or Mixed Scripts]: /supplementary-docs/Showing-Text-in-Complex-or-Mixed-Scripts.md
[Supporting and Using Extra Input Messages]: /supplementary-docs/Supporting-and-Using-Extra-Input-Messages.md
[Using CommLCD NEWGAL Engine and Comm IAL Engine]: /supplementary-docs/Using-CommLCD-NEWGAL-Engine-and-Comm-IAL-Engine.md
[Using Enhanced Font Interfaces]: /supplementary-docs/Using-Enhanced-Font-Interfaces.md
[Using Images and Fonts on System without File System]: /supplementary-docs/Using-Images-and-Fonts-on-System-without-File-System.md
[Using SyncUpdateDC to Reduce Screen Flicker]: /supplementary-docs/Using-SyncUpdateDC-to-Reduce-Screen-Flicker.md
[Writing DRI Engine Driver for Your GPU]: /supplementary-docs/Writing-DRI-Engine-Driver-for-Your-GPU.md
[Writing MiniGUI Apps for 64-bit Platforms]: /supplementary-docs/Writing-MiniGUI-Apps-for-64-bit-Platforms.md

[Quick Start]: /user-manual/MiniGUIUserManualQuickStart.md
[Building MiniGUI]: /user-manual/MiniGUIUserManualBuildingMiniGUI.md
[Compile-time Configuration]: /user-manual/MiniGUIUserManualCompiletimeConfiguration.md
[Runtime Configuration]: /user-manual/MiniGUIUserManualRuntimeConfiguration.md
[Tools]: /user-manual/MiniGUIUserManualTools.md
[Feature List]: /user-manual/MiniGUIUserManualFeatureList.md

[MiniGUI Overview]: /MiniGUI-Overview.md
[MiniGUI User Manual]: /user-manual/README.md
[MiniGUI Programming Guide]: /programming-guide/README.md
[MiniGUI Porting Guide]: /porting-guide/README.md
[MiniGUI Supplementary Documents]: /supplementary-docs/README.md
[MiniGUI API Reference Manuals]: /api-reference/README.md

[MiniGUI Official Website]: http://www.minigui.com
[Beijing FMSoft Technologies Co., Ltd.]: https://www.fmsoft.cn
[FMSoft Technologies]: https://www.fmsoft.cn
[HarfBuzz]: https://www.freedesktop.org/wiki/Software/HarfBuzz/
