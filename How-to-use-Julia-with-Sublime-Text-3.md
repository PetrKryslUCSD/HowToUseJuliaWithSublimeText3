# How to use Julia with Sublime Text 3

## Rationale

This document was really written as a personal aide-memoire, which means it will be mostly limited to the ways in which I myself use the editor. Hence the discussion is limited to the programming of Julia and writing of markdown files. I do hope that other people may find it useful too, however. 

## Installation 

### Sublime Text 3

Download the editor from the website [https://www.sublimetext.com/](https://www.sublimetext.com/). (Isn't it nice that the download is only 8.8 MB?)
As I am on Windows 10, I will describe the installation and use of the editor on that platform. I do have the Windows Subsystem for Linux installed
and I use it regularly, so I will describe use of Julia with Linux from within the editor running on Windows.

### Install Package Control

The first thing you would want to do is install the package **Package Control**.
Use the menu item **Tools/Command Palette** (key binding: `ctrl+shift+P`), type `install`, and an item **Install Package Control** will bubble up. Select it, and practically instantaneously you will get a confirmation box that the package was installed.

By the way, remember the key binding `ctrl+shift+P`: it will bring up the **Command Palette**, and practically everything that you might need can be done from here.

### Install Julia syntax highlighting

**Package Control** can now be used to install other useful packages.
Bring up the **Command Palette**, and type in a few letters of **Package Control: Install Package**. When it comes up, select it. Loading the registry will take a couple of seconds, and then we get window where we can type in `Julia`. A button named **Julia** (with the subtitle "Julia syntax highlighting for Sublime Text 2/3"; refer to [https://github.com/JuliaEditorSupport/Julia-sublime](https://github.com/JuliaEditorSupport/Julia-sublime)) will come up highlighted. Click on it, and the package will be installed. At this point one should be able to open a Julia source file and get it highlighted based upon the syntax of Julia.

### Install the Terminus and SendCode packages

Repeat the procedure of package installation from **Package Control** to install the terminal emulation package **Terminus** (https://github.com/randy3k/Terminus), and the package to enable communication between a source window and the terminal, **SendCode** (https://github.com/randy3k/SendCode).

### Install the Zeal package (off-line access the documentation)

**Package Control** can also be used to install **Zeal for Sublime Text 2/3**.
Furthermore the executable of Zeal is required, and can be found at
[https://zealdocs.org/download.html](https://zealdocs.org/download.html).
Finally, the dock set for Julia needs to be downloaded.

## Customization

The key bindings file and the other customization files for the user live in the folder for user settings. In my case that is `C:\Users\PetrKrysl\AppData\Roaming\Sublime Text 3\Packages\User`. For brevity I will call this folder `USER`.

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
        "keys": ["ctrl+enter"], "command": "send_code",
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
    }
]
```
The key bindings may seem Baroque, but I use voice recognition which means I say a command, which sends these keys. (The point is to avoid typing these contortions on the keyboard.)

The key bindings for access to the current file name and path are provided by a little plug-in:
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
    // the shell marked as "default" will the default shell
    "shell_configs": [
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
            "name": "Command Prompt",
            "cmd": "cmd.exe",
            "env": {},
            "enable": true,
            "default": false,
            "platforms": ["windows"]
        },
        {
            "name": "Power Shell",
            "cmd": "powershell.exe",
            "env": {},
            "enable": true,
            "default": false,
            "platforms": ["windows"]
        },
        {
            "name": "Ubuntu 18.04 Login Shell",
            "cmd": "C:\\Users\\PetrKrysl\\Ubuntu_18.04TLS\\ubuntu1804.exe",
            "env": {},
            "enable": true,
            "default": true,
            "platforms": ["windows"]
        }
    ],
}
```

### Customization of the SendCode package

I make sure Julia code is sent to a **Terminus** terminal.

```
{
    "prog": "terminus", 

    "julia" : {
        "prog": "terminus",
        "bracketed_paste_mode": true
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

In order to be able to open a Julia REPL from a Julia source file currently opened in the editor, I define the following command binding in the file `USER\Default.sublime-commands`: 
```
[
	// Command to open a Julia REPL from a currently-opened Julia source file. 
	// This command assumes that the Julia executable is in the path.
	// 	Courtesy of Paul Soderlind.
    {
        "caption": "Terminus: Open Julia",
        "command": "terminus_open",
        "args"   : {
            "cmd": ["julia"],
            "cwd": "${file_path:${folder}}",
            "title": "Julia REPL",
            "pre_window_hooks": [
                ["set_layout", {
                   "cols": [0.0, 0.6, 1.0],
                   "rows": [0.0, 1.0],
                   "cells": [[0, 0, 1, 1], [1, 0, 2, 1]]
                }],
                ["focus_group", {"group": 1}]
            ]        
        }
    }
]
```
Note that the above assumes that the Julia executable, `julia`, is somewhere in the path.
Alternatively, spell out the full path to the executable. I suppose having multiple such commands for different versions of Julia would also make sense.

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

Open a Julia source file, and in the **Command Palette**, start typing in `Terminus`. Select **Terminus: Open Julia**. This will open the default terminal on the user's platform (`cmd` on Windows), and run Julia in the directory that contains the open file.

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