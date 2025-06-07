---
blog-skip: true
blog-archived: true
---

> [!TIP] Ideas
> ```dataview
> list
> from ""
> where blog-skip = true and blog-archived != true
> sort blog-published desc
> ```

> [!Check]- Online (Recent) 
> ```dataview
> table blog-published
> from ""
> where blog-skip != true and blog-archive != true  
> sort blog-published desc
> ```

> [!TIP]- Online (Archived)
> ```dataview
> table blog-published
> from ""
> where blog-skip != true and blog-archived = true
> sort blog-published desc
> ```

