# NL-X Theme Documentation

This documentation covers the **complete** theming system for NL-X. It explains every single property, what it does, where it appears on screen, and how to customize it. You do NOT need access to the NL-X source code to create themes - this guide tells you everything.

---

## Table of Contents

1. [Overview](#overview)
2. [Understanding the NL-X Windows](#understanding-the-nl-x-windows)
3. [Theme File Location](#theme-file-location)
4. [Theme Structure](#theme-structure)
5. [Property Types Explained](#property-types-explained)
6. [Color Format (TColor)](#color-format)
7. [Font Format (TFont)](#font-format)
8. [Complete WPF Font List](#complete-wpf-font-list)
9. [Image Format (TImage)](#image-format)
10. [Opacity Explained](#opacity-explained)
11. [Button Types Explained](#button-types-explained)
12. [Load Section - Complete Reference](#load-section)
13. [Main Section - Complete Reference](#main-section)
14. [ScriptHub Section - Complete Reference](#scripthub-section)
15. [All Customizable Strings](#all-customizable-strings)
16. [Ace Editor - Complete Guide](#ace-editor-complete-guide)
17. [Troubleshooting](#troubleshooting)

---

## Overview

NL-X uses a JSON-based theming system that allows complete customization of the application's appearance. The theme file controls colors, fonts, images, opacity, and text for all UI elements across three main windows:

- **Loader** - The initial loading window (shows during startup)
- **KeyPage** - Where users enter their key (uses Load theme settings)
- **PremiumLogin** - Where premium users log in (uses Load theme settings)
- **Main** - The primary application window with the script editor
- **ScriptHub** - The script browser popup window

---

## Understanding the NL-X Windows

This section describes each window and where every UI element is located.

### Loader Window (Loading Screen)

This is the first window that appears when you start NL-X.

**Layout (top to bottom):**
- **TopBox** - A colored horizontal bar at the very top of the window
- **TitleBox** - Sits inside/below the TopBox, displays "NL-X - Loader" text
- **Logo** - Optional image in the center area (if you set an image path)
- **StatusBox** - Text in the center showing loading status like "Starting up..."
- **ProgressBar** - A horizontal loading bar near the bottom

**What happens during loading:**
1. Window opens showing your `StatusBox.Text` (default: "Initializing...")
2. Checks for updates and downloads files
3. Validates credentials if saved
4. Navigates to KeyPage (if no saved credentials) or MainWindow

### KeyPage Window (Key Entry)

This window appears when a user needs to enter their key or log in.

**Layout (top to bottom):**
- **TopBox** - Colored bar at top
- **TitleBox** - Window title, shows "NL-X" or status messages
- **Close Button** - Top-right corner (X button)
- **WelcomeLabel** - Welcome text like "Welcome to NL-X"
- **Key Input Box** - Text field where user types their key (styled by `Main.ScriptBox`)
- **Remember Me Checkbox** - Option to save the key
- **Buttons** - "Get Key", "Submit", "Premium Login" (styled by `Main.ExecuteButton`)

**Theme properties used:**
- `Load.TitleBox` - Title styling (font, colors)
- `Load.WelcomeLabel` - Welcome message text and styling
- `Main.ScriptBox` - Key input box styling (background, text color, font)
- `Main.ExecuteButton` - Button styling (colors, gradient, font)

### Main Window (Script Executor)

This is the primary window where you write and execute scripts.

**Layout:**
- **TopBox** - Colored bar at very top
- **TitleBox** - Window title showing "NL-X - {version}", updates with status during attach
- **Minimize Button** - Top-right, the (─) icon
- **Exit Button** - Top-right corner, the (×) icon
- **Logo** - Optional image (if path set)
- **TabControl** - Script tabs area (Script 1, Script 2, etc.) with [+] to add new tabs
- **Ace Editor** - The large code editing area (customized via Ace.html file, NOT theme JSON)
- **ScriptBox** - File list on the right side showing .lua files from your scripts folder
- **Buttons at bottom** - Execute, Clear, Open File, Execute File, Save File, Options, Attach, Script Hub

### ScriptHub Window (Script Browser)

A popup window for browsing and running pre-made scripts.

**Layout:**
- **TopBox** - Colored bar at top
- **TitleBox** - Shows "NL-X - Script Hub"
- **Minimize Button** - (─) icon
- **Close Button** - (×) icon (styled differently than main close)
- **ScriptBox** - List of available scripts on the left
- **DescriptionBox** - Description of selected script on the right
- **ExecuteButton** - Runs the selected script (Yield button - shows "Executing..." while running)
- **CloseButton** - Closes the Script Hub window

---

## Theme File Location

The theme file must be placed at:
```
bin/theme-wpf.json
```

If no theme file exists, the application uses default styles.

### Included Template Files

This documentation comes with ready-to-use template files:

| File | Description |
|------|-------------|
| `theme-wpf-blank.json` | Blank template with default values - start from scratch |
| `Ace-blank.html` | Blank Ace editor template with required functions |
| `theme-wpf.json` | Complete Cyberpunk theme example (already in bin folder) |
| `Ace.html` | Complete Cyberpunk Ace editor (already in bin folder) |

**To create a new theme:** Copy `theme-wpf-blank.json` to `bin/theme-wpf.json` and `Ace-blank.html` to `bin/Ace.html`, then customize.

---

## Theme Structure

The theme JSON has this top-level structure:

```json
{
  "Version": 1,
  "Load": { ... },
  "Main": { ... },
  "ScriptHub": { ... }
}
```

---

## Color Format

All colors use ARGB format:

```json
{
  "A": 255,    // Alpha (0-255, 0 = transparent, 255 = opaque)
  "R": 200,    // Red (0-255)
  "G": 150,    // Green (0-255)
  "B": 255     // Blue (0-255)
}
```

**Examples:**
- Transparent: `{ "A": 0, "R": 0, "G": 0, "B": 0 }`
- White: `{ "A": 255, "R": 255, "G": 255, "B": 255 }`
- Red: `{ "A": 255, "R": 255, "G": 0, "B": 0 }`
- Semi-transparent black: `{ "A": 128, "R": 0, "G": 0, "B": 0 }`

---

## Font Format

Fonts are defined with name and size:

```json
{
  "Name": "Segoe UI Semibold",
  "Size": 12.0
}
```

**Common Font Names:**
- `Segoe UI`
- `Segoe UI Semibold`
- `Segoe UI Bold`
- `Arial`
- `Consolas`
- `Roboto`

---

## Complete WPF Font List

WPF uses the `FontFamily` class which accepts any font installed on the system. The `theme-wpf.json` font names are passed directly to WPF's `FontFamily` constructor.

### Windows Default Fonts (Always Available)

These fonts are included with Windows and guaranteed to work:

**Sans-Serif Fonts:**
| Font Name | Variants | Best For |
|-----------|----------|----------|
| `Segoe UI` | Light, Semilight, Regular, Semibold, Bold, Black | Modern UI (Windows 10/11 default) |
| `Segoe UI Variable` | Display, Text, Small | Variable font with optical sizes |
| `Arial` | Regular, Bold, Italic, Bold Italic | Classic web-safe font |
| `Calibri` | Light, Regular, Bold, Italic | Office documents |
| `Verdana` | Regular, Bold, Italic | Screen readability |
| `Tahoma` | Regular, Bold | Compact UI text |
| `Trebuchet MS` | Regular, Bold, Italic | Headings |
| `Franklin Gothic Medium` | Regular | Headlines |
| `Candara` | Light, Regular, Bold, Italic | Elegant UI |
| `Corbel` | Light, Regular, Bold, Italic | Clean modern look |

**Serif Fonts:**
| Font Name | Variants | Best For |
|-----------|----------|----------|
| `Times New Roman` | Regular, Bold, Italic | Documents |
| `Georgia` | Regular, Bold, Italic | Elegant body text |
| `Cambria` | Regular, Bold, Italic | Headings |
| `Palatino Linotype` | Regular, Bold, Italic | Books |
| `Book Antiqua` | Regular, Bold, Italic | Classic look |
| `Garamond` | Regular, Italic | Traditional |
| `Century Schoolbook` | Regular, Bold, Italic | Educational |
| `Constantia` | Regular, Bold, Italic | Formal documents |

**Monospace Fonts (Code/Terminal):**
| Font Name | Variants | Best For |
|-----------|----------|----------|
| `Consolas` | Regular, Bold | Code editing (recommended) |
| `Cascadia Code` | Regular, Bold, Italic + Ligatures | Modern code |
| `Cascadia Mono` | Regular, Bold | Code without ligatures |
| `Courier New` | Regular, Bold, Italic | Classic monospace |
| `Lucida Console` | Regular | Terminal |
| `Source Code Pro` | ExtraLight to Black | Programming |

**Display/Decorative Fonts:**
| Font Name | Style | Best For |
|-----------|-------|----------|
| `Impact` | Bold condensed | Headlines |
| `Arial Black` | Extra bold | Emphasis |
| `Comic Sans MS` | Casual | Informal |
| `Lucida Handwriting` | Script | Signatures |
| `Segoe Script` | Handwritten | Personal touch |
| `Segoe Print` | Handwritten | Notes |
| `Ink Free` | Handwritten | Casual notes |
| `MV Boli` | Handwritten | Friendly |

**Symbol Fonts:**
| Font Name | Contains |
|-----------|----------|
| `Segoe UI Symbol` | Unicode symbols |
| `Segoe UI Emoji` | Color emoji |
| `Segoe MDL2 Assets` | Windows icons |
| `Wingdings` | Classic symbols |
| `Webdings` | Web symbols |
| `Marlett` | Window controls |

### Using Font Variants

To use bold, italic, or other variants, append them to the font name:

```json
{ "Name": "Segoe UI", "Size": 12.0 }           // Regular
{ "Name": "Segoe UI Semibold", "Size": 12.0 }  // Semibold
{ "Name": "Segoe UI Bold", "Size": 12.0 }      // Bold
{ "Name": "Segoe UI Light", "Size": 12.0 }     // Light
{ "Name": "Arial Bold", "Size": 14.0 }         // Arial Bold
{ "Name": "Consolas Bold", "Size": 11.0 }      // Consolas Bold
```

### Custom/Installed Fonts

Any font installed on the user's Windows system can be used. Popular custom fonts:

- `Roboto` - Google's Android font
- `Open Sans` - Clean web font
- `Lato` - Modern sans-serif
- `Montserrat` - Geometric font
- `Poppins` - Rounded modern font
- `Inter` - UI-optimized font
- `JetBrains Mono` - Coding font
- `Fira Code` - Coding font with ligatures
- `Hack` - Coding font

**Note:** If a font is not installed, WPF falls back to the default system font.

### Font Size Guidelines

| Use Case | Recommended Size |
|----------|------------------|
| Window titles | 12-14 |
| Button text | 12-14 |
| Body text | 11-12 |
| Small labels | 10-11 |
| Code editor | 12-14 |
| Status text | 10-12 |

---

## Image Format

Images can be local or online:

```json
{
  "Path": "path/to/image.png",
  "Online": false
}
```

**Online Image (GIF support):**
```json
{
  "Path": "https://example.com/animated.gif",
  "Online": true
}
```

Supported formats: PNG, JPG, GIF (animated), BMP, WebP

---

## Load Section

The loader window configuration:

```json
"Load": {
  "Base": {
    "Image": { "Path": "", "Online": false },
    "BackColor": { "A": 255, "R": 15, "G": 0, "B": 0 },
    "TopMost": true,
    "Opacity": 1.0
  },
  "Logo": { "Image": { "Path": "", "Online": false } },
  "TopBox": {
    "Enabled": true,
    "BackColor": { "A": 200, "R": 40, "G": 0, "B": 0 },
    "Opacity": 1.0
  },
  "TitleBox": {
    "Enabled": true,
    "Font": { "Name": "Segoe UI Semibold", "Size": 12.0 },
    "Text": "NL-X - Loader",
    "BackColor": { "A": 0, "R": 0, "G": 0, "B": 0 },
    "TextColor": { "A": 255, "R": 200, "G": 150, "B": 255 },
    "Opacity": 1.0
  },
  "StatusBox": {
    "Enabled": true,
    "Font": { "Name": "Segoe UI Semibold", "Size": 14.0 },
    "Text": "Initializing...",
    "BackColor": { "A": 0, "R": 0, "G": 0, "B": 0 },
    "TextColor": { "A": 255, "R": 255, "G": 255, "B": 255 },
    "Opacity": 1.0
  },
  "WelcomeLabel": {
    "Enabled": true,
    "Font": { "Name": "Segoe UI Semibold", "Size": 12.0 },
    "Text": "Welcome to NL-X",
    "BackColor": { "A": 0, "R": 0, "G": 0, "B": 0 },
    "TextColor": { "A": 255, "R": 200, "G": 150, "B": 255 },
    "Opacity": 1.0
  },
  "ProgressBar": {
    "BackColor": { "A": 255, "R": 60, "G": 20, "B": 60 },
    "ForeColor": { "A": 255, "R": 180, "G": 100, "B": 220 }
  }
}
```

### Load Properties

| Property | Description |
|----------|-------------|
| `Base.Image` | Background image for the loader window |
| `Base.BackColor` | Fallback background color |
| `Base.TopMost` | Keep window on top |
| `Base.Opacity` | Window opacity (0.0 - 1.0) |
| `TopBox` | Top bar styling |
| `TitleBox` | Title label styling |
| `StatusBox` | Status text styling (e.g., "Initializing...") |
| `WelcomeLabel` | "Welcome to NL-X" text on access page |
| `ProgressBar` | Loading bar colors |

---

## Main Section

The main application window configuration:

```json
"Main": {
  "Base": {
    "Image": { "Path": "https://example.com/background.gif", "Online": true },
    "BackColor": { "A": 255, "R": 15, "G": 0, "B": 0 },
    "TopMost": false,
    "Opacity": 1.0
  },
  "Editor": {
    "Light": false,
    "FixPixel": { "A": 255, "R": 0, "G": 0, "B": 0 },
    "Opacity": 1.0
  },
  "RightClickStrings": {
    "Execute": "Execute",
    "LoadToEditor": "Load to Editor",
    "Refresh": "Refresh"
  },
  "TopBox": { ... },
  "TitleBox": { ... },
  "ScriptBox": { ... },
  "MinimizeButton": { ... },
  "ExitButton": { ... },
  "ExecuteButton": { ... },
  "ClearButton": { ... },
  "OpenFileButton": { ... },
  "ExecuteFileButton": { ... },
  "SaveFileButton": { ... },
  "OptionsButton": { ... },
  "AttachButton": { ... },
  "ScriptHubButton": { ... },
  "TabControl": { ... }
}
```

### Button Types

**Standard Button (TButton):**
```json
{
  "Image": { "Path": "", "Online": false },
  "Font": { "Name": "Segoe UI Semibold", "Size": 14.0 },
  "BackColor": { "A": 255, "R": 139, "G": 0, "B": 0 },
  "GradientColor": { "A": 255, "R": 60, "G": 0, "B": 0 },
  "TextColor": { "A": 255, "R": 200, "G": 150, "B": 255 },
  "Text": "Execute",
  "Opacity": 1.0
}
```

**Glyph Button (TGlyphButton):**
```json
{
  "BackColor": { "A": 0, "R": 0, "G": 0, "B": 0 },
  "GlyphColor": { "A": 255, "R": 200, "G": 150, "B": 255 },
  "Opacity": 1.0
}
```

**Yield Button (TYieldButton):**
```json
{
  "Image": { "Path": "", "Online": false },
  "Font": { "Name": "Segoe UI Semibold", "Size": 14.0 },
  "BackColor": { "A": 255, "R": 150, "G": 0, "B": 0 },
  "GradientColor": { "A": 255, "R": 70, "G": 0, "B": 0 },
  "TextColor": { "A": 255, "R": 200, "G": 150, "B": 255 },
  "TextNormal": "Attach",
  "TextYield": "Attaching...",
  "Opacity": 1.0
}
```

### TabControl

```json
"TabControl": {
  "BackColor": { "A": 180, "R": 30, "G": 0, "B": 0 },
  "TabBackColor": { "A": 255, "R": 60, "G": 10, "B": 10 },
  "TabActiveColor": { "A": 255, "R": 100, "G": 20, "B": 20 },
  "TabTextColor": { "A": 255, "R": 200, "G": 150, "B": 255 },
  "TabFont": { "Name": "Segoe UI Semibold", "Size": 11.0 },
  "AddButtonColor": { "A": 255, "R": 80, "G": 15, "B": 15 },
  "CloseButtonColor": { "A": 255, "R": 200, "G": 150, "B": 255 },
  "Opacity": 1.0
}
```

| Property | Description |
|----------|-------------|
| `BackColor` | Tab control background |
| `TabBackColor` | Inactive tab background |
| `TabActiveColor` | Active/selected tab background |
| `TabTextColor` | Tab text color (Script 1, Script 2, etc.) |
| `TabFont` | Font for tab names (Script 1, Script 2, etc.) |
| `AddButtonColor` | "+" button background |
| `CloseButtonColor` | "×" close button color |

### ScriptBox (ListBox)

```json
"ScriptBox": {
  "Font": { "Name": "Segoe UI Semibold", "Size": 12.0 },
  "BackColor": { "A": 180, "R": 30, "G": 0, "B": 0 },
  "TextColor": { "A": 255, "R": 200, "G": 150, "B": 255 },
  "Opacity": 1.0
}
```

> **Note:** `ScriptBox.Font` is also applied to text boxes and checkboxes on the Access and Premium Login pages.

### Font Inheritance

Some theme properties are shared across multiple pages:

| Theme Property | Also Used For |
|----------------|---------------|
| `ExecuteButton.Font` | Get Key, Submit, Premium, Login buttons |
| `ScriptBox.Font` | Key text box, password boxes, "Remember Me" checkbox |
| `WelcomeLabel` | "Welcome to NL-X" on Access page |

> ⚠️ **IMPORTANT NOTE:** Due to the nature of NL-X Key and Premium systems, the **Premium Login** and **NL-X Key (Access)** windows will **only be visible if the user does not tick remember me and has logged in as a premium user before.** This is not to limit what you can make your UIs look like, but it is to keep security strict. If you hit remember me on your premium login before and want to view these pages with your theme, run NL-X with this command-line argument in Command Prompt:
```NL-X.exe -reset```
After doing that, you must log into your premium account without hitting "Remember Me", then restart NL-X.

---

## ScriptHub Section

The script hub window configuration:

```json
"ScriptHub": {
  "Base": { ... },
  "TopBox": { ... },
  "TitleBox": { ... },
  "ScriptBox": { ... },
  "DescriptionBox": { ... },
  "MinimizeButton": { ... },
  "ExecuteButton": { ... },
  "CloseButton": { ... }
}
```

---

## Opacity Explained

**What is Opacity?**
Opacity controls how see-through (transparent) an element is. It's a number between 0.0 and 1.0.

```json
"Opacity": 0.85
```

| Value | Effect | Use Case |
|-------|--------|----------|
| `1.0` | Fully solid, nothing shows through | Default, normal look |
| `0.9` | Slightly transparent | Subtle see-through effect |
| `0.7` | Noticeably transparent | Overlay effect, can see background |
| `0.5` | Half transparent | Strong overlay, background clearly visible |
| `0.3` | Very transparent | Ghost-like, barely visible |
| `0.0` | Completely invisible | Element hidden but still takes space |

**Where Opacity Applies:**

| Element | What Gets Transparent |
|---------|----------------------|
| `Base.Opacity` | The ENTIRE window including all contents |
| `TitleBox.Opacity` | Just the title label |
| `StatusBox.Opacity` | Just the status text |
| `Button.Opacity` | Just that specific button |
| `TopBox.Opacity` | Just the top bar |
| `TabControl.Opacity` | The tab area |
| `ScriptBox.Opacity` | The script file list |

**Important Notes:**
- Window `Base.Opacity` affects EVERYTHING inside. Set to 0.5 and all buttons, text, etc. become 50% transparent too.
- Individual element opacity stacks with window opacity. Window at 0.8 + button at 0.5 = button appears at 0.4 (0.8 × 0.5)
- Use Alpha channel in colors for transparency of just the background color, not the whole element

**Opacity vs Alpha (Color Transparency):**
```json
// This makes the ENTIRE button 50% transparent (background AND text):
"Opacity": 0.5

// This makes just the BACKGROUND 50% transparent (text stays solid):
"BackColor": { "A": 128, "R": 255, "G": 0, "B": 0 }
```

---

## Button Types Explained

NL-X has THREE different types of buttons. Each has different properties.

### 1. Standard Button (TButton)

Used for: Execute, Clear, Open File, Execute File, Save File, Options, Attach, Close

```json
{
  "Image": { "Path": "", "Online": false },
  "Font": { "Name": "Segoe UI", "Size": 14.0 },
  "BackColor": { "A": 255, "R": 60, "G": 60, "B": 60 },
  "GradientColor": { "A": 255, "R": 30, "G": 30, "B": 30 },
  "TextColor": { "A": 255, "R": 255, "G": 255, "B": 255 },
  "Text": "Execute",
  "Opacity": 1.0
}
```

| Property | What It Does |
|----------|--------------|
| `Image` | Background image for the button (optional) |
| `Font` | Font name and size for button text |
| `BackColor` | Button background color (or TOP of gradient) |
| `GradientColor` | BOTTOM of gradient (set A=0 to disable gradient) |
| `TextColor` | Color of the button text |
| `Text` | The text displayed on the button |
| `Opacity` | Transparency of the entire button |

**How Gradients Work:**

Gradients create a smooth color transition on buttons. NL-X uses a **45-degree diagonal gradient** from top-left to bottom-right.

```
┌─────────────┐
│ BackColor   │  ← Top-left corner starts with BackColor
│      ↘      │
│        ↘    │  ← Colors blend diagonally
│          ↘  │
│ GradientColor│  ← Bottom-right corner ends with GradientColor
└─────────────┘
```

**Rules:**
1. If `GradientColor.A` (Alpha) is greater than 0 → Gradient is shown
2. If `GradientColor.A` is 0 → Solid `BackColor` is shown (no gradient)
3. If `Image.Path` is set → Image is used instead of any colors

**Example - Red to Dark Red Gradient:**
```json
"BackColor": { "A": 255, "R": 200, "G": 50, "B": 50 },
"GradientColor": { "A": 255, "R": 100, "G": 20, "B": 20 }
```

**Example - Solid Color (No Gradient):**
```json
"BackColor": { "A": 255, "R": 60, "G": 60, "B": 60 },
"GradientColor": { "A": 0, "R": 0, "G": 0, "B": 0 }
```

**Example - Purple to Blue Gradient:**
```json
"BackColor": { "A": 255, "R": 138, "G": 43, "B": 226 },
"GradientColor": { "A": 255, "R": 65, "G": 105, "B": 225 }
```

### 2. Glyph Button (TGlyphButton)

Used for: Minimize button (─), Close/Exit button (×)

These are the small icon buttons in the top-right corner.

```json
{
  "BackColor": { "A": 0, "R": 0, "G": 0, "B": 0 },
  "GlyphColor": { "A": 255, "R": 255, "G": 255, "B": 255 },
  "Opacity": 1.0
}
```

| Property | What It Does |
|----------|--------------|
| `BackColor` | Background behind the icon (usually transparent) |
| `GlyphColor` | Color of the icon symbol (─ or ×) |
| `Opacity` | Transparency of the button |

**Note:** You cannot change the icon symbol itself, only its color.

### 3. Yield Button (TYieldButton)

Used for: Script Hub button, ScriptHub Execute button

**What is a "Yield" Button?**
A yield button has TWO text states:
- **Normal text** - Shown when button is idle
- **Yield text** - Shown when button is processing/loading

Example: Script Hub button shows "Script Hub" normally, but shows "Starting..." while the Script Hub window is loading.

```json
{
  "Image": { "Path": "", "Online": false },
  "Font": { "Name": "Segoe UI", "Size": 14.0 },
  "BackColor": { "A": 255, "R": 60, "G": 60, "B": 60 },
  "GradientColor": { "A": 255, "R": 30, "G": 30, "B": 30 },
  "TextColor": { "A": 255, "R": 255, "G": 255, "B": 255 },
  "TextNormal": "Script Hub",
  "TextYield": "Starting...",
  "Opacity": 1.0
}
```

| Property | What It Does |
|----------|--------------|
| `TextNormal` | Text shown when button is ready/idle |
| `TextYield` | Text shown when button is processing |
| All others | Same as Standard Button |

**Yield Buttons in NL-X:**

| Button | TextNormal | TextYield | When Yield Shows |
|--------|------------|-----------|------------------|
| `Main.ScriptHubButton` | "Script Hub" | "Starting..." | While Script Hub window loads |
| `ScriptHub.ExecuteButton` | "Execute" | "Executing..." | While script is running |

---

## Ace Editor (Complete Guide)

### What is the Ace Editor?

The Ace Editor is the **code editing box** where you type your Lua scripts in the Main window. It's the big text area with syntax highlighting, line numbers, and code completion.

**Important:** The Ace Editor is **NOT** customized through `theme-wpf.json`. It has its own separate file: `Ace.html`.

### Why is it Separate?

The Ace Editor is actually a web-based code editor (like VS Code's editor) running inside NL-X through a WebView2 component. This means:
- It uses HTML, CSS, and JavaScript for styling
- You have FULL control over how it looks
- You can add animations, effects, custom fonts - anything web browsers support

### File Location

The Ace Editor file is located at:
```
bin/Ace.html
```

This is the file you edit to customize the code editor's appearance.

### What You Can Customize

| Element | What It Is |
|---------|-----------|
| Background | The editor's background color or image |
| Text colors | Syntax highlighting for keywords, strings, comments, etc. |
| Cursor | The blinking line where you type |
| Selection | The highlight color when you select text |
| Line numbers | The gutter on the left showing line numbers |
| Scrollbar | The scrollbar appearance |
| Active line | The highlight on the current line |
| Font | The code font family and size |

### What the Theme JSON Controls (Editor Section)

The `Main.Editor` section in `theme-wpf.json` only controls TWO things:

```json
"Editor": {
  "Light": false,
  "FixPixel": { "A": 255, "R": 30, "G": 30, "B": 30 },
  "Opacity": 1.0
}
```

| Property | What It Does |
|----------|--------------|
| `Light` | `true` = light theme, `false` = dark theme (sets base Ace theme) |
| `FixPixel` | A 1-pixel color fix for rendering alignment |
| `Opacity` | Transparency of the entire editor container |

**Everything else** (syntax colors, cursor, scrollbar, etc.) is controlled in `Ace.html`.

### The Power of HTML/CSS

Since Ace.html is rendered in a Chromium-based browser, you have **full access to**:

- **All HTML elements** - Add divs, images, buttons, anything
- **All CSS properties** - Animations, transforms, filters, gradients, shadows
- **All JavaScript** - Event handlers, DOM manipulation, fetch API, animations
- **CSS Animations** - @keyframes, transitions, transforms
- **CSS Filters** - blur, brightness, contrast, hue-rotate, invert
- **CSS Grid & Flexbox** - Complex layouts
- **Web Fonts** - Google Fonts, custom fonts via @font-face
- **Canvas** - 2D/3D graphics
- **SVG** - Vector graphics
- **WebGL** - 3D rendering
- **Audio/Video** - Media elements
- **LocalStorage** - Persistent data

### Ace.html Structure

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    /* ALL YOUR CSS HERE - Full CSS3 support */
  </style>
  <script src="ace/ace.js"></script>
</head>
<body>
  <pre id="editor"></pre>
  <script>
    /* ALL YOUR JAVASCRIPT HERE - Full ES6+ support */
  </script>
</body>
</html>
```

### CSS Customization Examples

#### Make Editor Transparent
```css
html, body {
  background: transparent;
}

#editor {
  opacity: 0.85;
  background-color: transparent;
}

.ace_scroller, .ace_content, .ace_layer {
  background: transparent !important;
}
```

#### Custom Scrollbar
```css
::-webkit-scrollbar {
  width: 10px;
  height: 10px;
}

::-webkit-scrollbar-track {
  background: rgba(40, 0, 40, 0.5);
  border-radius: 5px;
}

::-webkit-scrollbar-thumb {
  background: linear-gradient(180deg, #C896FF, #8B5CF6);
  border-radius: 5px;
}

::-webkit-scrollbar-thumb:hover {
  background: linear-gradient(180deg, #D8B4FE, #A78BFA);
}
```

#### Cursor Styling
```css
.ace_cursor {
  border-left: 2px solid #C896FF !important;
  animation: blink 1s infinite;
}

@keyframes blink {
  0%, 50% { opacity: 1; }
  51%, 100% { opacity: 0; }
}
```

#### Selection Highlight
```css
.ace_selection {
  background: rgba(200, 150, 255, 0.3) !important;
}

.ace_selected-word {
  background: rgba(200, 150, 255, 0.5) !important;
  border: 1px solid #C896FF !important;
}
```

#### Line Numbers (Gutter)
```css
.ace_gutter {
  background: transparent !important;
  color: #C896FF !important;
  border-right: 1px solid rgba(200, 150, 255, 0.2);
}

.ace_gutter-active-line {
  background: rgba(200, 150, 255, 0.2) !important;
}
```

#### Active Line Highlight
```css
.ace_marker-layer .ace_active-line {
  background: rgba(200, 150, 255, 0.1) !important;
}
```

#### Syntax Highlighting Colors
```css
.ace_keyword { color: #FF79C6 !important; }
.ace_string { color: #F1FA8C !important; }
.ace_comment { color: #6272A4 !important; font-style: italic; }
.ace_function { color: #50FA7B !important; }
.ace_variable { color: #BD93F9 !important; }
.ace_constant { color: #FFB86C !important; }
.ace_number { color: #BD93F9 !important; }
.ace_operator { color: #FF79C6 !important; }
```

#### Add Background Image
```css
body {
  background-image: url('https://example.com/background.gif');
  background-size: cover;
  background-position: center;
}
```

#### Add CSS Animations
```css
#editor {
  animation: fadeIn 0.5s ease-in;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}
```

#### Glassmorphism Effect
```css
#editor {
  background: rgba(255, 255, 255, 0.1) !important;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 10px;
}
```

#### Neon Glow Effect
```css
#editor {
  box-shadow: 
    0 0 5px #C896FF,
    0 0 10px #C896FF,
    0 0 20px #C896FF,
    0 0 40px #8B5CF6;
}
```

### JavaScript Customization

#### Set Default Script
```javascript
editor.setValue("print(\"hello\")\n", -1);
```

#### Change Ace Theme
```javascript
editor.setTheme("ace/theme/monokai");
editor.setTheme("ace/theme/dracula");
editor.setTheme("ace/theme/tomorrow_night");
```

#### Editor Options
```javascript
editor.setOptions({
  fontSize: "14px",
  showPrintMargin: false,
  enableLiveAutocompletion: true,
  cursorStyle: "smooth",
  showGutter: true,
  highlightActiveLine: true,
  wrap: true
});
```

#### Custom Key Bindings
```javascript
editor.commands.addCommand({
  name: 'runScript',
  bindKey: {win: 'Ctrl-Enter', mac: 'Command-Enter'},
  exec: function(editor) {
    // Your code here
  }
});
```

#### Add Custom Autocomplete
```javascript
var customCompleter = {
  getCompletions: function(editor, session, pos, prefix, callback) {
    callback(null, [
      {value: "print", score: 1000, meta: "function"},
      {value: "game", score: 900, meta: "global"},
      {value: "workspace", score: 900, meta: "service"}
    ]);
  }
};
editor.completers.push(customCompleter);
```

### Available Ace Themes

| Theme Name | Style |
|------------|-------|
| `monokai` | Dark, vibrant colors |
| `dracula` | Dark purple theme |
| `tomorrow_night` | Dark, muted |
| `github` | Light, clean |
| `twilight` | Dark, warm |
| `terminal` | Green on black |
| `cobalt` | Dark blue |
| `chrome` | Light, minimal |
| `clouds` | Light, soft |
| `solarized_dark` | Dark, easy on eyes |
| `solarized_light` | Light, easy on eyes |
| `nord_dark` | Dark, arctic colors |
| `one_dark` | Atom One Dark |
| `gruvbox` | Retro groove |

### Required JavaScript Functions

**IMPORTANT:** Your Ace.html file MUST define these JavaScript functions for NL-X to work properly. These are called by NL-X to interact with the editor.

```javascript
// REQUIRED - NL-X calls these functions
GetText = () => editor.getValue();           // Returns the code in the editor
SetText = (x) => editor.setValue(x, -1);     // Sets the code in the editor
ClearText = () => editor.setValue("");       // Clears all code
SetTheme = (th) => editor.setTheme("ace/theme/" + th);  // Changes Ace theme
SetOpacity = (o) => document.getElementById('editor').style.opacity = o;  // Sets transparency
```

If these functions are missing, NL-X won't be able to get or set your script code!

### Minimum Required Ace.html

Here's the absolute minimum Ace.html needs to work:

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    html, body { margin: 0; padding: 0; width: 100%; height: 100%; }
    #editor { position: absolute; top: 0; right: 0; bottom: 0; left: 0; }
  </style>
  <script src="ace/ace.js"></script>
</head>
<body>
  <pre id="editor"></pre>
  <script>
    var editor = ace.edit("editor");
    editor.setTheme("ace/theme/monokai");
    editor.session.setMode("ace/mode/lua");
    
    // REQUIRED FUNCTIONS - Do not remove!
    GetText = () => editor.getValue();
    SetText = (x) => editor.setValue(x, -1);
    ClearText = () => editor.setValue("");
    SetTheme = (th) => editor.setTheme("ace/theme/" + th);
    SetOpacity = (o) => document.getElementById('editor').style.opacity = o;
  </script>
</body>
</html>
```

### Complete Custom Theme Example

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    html, body {
      margin: 0;
      padding: 0;
      width: 100%;
      height: 100%;
      background: transparent;
    }

    #editor {
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      opacity: 0.9;
      background: rgba(20, 0, 30, 0.7) !important;
      border: 1px solid rgba(200, 150, 255, 0.3);
      box-shadow: inset 0 0 30px rgba(139, 92, 246, 0.1);
    }

    .ace_scroller { background: transparent !important; }
    .ace_content { background: transparent !important; }
    
    .ace_gutter {
      background: rgba(20, 0, 30, 0.5) !important;
      color: #C896FF !important;
    }

    .ace_cursor {
      border-left: 2px solid #C896FF !important;
    }

    .ace_selection {
      background: rgba(200, 150, 255, 0.25) !important;
    }

    .ace_marker-layer .ace_active-line {
      background: rgba(200, 150, 255, 0.1) !important;
    }

    ::-webkit-scrollbar { width: 8px; }
    ::-webkit-scrollbar-track { background: rgba(20, 0, 30, 0.5); }
    ::-webkit-scrollbar-thumb { 
      background: rgba(200, 150, 255, 0.4);
      border-radius: 4px;
    }
  </style>
  <script src="ace/ace.js"></script>
  <script src="ace/ext-language_tools.js"></script>
</head>
<body>
  <pre id="editor"></pre>
  <script>
    ace.require("ace/ext/language_tools");
    var editor = ace.edit("editor");
    editor.setTheme("ace/theme/tomorrow_night");
    editor.session.setMode("ace/mode/lua");
    editor.setOption("enableLiveAutocompletion", true);
    editor.setShowPrintMargin(false);
    editor.setOption("fontSize", "13px");
    editor.setValue("print(\"hello\")\n", -1);

    // JavaScript functions called from C#
    GetText = () => editor.getValue();
    SetText = (x) => editor.setValue(x, -1);
    ClearText = () => editor.setValue("");
    SetTheme = (th) => editor.setTheme("ace/theme/" + th);
    SetOpacity = (o) => document.getElementById('editor').style.opacity = o;
  </script>
</body>
</html>
```

---

## Complete Example

See `theme-wpf.json` and `Ace.html` for a complete working example with all properties configured.

---

## Tips

1. **Use transparent backgrounds** with alpha channel for overlay effects
2. **GIF backgrounds** work great for animated themes
3. **Gradient buttons** use `BackColor` as top and `GradientColor` as bottom
4. **Test incrementally** - change one section at a time
5. **Backup your theme** before making major changes
6. **Ace.html is just HTML** - use any CSS/JS technique you know

---

## All Customizable Strings

This section documents **ALL customizable strings** in NL-X - every piece of text you can change.

---

### Load Section Strings

These strings appear on the Loader window and KeyPage.

#### StatusBox.Text
```json
"StatusBox": {
  "Text": "Starting up..."
}
```
- **Where:** Center of Loader window
- **What it shows:** The initial status message when NL-X opens
- **Changes to:** Updates automatically during loading process

#### TitleBox.Text
```json
"TitleBox": {
  "Text": "NL-X - Loader"
}
```
- **Where:** Top bar of the Loader window
- **What it shows:** Window title during loading

#### WelcomeLabel.Text
```json
"WelcomeLabel": {
  "Text": "Welcome to NL-X"
}
```
- **Where:** KeyPage window, above the key input box
- **What it shows:** A welcome/greeting message

**Loading Flow:**
```
Your StatusBox.Text ("Initializing...")
         ↓
    [checks for updates]
         ↓
    [downloads files]
         ↓
    [validates credentials if saved]
         ↓
    Main window opens
```

---

### Main Section Strings

These strings appear on the Main window.

#### TitleBox.FormatString
```json
"TitleBox": {
  "FormatString": "NL-X - {version}"
}
```
- **Where:** Top bar of Main window
- **Special:** Use `{version}` and it gets replaced with the actual NL-X version number
- **Example:** `"My Executor - {version}"` → `"My Executor - 1.0.8"`

#### RightClickStrings

Text for the context menu when you right-click a script in the script list:

```json
"RightClickStrings": {
  "Execute": "Execute",
  "LoadToEditor": "Load to Editor",
  "Refresh": "Refresh"
}
```

| String | Default | What It Does |
|--------|---------|--------------|
| `Execute` | "Execute" | Runs the selected script |
| `LoadToEditor` | "Load to Editor" | Opens script in the editor |
| `Refresh` | "Refresh" | Reloads the script file list |

#### Button Text

Every button on the Main window has customizable text:

| Button | JSON Property | Default Text |
|--------|---------------|--------------|
| Execute | `ExecuteButton.Text` | "Execute" |
| Clear | `ClearButton.Text` | "Clear" |
| Open File | `OpenFileButton.Text` | "Open File" |
| Execute File | `ExecuteFileButton.Text` | "Execute File" |
| Save File | `SaveFileButton.Text` | "Save File" |
| Options | `OptionsButton.Text` | "Options" |
| Attach | `AttachButton.Text` | "Attach" |

#### ScriptHubButton (Yield Button)
```json
"ScriptHubButton": {
  "TextNormal": "Script Hub",
  "TextYield": "Starting..."
}
```
- `TextNormal` - Shows when button is ready
- `TextYield` - Shows while Script Hub is loading

---

### ScriptHub Section Strings

#### TitleBox.Text
```json
"TitleBox": {
  "Text": "NL-X - Script Hub"
}
```
- **Where:** Top bar of Script Hub window

#### ExecuteButton (Yield Button)
```json
"ExecuteButton": {
  "TextNormal": "Execute",
  "TextYield": "Executing..."
}
```
- `TextNormal` - Shows when ready
- `TextYield` - Shows while script is running

#### CloseButton.Text
```json
"CloseButton": {
  "Text": "Close"
}
```

---

### Non-Customizable Strings

These messages appear automatically and **CANNOT** be changed via theme:

**Loader/KeyPage:**
- "NL-X - Please enter a key!" (empty key submitted)
- "NL-X - Please enter a valid key!" (invalid key)
- "NL-X - Your key has expired, get a new one"
- "NL-X - Invalid HWID, please create a ticket for help!"
- "NL-X - Connection to servers failed! Please try a VPN!"

**Main Window Title (during attach):**
- "NL-X - Roblox wasn't found!"
- "NL-X - Attaching..."
- "NL-X - Attaching... (Auto Attach)"
- "NL-X - Please attach first!"
- "NL-X - Setting up execution environment..."
- "NL-X - Waiting for you to join a game..."
- "NL-X - Attached"
- "NL-X - Already attached"
- "NL-X - Not yet updated for this Roblox version..."
- "NL-X - Roblox closed"

---

### String Examples

**Gaming Theme:**
```json
"Load": {
  "StatusBox": { "Text": "Loading..." },
  "TitleBox": { "Text": "GameExecutor - Loading" },
  "WelcomeLabel": { "Text": "Ready to play?" }
}
```

**Anime Theme:**
```json
"Load": {
  "StatusBox": { "Text": "Powering up..." },
  "TitleBox": { "Text": "NL-X :: Awakening" },
  "WelcomeLabel": { "Text": "Welcome, Warrior!" }
},
"Main": {
  "TitleBox": { "FormatString": "Ninja Executor - {version}" },
  "ExecuteButton": { "Text": "Unleash!" },
  "AttachButton": { "Text": "Connect" }
}
```

---

## Troubleshooting

### Theme Not Loading
1. Make sure file is named exactly `theme-wpf.json`
2. Make sure it's in the `bin` folder
3. Check JSON syntax - use a JSON validator
4. Make sure you have premium access

### Colors Not Showing
- Check Alpha (A) value is not 0 - that makes it invisible
- Check RGB values are in range 0-255

### Gradient Not Working
- `GradientColor.A` must be greater than 0
- If A is 0, solid color is used instead

### Image Not Loading
- For online images, set `"Online": true`
- Check URL is accessible
- GIF URLs must end in .gif or contain .gif?

### Text Not Changing
- Some messages are hardcoded (see Non-Customizable Strings above)
- Make sure you're editing the right section (Load vs Main)

### Premium/Key Pages Not Visible
- These only show if you haven't saved credentials
- Run `NL-X.exe -reset` to clear saved login
- Log in without checking "Remember Me" to see these pages

### Ace Editor Not Working
- Make sure `bin/Ace.html` exists
- Make sure the required JavaScript functions are defined (GetText, SetText, ClearText, SetTheme, SetOpacity)
- Check browser console for JavaScript errors
- Make sure `bin/ace/ace.js` exists

### Fonts Not Applying
- Font must be installed on the user's system
- Check spelling of font name exactly
- Try a common font like "Segoe UI" to test

---

## Quick Reference

### Color Values
- **Transparent:** `{ "A": 0, "R": 0, "G": 0, "B": 0 }`
- **White:** `{ "A": 255, "R": 255, "G": 255, "B": 255 }`
- **Black:** `{ "A": 255, "R": 0, "G": 0, "B": 0 }`
- **Red:** `{ "A": 255, "R": 255, "G": 0, "B": 0 }`
- **Green:** `{ "A": 255, "R": 0, "G": 255, "B": 0 }`
- **Blue:** `{ "A": 255, "R": 0, "G": 0, "B": 255 }`
- **50% Transparent Black:** `{ "A": 128, "R": 0, "G": 0, "B": 0 }`

### Files You Need
| File | Purpose |
|------|---------|
| `bin/theme-wpf.json` | Main theme configuration (colors, fonts, text, images) |
| `bin/Ace.html` | Code editor styling (syntax colors, cursor, scrollbar) |
| `bin/ace/ace.js` | Ace editor library (don't modify) |

### What Controls What
| Want to change... | Edit this... |
|-------------------|--------------|
| Window backgrounds | `theme-wpf.json` → `Base.Image` or `Base.BackColor` |
| Button colors | `theme-wpf.json` → `[ButtonName].BackColor/GradientColor` |
| Button text | `theme-wpf.json` → `[ButtonName].Text` |
| Title text | `theme-wpf.json` → `TitleBox.Text` or `TitleBox.FormatString` |
| Code editor colors | `Ace.html` → CSS styles |
| Syntax highlighting | `Ace.html` → `.ace_keyword`, `.ace_string`, etc. |

---

*End of NL-X Theme Documentation*
