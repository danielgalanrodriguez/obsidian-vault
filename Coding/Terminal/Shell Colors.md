#terminal #zsh #bash #colours #shell
# 🎨 [[Shell]] Colors Summary: ANSI Escape Codes

Colours and formatting in Zsh, Bash, and most modern terminals are controlled by **ANSI Escape Codes**. These are special, non-printing character sequences that instruct the terminal to change how it renders text.

---

## 1. The ANSI Code Structure

The general format for setting colours is:

$$\text{Escape} + \text{CSI} + \text{Parameters} + \text{Final Character}$$

| Element | Code | Description |
| :--- | :--- | :--- |
| **Escape (Octal)** | `\033` | Signals the start of a command sequence. |
| **CSI** | `[` | Control Sequence Introducer. |
| **Parameters** | e.g., `1;31;40` | Semicolon-separated numbers defining style, foreground, and background. |
| **Final Character** | `m` | Signals the end of the command (**Set Graphics Rendition**). |

---

## 2. Key Colour Variables and Usage

Variables are defined in your script or shell configuration (`.bashrc`, `.zshrc`) and used with the `echo -e` command (the `-e` flag enables the interpretation of escape sequences).

| Variable Example | Code | Parameters | Meaning |
| :--- | :--- | :--- | :--- |
| `RED_TEXT` | `\033[1;31m` | `1` (Bold) + `31` (Red Foreground) | Bold Red Text |
| `BLUE_BG` | `\033[44m` | `44` (Blue Background) | Blue Background |
| **`NC` (No Color)** | `\033[0m` | `0` (Reset) | **Crucial:** Resets all formatting to terminal defaults. |

**Example Usage:**

    RED='\033[1;31m'
    NC='\033[0m'
    echo -e "${RED}This text is red and bold!${NC} This is normal text."

---

## 3. Common Parameter Codes

The parameters are typically grouped by their function. You can combine them (e.g., `1;31;44m` for Bold Red text on a Blue background).

### a. Style Codes

| Code | Style                                                     |
| :--- | :-------------------------------------------------------- |
| `0`  | **Reset/Normal** (resets all)                             |
| `1`  | **Bold** (or Bright/High-intensity color)                 |
| `2`  | **Dim**                                                   |
| `3`  | -                                                         |
| `4`  | **Underline**                                             |
| `5`  | **Blink** - **cool stuff!**                               |
| `6`  | -                                                         |
| `7`  | **Reverse** (invert the foreground and background colors) |
| `8`  | **Hidden** (useful for passwords)                         |

### b. Foreground (Text) Colours

| Code | Standard Color | Code | Bright Color   |
| :--- | :------------- | :--- | :------------- |
| `30` | Black          | `90` | Bright Black   |
| `31` | Red            | `91` | Bright Red     |
| `32` | Green          | `92` | Bright Green   |
| `33` | Yellow         | `93` | Bright Yellow  |
| `34` | Blue           | `94` | Bright Blue    |
| `35` | Magenta        | `95` | Bright Magenta |
| `36` | Cyan           | `96` | Bright Cyan    |
| `37` | White          | `97` | Bright White   |

### c. Background Colours

| Code | Standard Color | Code | Bright Color |
| :--- | :--- | :--- | :--- |
| `40` | Black | `100` | Bright Black |
| `41` | Red | `101` | Bright Red |
| `42` | Green | `102` | Bright Green |
| `43` | Yellow | `103` | Bright Yellow |
| `44` | Blue | `104` | Bright Blue |
| `45` | Magenta | `105` | Bright Magenta |
| `46` | Cyan | `106` | Bright Cyan |
| `47` | White | `107` | Bright White |

---

## 4. Zsh's Built-in Color Handling (For Prompts)

When setting the shell prompt (`PS1`), **Zsh** offers a cleaner, safer syntax that correctly handles character length, preventing display issues.

| Zsh Code | Function | Purpose |
| :--- | :--- | :--- |
| `%F{color}` | Foreground color start (e.g., `%F{red}`). | Set text color. |
| `%f` | Foreground color reset. | Stops coloring text. |
| `%K{color}` | Background color start (e.g., `%K{blue}`). | Set background color. |
| `%k` | Background color reset. | Stops background color. |
| `%B` / `%b` | Bold start / Bold end. | Set text style to bold. |
| `%{...%}` | **Wrapper** | Wraps ANSI codes so Zsh ignores them when calculating prompt length. |

**Example Zsh Prompt:**

    PROMPT='%F{cyan}%K{black} User: %n %k%f > '