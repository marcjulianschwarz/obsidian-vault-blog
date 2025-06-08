---
blog-title: Obsidian Raycast Extension - Update
blog-published: 2022-03-06
blog-tags:
  - EN
  - Obsidian
  - Raycast
blog-subtitle: Notes, notes, notes ...
---

My Raycast extension "Obsidian" has been in the [Raycast Store](https://www.raycast.com/marcjulian/obsidian) for some months now and lots of people are already using it. In this post I would like to introduce some of the new features that I added.

## Create Note 

With the new command *Create Note* you can now create new notes on the fly. Like the other commands you can access it system-wide which makes it a great tool for jotting down some notes while working in another application.

![Create Note Command](/images/create_note.jpg)

The form accepts a name, local path (the command will automatically create a folder structure when the path doesn't exist), a customizable tag picker and a big content field.

### Customize the tag picker 

Use the Racast Search to find the extensions preferences by searching for *Extensions*. Scroll down to *Obsidian* and select the *Create Note* command. You will see a list of settings pop up on the right side. In the tags section you can enter a comma separated list of tags. For example: "daily, blog, project, todo, read".

### Other settings 

Entering a default path and tag will auto-fill the corresponding fields when running the command. This might be helpful when you always want to add notes to a certain folder or always tag them with the same tag.

## New keyboard shortcuts for Search Note 

The new update comes with two new keyboard shortcuts for the *Search Note* command. You can now use `opt + l` to copy a markdown link for the note to your clipboard or `opt + u` to copy the notes [Obsidian URI](https://help.obsidian.md/Advanced+topics/Using+obsidian+URI).

## Feature Requests
If you know of a feature that's still missing or want to contribute otherwise, make sure to visit the projects [GitHub repo](https://github.com/marcjulianschwarz/obsidian-raycast).