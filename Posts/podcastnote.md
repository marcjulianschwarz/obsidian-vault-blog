---
blog-title: Podcast Note plugin for Obsidian
blog-published: 2021-09-09
blog-tags:
  - EN
  - Obsidian
blog-archived: true
---

# Podcast Note

My plugin *Podcast Note* for the note taking app [Obsidian](https://obsidian.md) is a great way to write notes on podcasts. With a single URL you will get the title, image and description of your podcast.
Using a custom template you can style the note to your likings. I will explain more settings and features further down.

You can find it in the official community plugins list right in [Obsidian](https://obsidian.md) or on [GitHub](https://github.com/marcjulianschwarz/obsidian-podcast-note).

## How to use it
You can add new podcast notes by opening the command pallete (cmd + p) and searching for "Podcast Note" commands:
### Add Podcast Note
A prompt will open where you can enter the URL for the podcast you want to take notes on. 
Of course you can also specify a keyboard shortcut to trigger the prompt.

### Add Podcast Notes from selection

This command will be visible in editor mode. Make sure you have text selected which contains markdown links to podcast episodes. Running the command will create new podcast notes for every url in the selected text. It will also automatically link these notes.

### Supported Podcast services

+ Apple Podcast
+ Spotify Podcast
+ Google Podcast
+ Pocket Casts
+ Airr
+ Overcast
+ Castro
+ Castbox

## Demo

### Example Podcast Note:

![Podcast Note example](https://user-images.githubusercontent.com/67844154/131222181-e9a52afa-fee2-4eff-83e1-f03deb633df3.png)

## Settings
### 1. Template
Here you can specify how the metadata for your podcast notes looks like. 
Use these three placeholders:

+ **{{Title}}** → title of your podcast
+ **{{ImageURL}}** → image url of your podcast
+ **{{Description}}** → short podcast description
+ **{{PodcastURL}}** → url to podcast
+ **{{Date}}** → date (format: Day-Month-Year)
+ **{{Timestamp}}** → current timestamp

#### Example template:
```
---
tags: [Podcast]
date: {{Date}}
---
# {{Title}} 
![]({{ImageURL}})
## Description: 
> {{Description}}
-> [Podcast Link]({{PodcastURL}})

## Notes:
```

### 2. Filename template

Specify whether you want the plugin to insert the podcast note at your cursor or whether it should create a new note. You can also use a template for the filename.

Possible placeholders for this template are:
+ **{{Title}}** → title of your podcast
+ **{{Timestamp}}** → timestamp (like zettelkasten id)
+ **{{Date}}** → date (format: Day-Month-Year)

### 3. Folder

Set the folder where. The path is relative to your vault. For example **folder/podcast_folder** will become **path/to/vault/folder/podcastfolder**.

### 4. Insert podcast note at cursor
Specify whether you want to create a new note or whether you want the metadata to be inserted at your cursor.

