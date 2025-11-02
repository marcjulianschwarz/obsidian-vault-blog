# Today I learned ([TIL](https://www.marc-julian.de/...

Today I learned ([TIL](https://www.marc-julian.de/tags/TIL.html)) that you can reference regex groups in the *"Find and Replace"* search of VSCode.

For example, if you want to find a certain HTML tag to replace it with a different one, you could use the following regex expression in the search

```
<p>([.\W\w]*?)</p>
```

and use the captured group `$1` in the replacement like this:

```
<h1>$1</h1>
```

Ensure that you enabled regex syntax using the button in the search field on the right side.



---
Word Count: 80
Source: Manual
Added: 11/2/2025
