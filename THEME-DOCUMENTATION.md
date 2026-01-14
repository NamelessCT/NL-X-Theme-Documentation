# NL-X Theme Documentation

This documentation covers the complete theming system for NL-X, including the `theme-wpf.json` configuration file and the Ace editor customization.

---

## Table of Contents

1. [Overview](#overview)
2. [WPF Compatibility](#wpf-compatibility)
3. [Theme File Location](#theme-file-location)
4. [Theme Structure](#theme-structure)
5. [Color Format](#color-format)
6. [Font Format](#font-format)
7. [Complete WPF Font List](#complete-wpf-font-list)
8. [Image Format](#image-format)
9. [Load Section](#load-section)
10. [Main Section](#main-section)
11. [ScriptHub Section](#scripthub-section)
12. [Opacity Support](#opacity-support)
13. [Ace Editor (Complete Guide)](#ace-editor-complete-guide)

---

## Overview

NL-X uses a JSON-based theming system that allows complete customization of the application's appearance. The theme file controls colors, fonts, images, opacity, and text for all UI elements across three main windows:

- **Loader** - The initial loading window
- **Main** - The primary application window
- **ScriptHub** - The script browser window

---

## WPF Compatibility

NL-X is built with **WPF (Windows Presentation Foundation)** using **XAML** for its UI. The `theme-wpf.json` file is designed to work seamlessly with WPF's rendering system.

### How It Works

1. **JSON → C# Classes**: The theme JSON is deserialized into C# classes (`TButton`, `TLabel`, `TColor`, etc.)
2. **C# → WPF Properties**: These classes are converted to WPF-compatible types:
   - `TColor` → `SolidColorBrush` or `LinearGradientBrush`
   - `TFont` → `FontFamily` + `FontSize`
   - `TImage` → `ImageBrush` or `BitmapImage`
3. **WPF → XAML Controls**: Properties are applied to XAML controls (`Button`, `Label`, `TabControl`, etc.)

### Why WPF?

- **Hardware Acceleration**: GPU-accelerated rendering for smooth animations
- **Vector Graphics**: Resolution-independent UI scaling
- **Rich Styling**: Complete control over every visual element
- **Data Binding**: Dynamic theme updates without restart
- **Transparency**: Native support for layered windows and alpha blending

### XAML Control Mapping

| Theme Property | WPF/XAML Control |
|----------------|------------------|
| `TitleBox` | `Label` |
| `ExecuteButton` | `Button` |
| `ScriptBox` | `ListBox` |
| `TabControl` | `TabControl` + `TabItem` |
| `TopBox` | `Grid` or `Border` |
| `ProgressBar` | `ProgressBar` |

---

## Theme File Location

The theme file must be placed at:
```
bin/theme-wpf.json
```

If no theme file exists, the application uses default styles.

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
  "BaseStrings": {
    "CheckingUpdates": "Checking for updates...",
    "DownloadingData": "Downloading data...",
    "CheckingData": "Checking data...",
    "DownloadingDlls": "Downloading DLLs...",
    "ValidatingKey": "Validating key...",
    "Ready": "Ready!"
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
| `BaseStrings` | Status messages during loading |
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
  "BaseStrings": {
    "Checking": "Checking...",
    "Injecting": "Injecting...",
    "Scanning": "Scanning...",
    "Ready": "Ready",
    "FailedToFindRoblox": "Failed to find Roblox",
    "NotInjected": "Not Injected",
    "AlreadyInjected": "Already Injected"
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

> ⚠️ **IMPORTANT NOTE:** Due to the nature of NL-X Key and Premium systems, the **Premium Login** and **NL-X Key (Access)** windows will **not be visible** at the moment. These windows may be added later for theme inspection/preview purposes once enforcement is implemented.

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

## Opacity Support

All UI elements support an `Opacity` property (0.0 to 1.0):

```json
"Opacity": 0.85
```

- `1.0` = Fully opaque (default)
- `0.5` = 50% transparent
- `0.0` = Fully transparent

---

## Ace Editor (Complete Guide)

The Ace editor is the code editing component in NL-X. It runs inside a WebView2 control, which means **you can do ANYTHING you would normally do in HTML/CSS/JavaScript**.

### File Locations

**Application file:**
```
bin/Ace.html
```

**Example file included in this documentation:**
```
documentation/Ace.html
```

> **Tip:** Check out the `Ace.html` example file in this documentation folder for a working reference!

### The Power of HTML

Since Ace.html is a standard HTML file rendered in a Chromium-based WebView2, you have **full access to**:

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

### C# API Reference

The `NLX.Controls.Ace` class provides these methods:

```csharp
// Properties
bool AceLoaded { get; }              // True when editor is ready

// Events
event Action AceReady;               // Fired when editor loads

// Methods
void SetText(string text);           // Set editor content
Task<string> GetTextAsync();         // Get editor content
void SetOpacity(double opacity);     // Set transparency (0.0-1.0)
void SetTheme(AceTheme theme);       // Light or Dark
void AppendText(string text);        // Add text to end
void GoToLine(int line);             // Scroll to line
void EditorRefresh();                // Refresh display
void UpdateSettings(AceSettings s);  // Update multiple settings
void AddIntellisense(...);           // Add autocomplete entry
void ShowSyntaxError(...);           // Show error marker
```

### AceSettings Class

```csharp
var settings = new AceSettings {
    ReadOnly = false,
    AutoIndent = true,
    Folding = true,
    FontLigatures = false,
    Links = true,
    MinimapEnabled = false,
    LineHeight = 20,
    FontSize = 14,
    FontFamily = "Consolas",
    RenderWhitespace = "none"  // "none", "all", "boundary"
};
editor.UpdateSettings(settings);
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
