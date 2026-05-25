---
blog-title: Speeding through Git
blog-published: 2025-09-03
blog-tags:
  - EN
  - Git
---
The best tool for speeding up git workflows (imo) is the [oh-my-zsh git plugin](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/git).

Doing something like the following feels like magic:

```bash
gsw main
gl
gsw feature/my-feature
gst
gstu
grb main
gpf
gstp
```

- switching to main branch
- pulling newest changes on main 
- switching back to feature branch
- checking status of my files 
- stashing all changes (including untracked)
- rebasing feature branch onto main
- force pushing the rebased branch to the remote
- popping the stashed changes to continue working

All that in about 10 seconds because the commands get into muscle memory fast. Could not live without it anymore.
