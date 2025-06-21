---
blog-title: Obsidian Dataview Serializer
blog-tags:
  - EN
  - Obsidian
  - Markdown
blog-author: Marc Julian Schwarz
blog-published: 2025-06-13
---

The popular [Obsidian Dataview plugin](https://github.com/blacksmithgu/obsidian-dataview) let's you query markdown notes with SQL-like syntax. It uses the YAML frontmatter (also known as Obsidian properties) as attributes. This enables powerful workflows. For example, [all of my blog posts](https://github.com/marcjulianschwarz/obsidian-vault-blog) contain these YAML properties:

- `blog-title`
- `blog-tags`
- `blog-published`
- `blog-author`
- `blog-archived`
- `blog-skip`

With these, I can create lists and tables about my blog posts. Here, the query results in a table of all posts that are online at the moment, sorted by publishing date:

```
TABLE blog-published
FROM "Posts"
WHERE blog-skip != true and blog-archive != true
SORT blog-published desc
```

But you can also build more complex queries. The following adds a markdown formatted link to the actual published blog post website as a new column:

```
TABLE WITHOUT ID ("[" + blog-title + "](" + "https://marc-julian.com/blog/posts/" + replace(file.name, ".md", "") + ")") AS "URL", blog-published as "Published" FROM "Posts" 
WHERE blog-skip != true and blog-archive != true 
SORT blog-published desc
```

Now these tables created with dataview are great inside of the Obsidian editor, but they won't render in other markdown editors/viewers (e.g. GitHub READMEs). [The Obsidian Dataview Serializer](https://github.com/dsebastien/obsidian-dataview-serializer) solves this specific problem by live-updating a *"serialized"* markdown version of the table. Every time the underlying data or the query changes, the plugin will update the markdown table accordingly. Check out [this README of my blog post repository](https://github.com/marcjulianschwarz/obsidian-vault-blog) on GitHub to see it in action. It uses the second more complex query to provide links to all posts right in the README. There you should even be able to see the exact post you are reading right now.

What I like about this approach is that it works in all places that can render markdown tables. For example, I am also using the plugin to render a table of differn


