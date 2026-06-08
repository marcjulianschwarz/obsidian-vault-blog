---
blog-title: AirPlay Receiver Runs on Port 5000
blog-published: 2026-06-08
blog-tags:
  - EN
  - TIL
  - macOS
---
Today I learned that the AirPlay Receiver on macOS runs on port 5000. Killing the process listening on this port reloads the menubar, but the port gets instantly occupied again as the process restarts automatically.

Disabling the AirPlay Receiver in System Settings stops the process, and the port is clear again.

You can toggle it here:
```
System Settings → General → AirDrop & Handoff → AirPlay Receiver off
```
