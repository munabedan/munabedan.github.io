---

title: Enhancing Caja Capabilities Using Bash Scripting
date: 2023-21-26 17:00
categories: [Development, Scripting]
tags: [development, scripting, bash]
image:
    path: /images/screenshot-caja-scripts.png
---

### Introduction

If you're a Mate desktop user, Caja serves as your primary file manager. Today, let's delve into something exciting—expanding the functionalities of Caja using Bash scripting. Additionally, these methods can easily apply to Nautilus with just a few adjustments, refer to [Ubuntu's Nautilus Scripts How-to](https://help.ubuntu.com/community/NautilusScriptsHowto)
.

### What You Need to Know

Caja scripting enables the execution of scripts, usually simpler in operation compared to extensions, in any scripted language supported by your system. To leverage this capability:

1. Installing File Manager Scripts:
   - Save these script files in `~/.config/caja/scripts/`.
   - To install a script, copy it to the script folder and grant it user executable permissions.
   - To access the scripts folder, go to File ▸ Scripts ▸ Open Scripts Folder.

2. Running Installed Scripts:
   - Execute a script via File ▸ Scripts and select the script from the submenu.
   - Access scripts conveniently from the context menu.

### Writing File Manager Scripts

Before diving in, acquaint yourself with the available variables that our scripts can leverage:

- Environment variables set by Caja:
  - `CAJA_SCRIPT_SELECTED_FILE_PATHS` newline-delimited paths for selected files (only if local)
  - `CAJA_SCRIPT_SELECTED_URIS` newline-delimited URIs for selected files
  - `CAJA_SCRIPT_CURRENT_URI` URI for current location
  - `CAJA_SCRIPT_WINDOW_GEOMETRY` position and size of current window
  - `CAJA_SCRIPT_NEXT_PANE_SELECTED_FILE_PATHS` newline-delimited paths for selected files in the inactive pane of a split-view window (only if local)
  - `CAJA_SCRIPT_NEXT_PANE_SELECTED_URIS` newline-delimited URIs for selected files in the inactive pane of a split-view window
  - `CAJA_SCRIPT_NEXT_PANE_CURRENT_URI` URI for current location in the inactive pane of a split-view window

Bash scripting offers a straightforward method to create scripts, although more intricate tasks may necessitate other languages like Perl or Python.

### The Demo: Hide Files Script

Let's put theory into practice! Imagine a script that hides files without appending the pesky '.' at the beginning of their names. This script leverages a nifty Caja feature—creating a `.hidden` file within the selected directory.

This functionality proves immensely handy for organizing and decluttering your file system effortlessly.

Here's the bash code:

```bash
# Get the URI of the selected folder
selected_folder_uri="$CAJA_SCRIPT_SELECTED_FILE_PATHS"

# URI for the .hidden file
hidden_file_uri="$CAJA_SCRIPT_CURRENT_URI"

# Check if the .hidden file exists
if [ ! -f "$hidden_file_uri/.hidden" ]; then
    # Create the .hidden file
    touch "$hidden_file_uri/.hidden"
fi

# Loop through the selected files and add them to the .hidden file
for file in $selected_folder_uri; do
    basename "$file" >> "$hidden_file_uri/.hidden"
done

# Use Zenity to notify the user that the files are now hidden
notification="\n $selected_folder_uri \n\n Please reload to hide the files."

zenity --info --title="Reload required" --text="$notification"
```

To implement this:

1. Save the code in a file named "Hide Files."
2. Make it executable using `chmod +x`.
3. Place it in the Caja scripts folder at `~/.config/caja/scripts/`.

By clicking the reload icon, the script will be executed, hiding the specified files.

And voilà! You now have the ability to customize and streamline your file management experience.