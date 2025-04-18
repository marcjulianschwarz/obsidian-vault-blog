---
blog-title: Debugger Nest.js App
blog-tags:
  - EN
  - TypeScript
  - TIL
blog-published: 2025-04-12
---
Today I learned that it is REALLY easy to attach a debugger to a NestJS app in VS Code. Run the following commands in the command palette:

- Debug: Toggle Auto Attach
- (Debug: Attach to Node Process)

In the terminal, run the NestJS app in the usual way, for example `yarn start:dev`. The debugger will now attach automatically.

No need to edit any `launch.json` files, awesome!