---
blog-title: Obsidian Raycast Extension - Update 1.4 and 1.5
blog-published: 2022-05-05
blog-tags:
  - EN
  - Obsidian
  - Raycast
blog-subtitle: Check out the new look of the Search Note command
---

Since my last post, I released version 1.4 and 1.5 for the [Obsidian Raycast Extension](https://www.raycast.com/marcjulian/obsidian). These are the new features:

## Daily Note command

The new *Daily Note* command allows users to open or create a daily note. Select a vault, press enter and start typing. It requires the Advanced URI plugin for Obsidian.

## Pinned Notes command 

Use the *Pinned Notes* command to show a list of your pinned notes. You can use the same actions as with the *Search Note* command. This makes it easy to open, edit or view an important note.

## New actions and new look for Search Note command

### Actions 

With the *Append Selected Text to Note* action you are now able to append selected text to a note. Use the keyboard shortcut `opt + s` to trigger it.

You can now pin and unpin notes using the *Pin Note* and *Unpin Note* actions. Pinned notes will appear in the *Pinned Notes* command.

### New Look
Enabeling *Detail View* in preferences will show the content of a note on the right side.

![Search Note](/images/search_note.jpg)


## New Preferences
### Append Prefix

_Append Prefix_ is a new preference that appends a given prefix to text. This makes it  easy to maintain bullet or checkbox lists.

### Open Note on Creation 

If turned on this setting will open a note created with the *Create Note* command after creation.

### Hide LaTeX

You can now hide LaTeX from Quick Look, Detail View and all copy/paste commands.

### Default Note Name 

If the note name in the *Create Note* command is empty, this default note name replaces it.

### Folder Actions 

You can now provide a list of folders (paths) to the *Create Note* command. These folders will automatically create keyboard shortcuts (actions) to create notes in a specific folder. This is helpful if you are using [Obsidians Templater Plugin](https://silentvoid13.github.io/Templater/introduction.html).

## Store page update 

The store page got a new look with images, category and version history.

![Store Images](/images/store_images.jpg)
![Store Changelog](/images/store_changelog.jpg)

## Other 

Commands with a single vault will now trigger without prior vault selection.

## Feature Requests

If you know of a feature that's still missing or want to contribute otherwise, make sure to visit the projects [GitHub repo](https://github.com/marcjulianschwarz/obsidian-raycast). With these two updates I restructured the entire codebase which should make it a lot easier to contribute.