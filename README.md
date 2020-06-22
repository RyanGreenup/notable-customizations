# Notable Customizations

This repository contains a collection of interesting community-created customizations and tips for Notable.

Add your own interesting customization to this collection by submiting a pull request or sharing it in our [chat](https://chat.notable.app).

## Commands

List of interesting command usages.

- _No customizations yet_

## Context keys

List of interesting context keys expressions.

- _No customizations yet_

## Settings

List of interesting settings configurations.

- _No customizations yet_

## Shortcuts

List of interesting custom shortcuts.

- _No customizations yet_

## Theming

List of interesting theming-related configurations.

- 3500+ different themes ported from VS Code are installable via the "Theme: Install..." command

## Other customizations

List of other interesting customizations, mainly involving third-party tools.

- [Autocomplete YAML tags from Vim](https://github.com/RyanGreenup/Note-Taking-Tools/blob/master/auto-complete-tags-vim/Auto-Complete-Tags.md)
- [Synchronizing tags with TMSU](https://github.com/RyanGreenup/Note-Taking-Tools/blob/master/tags-to-TMSU/Import-Tags-to-TMSU.md)

## Maintenance etc.

### Quickly Import External Notes from clipboard

The following shell script can be used to import content from almost any source:

```bash
#!/bin/bash
## /bin/makeMarkdown.sh
command -v xclip >/dev/null 2>&1 || { echo >&2 "I require xclip but it's not installed. install it with sudo apt install xclip  Aborting."; exit 1; }
command -v pandoc >/dev/null 2>&1 || { echo >&2 "I require pandoc but it's not installed. install it with sudo apt install pandoc  Aborting."; exit 1; }

INPUT_FORMAT=$(pandoc --list-output-formats | fzf)
OUTPUT_FORMAT="markdown_github+tex_math_dollars --atx-headers"
x="xclip -selection clipboard"

$x -o | pandoc -f $INPUT_FORMAT -t $OUTPUT_FORMAT | $x
```

This works fairly well on Wikipedia pages with a lot of math, for example [this one](https://en.wikiversity.org/wiki/Introduction_to_Calculus/Limits), just copy the [source](https://en.wikiversity.org/w/index.php?title=Introduction_to_Calculus/Limits&action=edit) to the clipboard and run the script.

### Create Links

> This relies on [GNU CoreUtils](https://www.gnu.org/software/coreutils/)

If you want to create links between your notes in a semi interactive fashion, this script is quite handy, it prompts you to choose to notes and it will make a relative path between them, this is advantageous to `[[wiki-links]]` in that they will also work with a static site generator or on a mobile app.

```bash
#!/usr/bin/env bash

command -v fd >/dev/null 2>&1 || { echo >&2 "I require fd-find but it's not installed. install refer to https://github.com/sharkdp/fd ;  Aborting."; exit 1; }
command -v sk >/dev/null 2>&1 || { echo >&2 "I require sk but it's not installed. install refer to  https://github.com/lotabout/skim Aborting."; exit 1; }

Choose_Name_only_Preview () {
    cd ~/Notes/MD/notes
    fd \.md | sed s/^/basename\ / | bash | sk --preview "mdcat (fd {})" --bind pgup:preview-page-up,pgdn:preview-page-down
}

Choose_File () {
    cd ~/Notes/MD/notes

    fd \.md | sk --preview "mdcat {}" --bind pgup:preview-page-up,pgdn:preview-page-down
}

# Choose_Name_only_Preview
# Choose_File

SOURCE=$(realpath $(Choose_File))
TARGET=$(realpath $(Choose_File))

realpath --relative-to=$SOURCE $TARGET
```

This can also be easily modified to create a script to [fix a link in the clipboard](https://github.com/RyanGreenup/DotFiles/blob/master/Scripts/bin/fixLink.sh)
