---
blog-title: Obsidian Raycast Extension
blog-published: 2022-01-06
blog-tags:
  - EN
  - Obsidian
  - Raycast
blog-subtitle: Second Brain at your fingertips
---
 
[Raycast](https://www.raycast.com) is a free to use productivity tool. It's basically a replacement for the default macOS Spotlight Search and offers similar functionalities like searching for files and applications.

Or to put it in Raycasts words:
> Raycast is a blazingly fast, totally extendable launcher. It lets you complete tasks, calculate, share common links, and much more.

The biggest difference is that it is extendable through extensions which you can install in raycasts own store. There you will find everything from a YouTube Launcher, Visual Studio Code Search, GitHub actions, home control and so much more. The store is growing everyday and new useful extensions are developed.

As I really like the concept of a search/launcher to be the main hub on my Mac and because I am a big fan and extensive user of the note taking app [Obsidian](https://obsidian.md), I decided to create my own [raycast extension for Obsidian.](https://www.raycast.com/marcjulian/obsidian)

Right now it consists of the following two commands but new features are being developed while you are reading this sentence.

## Search Note
With the command *Search Note* you can search for every note in your vault. 

![Search Note Command](/images/search_note_06_01_2022.jpg)

Several actions are available when you select a note. 
You can view the note using *Quick Look* to get a glance at the content or use *Open in Obsidian* to directly open it in Obsidian itself.
With *Append to note* you can add text to the note, this is especially useful to add entries to lists or tables.
The *Copy note content* is self-explanatory and *Paste note content* will paste the text into the last used application. 

### Preferences
In the commands settings you are able to exclude folders from the search. This is especially useful if you have lots of reference notes or some sort of "database" which you don't want to search through.
For Quick Look, copy and paste actions it might be a good idea to hide the YAML frontmatter and wikilinks as they might not be needed in other applications. Therefore a setting for hiding this content is available too. 
Lastly you can configure the primary action (enter key) to trigger *Quick Look* or *Open in Obsidian*.

## Open Vault
With the command *Open Vault* you can quickly open one of your vaults which you can specify in the preferences. This allows for quick switching between different vaults and removes the rather annoying way of using Obsidian's vault switcher.

![Open Vault Command](/images/open_vault.jpg)

## Contribute
If you want to contribute to this extension you can visit the [GitHub repository](https://github.com/marcjulianschwarz/obsidian-raycast) and open a new issue. 
I am open to implement feature requests and would welcome it if any bugs or wrong behavior is reported to me. 