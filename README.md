# How to use Julia with Sublime Text 3

## Rationale

This document was written as a personal aide-memoire, which means it will be mostly limited to the ways in which I myself use the editor. Hence the discussion is limited to the programming of Julia and writing of markdown files. I do hope that other people may find it useful too, however.


## Installation

It is possible to go with a preconfigured portable editor, or with the regular version. We start with the portable.

### Grab a portable Sublime Text 3

#### Windows 10

I have configured a [portable version of Sublime Text for Windows](https://github.com/PetrKryslUCSD/HowToUseJuliaWithSublimeText3/blob/master/SublimeTextPortableX64.zip) using the instructions below (with a handful of omissions, for instance Zeal and Tabnine). Simply download, unzip in some good place (such as your `AppData` or `Documents`), and run `subl.exe`. If you also like the Git bash, run Git bash and start `subl.exe` from that shell. 

Example: I unzip SublimeTextPortableX64.zip (with 7-zip it is "Extract Here") in my Documents folder. So the installation is in
```
~/Documents/SublimeTextPortableX64/
```
with these files present in the folder
```
$ ls ~/Documents/SublimeTextPortableX64/
changelog.txt        Packages/         subl.exe*          update_installer.exe*
crash_reporter.exe*  plugin_host.exe*  sublime.py
Data/                python3.3.zip     sublime_plugin.py
msvcr100.dll*        python33.dll*     sublime_text.exe*
```
The executable to run is `subl.exe`. So I open Git bash, and run 
```
~/Documents/SublimeTextPortableX64/subl.exe
```

#### Linux 64-bit

I have configured a [portable version of Sublime Text for Linux](https://github.com/PetrKryslUCSD/HowToUseJuliaWithSublimeText3/blob/master/sublime_text_3_portable_x64.tar.gz)
using the instructions detailed below (with a handful of omissions, for
instance Zeal and Tabnine). Simply download, `tar xzvf` in some good place,
and run `sublime_text`. 

#### Additional configuration of the portable version

If something doesn't work precisely as you wish, you can meddle with the configuration files as described below.

### Sublime Text 3

In the following we describe how to set up the editor from scratch. Download
the editor from the website [https://www.sublimetext.com/](https://www.sublimetext.com/). (Isn't it nice that the download is only 8.8
MB?) As I am on Windows 10, I will describe the installation and use of the
editor on that platform. I do have the Windows Subsystem for Linux installed
and I use it regularly, so I will describe use of Julia running in an Ubuntu
Linux from within the editor running on Windows.

### Install Package Control

The first thing you would want to do is install the package **Package Control**.
Use the menu item **Tools/Command Palette** (key binding: `ctrl+shift+P`), type `install`, and an item **Install Package Control** will bubble up. Select it, and practically instantaneously you will get a confirmation box that the package was installed.

By the way, remember the key binding `ctrl+shift+P`: it will bring up the **Command Palette**, and practically everything that you might need can be done from here.

### Install Julia syntax highlighting

**Package Control** can now be used to install other useful packages.
Bring up the **Command Palette**, and type in a few letters of **Package Control: Install Package**. When it comes up, select it. Loading the registry will take a couple of seconds, and then we get window where we can type in `Julia`. A button named **Julia** (with the subtitle "Julia syntax highlighting for Sublime Text 2/3"; refer to [https://github.com/JuliaEditorSupport/Julia-sublime](https://github.com/JuliaEditorSupport/Julia-sublime)) will come up highlighted. Click on it, and the package will be installed. At this point one should be able to open a Julia source file and get it highlighted based upon the syntax of Julia.

### Install the Terminus and SendCode packages

Repeat the procedure of package installation from **Package Control** to install the terminal emulation package **Terminus** (https://github.com/randy3k/Terminus), and the package to enable communication between a source window and the terminal, **SendCode** (https://github.com/randy3k/SendCode).

### Install the Zeal package (off-line access to the documentation)

Zeal is an open-source browser of off-line documentation.
**Package Control** can also be used to install **Zeal for Sublime Text 2/3**.
The executable of Zeal is required, and can be found at
[https://zealdocs.org/download.html](https://zealdocs.org/download.html).
Finally, the docset for Julia needs to be downloaded within the Zeal documentation browser: start it and under Tools choose Docsets.

## Customization

The key bindings file and the other customization files for the user live in the folder for user settings. In my case that is `C:\Users\PetrKrysl\AppData\Roaming\Sublime Text 3\Packages\User`. For brevity I will call this folder `USER`.

### Basic user settings

Select **Preferences/Settings**. In the "User" settings file (probably opens on the right in your editor) put the following lines:
```
{
    "always_show_minimap_viewport": true,
    "auto_complete": false,
    "auto_complete_commit_on_tab": false,
    "bracket_contents_foreground": "foreground",
    "brackets_options": "foreground",
    "color_scheme": "Packages/Theme - Arzy/Heliopolis.tmTheme",
    "detect_indentation": false,
    "draw_minimap_border": true,
    "draw_white_space": "all",
    "font_face": "Fira Code",
    "font_size": 11,
    "highlight_line": true,
    "ignored_packages":
    [
        "Vintage"
    ],
    "margin": 1,
    "preview_on_click": false,  
    "translate_tabs_to_spaces": true
}
```
I like the following sublime theme: **Arzy.sublime-theme**. If you do too, install this theme, and then add the following line to the above settings `"theme": "Arzy.sublime-theme",`.

### Key bindings

Select **Preferences/Key bindings**. This will open a new window with
the file`Default (Windows).sublime-keymap`, shown in the window on the right.
I put in there my personal preferences for key bindings:
```
[
    // I find it useful to have pasted code immediately re-indented.
    { "keys": ["ctrl+v"], "command": "paste_and_indent" },
    { "keys": ["ctrl+shift+v"], "command": "paste" },
    // This key binding gives me access to code evaluation. A current line,
    // or a selection is passed to the terminal for evaluation
    // (the command belongs to the SendCode package).
    {
        "keys": ["ctrl+keypad_enter"], "command": "send_code",
        "context": [
            { "key": "selector", "operator": "equal", "operand": "source" }
        ]
    },
    // I like to have a key binding for re-indenting of source code
    { "keys": ["ctrl+shift+x", "ctrl+r"],  "command": "reindent" },
    // These two key bindings are used to give me access to the name of the current file and its full path.
    { "keys": ["ctrl+shift+x", "ctrl+alt+c"], "command": "filename_to_clipboard" },
    { "keys": ["ctrl+shift+x", "ctrl+alt+shift+c"], "command": "path_to_clipboard" },
    // To make the copy and paste keys work in the Terminus window
    // (otherwise they are ctrl+shift+c, ctrl+shift+v)
    { "keys": ["ctrl+c"], "command": "terminus_copy",
        "context": [
            { "key": "terminus_view" },
            { "key": "terminus_view.natural_keyboard" },
            { "key": "selection_empty", "operator": "equal", "operand": false, "match_all": true }
        ]
    },
    { "keys": ["ctrl+v"], "command": "terminus_paste",
        "context": [
            { "key": "terminus_view" },
            { "key": "terminus_view.natural_keyboard" }
        ]
    },
    // Send code to change the working folder to that holding the current Julia file
    {
        "keys": ["ctrl+shift+x", "ctrl+f"], "command": "send_code",
        "args": {"cmd": "cd(\"$file_path\")"},
        "context": [
            { "key": "selector", "operator": "equal", "operand": "source.julia" }
        ]
    },
    // Send code to display help information. Thanks to @mbauman for the suggestion.
    {
        "keys": ["ctrl+shift+x", "ctrl+h"], "command": "send_code",
        "args": {"cmd": "REPL.@repl $selection"},
        "context": [
            { "key": "selector", "operator": "equal", "operand": "source.julia" }
        ]
    },
    // Send code to display help information. 
    {
        "keys": ["ctrl+shift+x", "ctrl+alt+h"], "command": "send_code",
        "args": {"cmd": "?$selection"},
        "context": [
            { "key": "selector", "operator": "equal", "operand": "source.julia" }
        ]
    },
    { "keys": ["ctrl+shift+="], "command": "increase_font_size" },
    { "keys": ["ctrl+="], "command": "increase_font_size" },
    // This is to make the "go to anything" key available in the Terminus view
    {
        "keys": ["ctrl+p"],
        "command": "show_overlay",
        "args": {"overlay": "goto", "show_files": true},
        "context": [
            { "key": "terminus_view" },
        ]
    },
]
```
The key bindings may seem Baroque, but I use voice recognition which means I say a command, which sends these keys. (The point is to avoid typing these contortions on the keyboard.)

The commands `filename_to_clipboard`, `path_to_clipboard`, for the key bindings that enable access to the current file name and path are provided by a little plug-in:
```
import sublime, sublime_plugin, os
class FilenameToClipboardCommand(sublime_plugin.TextCommand):
    def run(self, edit):
        sublime.set_clipboard(os.path.basename(self.view.file_name()))
class PathToClipboardCommand(sublime_plugin.TextCommand):
    def run(self, edit):
        sublime.set_clipboard(self.view.file_name())
```
I store this in the user-settings folder as
`USER\copyfilename.py`.

### Customization of the Terminus package

In the file
`USER\Terminus.sublime-settings` I customize the Linux shell of the Windows Subsystem for Linux, which in my case is Ubuntu 18.04. I also set it to be the default shell to be started by Terminus:
```
{
    // a list of available shells to execute
    // the shell marked as "default" will be the default shell
    "shell_configs": [
        {
            "name": "Command Prompt",
            "cmd": "bash.exe",
            "env": {},
            "enable": true,
            "default": true,
            "platforms": ["windows"]
        },
        {
            "name": "Bash",
            "cmd": ["bash", "-i", "-l"],
            "env": {},
            "enable": true,
            "default": false,
            "platforms": ["linux", "osx"]
        },
        {
            "name": "Zsh",
            "cmd": ["zsh", "-i", "-l"],
            "env": {},
            "enable": true,
            "default": false,
            "platforms": ["linux", "osx"]
        },
        {
            "name": "Ubuntu 18.04 Login Shell",
            "cmd": "C:\\Program Files\\WindowsApps\\CanonicalGroupLimited.UbuntuonWindows_1804.2019.521.0_x64__79rhkp1fndgsc\\ubuntu.exe",
            "env": {},
            "enable": true,
            "default": false,
            "platforms": ["windows"]
        }
    ],
}
```

### Customization of the SendCode package

I make sure Julia code is sent to a **Terminus** terminal. Select **Preferences/Package Settings/SendCode/Settings**. Paste the below code into this file.

```
{
    "prog": "terminus",

    "julia" : {
        "prog": "terminus",
        "bracketed_paste_mode": false
    }

}
```
There also needs to be a file `Packages\SendCode\support\Julia - Source File.sublime-build` with the command to "build" a Julia file by running it in the REPL.
```
{
    "target": "send_code_build",
    "cmd": "include(\"${file_name}\");",
    "selector": "source.julia"
}
```

### Command to start Julia REPL

The following functionality is the brainchild of Paul Soderlind (thanks!).

In order to be able to open a Julia REPL from a Julia source file currently opened in the editor, I define the following command binding in the file `USER\Default.sublime-commands`:
```
[
    {
        "caption": "Terminus: Open Julia Stable",
        "command": "terminus_open",
        "args"   : {//"shell_cmd": "%LOCALAPPDATA%/Programs/Julia/Julia-1.4.1/bin/julia.exe",
            "shell_cmd": "%LOCALAPPDATA%\\Programs\\Julia\\Julia-1.4.1\\bin\\julia.exe",
            "cwd": "${file_path:${folder}}",
            "title": "Julia REPL",
            "pre_window_hooks": [
                ["focus_group", {"group": 1}]
            ],
            "env": {"JULIA_NUM_THREADS":"4"},
        }
    },
    {
        "caption": "Terminus: Open Julia Nightly",
        "command": "terminus_open",
        "args"   : {
            "shell_cmd": "%LOCALAPPDATA%\\Programs\\\"Julia 1.5.0-DEV\"\\bin\\julia.exe",
            "cwd": "${file_path:${folder}}",
            "title": "Julia Nightly REPL",
            "pre_window_hooks": [
                ["focus_group", {"group": 1}]
            ],
            "env": {"JULIA_NUM_THREADS":"1"},
        }
    }
]
```
Note that the above assumes that the Julia executable, `julia`, is in the default location specified by the installer. Note that  we can have multiple  commands for different versions of Julia. So in the **Tools/Command Palette** either choose the command **Terminus: Open Julia 1.3** or the command **Terminus: Open Julia Nightly**. Note the magic incantation to allow for a space in the Julia-development path: the quoting `"%LOCALAPPDATA%\\Programs\\\"Julia 1.5.0-DEV\"\\bin\\julia.exe"` is needed to make it work.

Also, it is possible to set the environment variable directing the use of threading, `JULIA_NUM_THREADS`. Extending this to other environment variables is likely to be successful as well.

Finally, the editor gives focus to the top-most file in the group 1. This is useful when two columns or two rows are used for the layout. Opening Julia with the cursor in a source file then places the REPL in the other view.

### Snippets

The **Julia** mode package comes with predefined snippets (check out the [github site](https://github.com/JuliaEditorSupport/Julia-sublime)). I have defined some of my own, such as this one
to speed up the specification of a doc string (file `docstring.sublime-snippet`):
```
<snippet>
    <content><![CDATA[
"""
    ${1:bar(x[, y])}

${0:Compute}
"""
]]></content>
    <tabTrigger>docs</tabTrigger>
    <scope>source.julia</scope>
</snippet>
```
The snippets go into one file per snippet, which I put in `USER\snippets`.
The current selection of snippets is part of this repository in the `snippets` folder.

### Customization of Zeal

The Zeal executable needs to be revealed to the editor. Also, Zeal
needs to be made aware of the language of the documentation request:
```
{
  /**
   Zeal executable path. The instructions below require zeal
   to be located very precisely by its full path!?
   */
  "zeal_command": "C:\\Program Files\\Zeal\\zeal.exe",
  "mapping_sort" : true,

  // Language mapping.
  "language_mapping": {
    "Python": {"lang": "python", "zeal_lang": "python"},
    "Julia": {"lang": "julia", "zeal_lang": "julia"}
  }
}
```

## Usage

### Open terminal

Bring up the **Command Palette**, start typing in `Terminus`. Select **Terminus: List Shells**, and from the list that appears choose the shell you wish to start. Note that then you can select whether to start the shell in a panel at the bottom or in a separate view.

Julia may be started in the resulting terminal in the usual way. The terminals that I have checked out (`cmd` and the WSL Ubuntu shell) work as expected.

### Open a source file and then run Julia from the source file

Open a Julia source file, and in the **Command Palette**, start typing in `Terminus`. Select **Terminus: Open Julia**. This will open the default terminal on the user's platform (`cmd` on Windows, but see also the note about the Git bash below), and run Julia in the directory that contains the open file.


### Evaluating code

Select some Julia code and press `ctrl+enter`. The code will be pasted into the **Terminus** window and evaluated in Julia (assuming Julia was started in that **Terminus** window).
This also works for evaluating a line of code: place the cursor on a line and type `ctrl+enter`.

### Running Julia files

In a currently opened Julia source file,
press `ctrl+b` (which is a key binding for the menu action **Tools/Build**).
The current file will be evaluated in a Julia-running **Terminus** window with an `include()`.

### Asking for help in the REPL

There is a key binding to send code to the REPL to invoke the help mode on the selection using the `send_code` command (see the key bindings above).

### Accessing documentation via Zeal

Zeal can be brought up by commands specified through the **Command Palette**.
Unfortunately I haven't found a way of making the Zeal window appear within the editor. On the flipside, searching the documentation is lightning fast with Zeal.

## Additional useful packages

### Wrap Plus Package

Excellent for wrapping comments or markdown text [[link]](https://github.com/ehuss/Sublime-Wrap-Plus). It is capable of wrapping selections, which prevents annoyances such as wrapping a comment text in a doc string that pulls in the code of the function being documented and destroys the indentation of the code.

### Markdown Editing Package

Markdown plugin for Sublime Text called **MarkdownEditing**. Provides a color scheme with robust syntax highlighting and useful editing features.

### FileIcons package

Provides very nice file icons in the sidebar, seeing the contents of a folder at a glance is hence much easier.

### Open URL package

In case of an error, Julia prints 
the relevant source file name and line number. 
It is possible to jump directly to 
this file/line using this package, 
with a key binding  (`Ctrl-Alt-U`, by default).

### SideBarEnhancements

Installs enhancements for the sidebar. 

### TabNine  auto completer

Install `TabNine`. This is a fantastic auto-completer, based upon deep learning it can suggest things that other auto-completers cannot possibly match. Do not forget to turn off the default  auto-completion
```
"auto_complete": false,
```
Otherwise `TabNine`'s auto-completion will not be in effect.

## Additional configurations

### Using projects to control the color scheme

I like to use projects to control the color scheme of the windows in which the projects are open. I put `"settings"` into the project file
```
{
    "folders":
    [
        {
            "path": "."
        }
    ],
    "settings": {
         // "theme": "Asphalt.sublime-theme",
         // "color_scheme": "Monokai.sublime-color-scheme",
         "color_scheme": "Packages/Color Scheme - Dusk/Dusk.tmTheme",
         // "color_scheme": "Packages/Color Scheme - JustBeforeDawn/JustBeforeDawn.tmTheme",
         // "color_scheme": "Packages/Theme - Asphalt/Asphalt.tmTheme",
         // "color_scheme": "Packages/Theme - Flatland/Flatland Dark.tmTheme",
    }
}
```
where several of my favorite schemes are shown.

### Fonts

[`Envy Code R`](https://damieng.com/blog/2008/05/26/envy-code-r-preview-7-coding-font-released) is great. I also like `InputMonoCondensed` and `Fira Code`.

`JetBrains Mono` is a very well designed font, and code is pleasingly legible when rendered in this code.

To get the entire editor to display everything in the selected font (well, except the menu bar at the top), edit the file `Default.sublime-theme`. I put this in mine:
```
{
    // http://www.sublimetext.com/docs/3/themes.html
    "variables": {
        "font_face": "JetBrains Mono",//"Envy Code R",
        "font_size_sm": 10,
        "font_size": 12,
        "font_size_lg": 14,
    },
    "rules": [
    ],
}
```
Nice: now the sidebar looks the same as the text displayed in the editor.

## How to actually run Sublime Text

I really can't stand the default "shell" (CMD) in which Julia starts on
Windows. The interaction with the shell is quite unsatisfactory then: most of the Windows commands and all of the UNIX
commands don't work as one would expect. What I prefer instead is [Git Bash]
(https://git-scm.com/downloads). One can get that to run Julia by starting
Sublime Text  from a bat file. I create such a file with the line 
``` cmd /C start "" "%PROGRAMFILES%\\Git\\bin\\sh.exe" --login -i -c "exec \"C:\Users\PetrKrysl\Documents\Productivity\PortableSublimeText\sublime_text.exe\""
``` 
where my portable Sublime Text executable is invoked from within the Git
shell.

In order to get an executable file with the Sublime Text icon, I create a
shortcut (for instance to be placed on the desktop), and I give it the icon
which is part of this repository.

Alternatively, start the Git Bash terminal, and run the `subl` executable of
your portable Sublime Text. When a terminal is opened inside the running
editor, it runs the Git Bash. In particular, when Julia is started in the
editor, its shell mode drops you to the Bash. Perfect!

# How to make this sustainable

If the above looks like a lot of work, maybe it is. I do find it worthwhile,
though. Nevertheless, if one had to do this over and over again, I can see how
this could be found an onerous task. Fortunately, this can be fixed with the
portable version of Sublime Text: download and expand  the portable version
from [sublimetext.com](sublimetext.com) and tweak it according to the
instructions above. Or, use the preconfigured executables in this repository.
That's it: even when using a new computer or a new version of Sublime Text,
this configuration persists. There is no need to do this more than once.

