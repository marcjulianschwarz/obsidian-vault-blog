---
blog-title: Remove macOS Sequoia Screen Capture Notifications
blog-tags:
  - EN
  - macOS
blog-published: 2025-04-12
---
With a recent macOS Sequoia update, apps capturing your screen would trigger a new notification that shows next to the menubar. For apps like [AltTab](https://alt-tab-macos.netlify.app/) which take screenshots of your screen for their functionality, this results in a constant flood of notifications every time you use the app.

Apparently, this new *"feature"* was not on purpose. It should show a notification once every 30 days. But through some misconfiguration of the timestamps in the `ScreenCaptureApprovals.plist` file, the notification was not limited to 30 days.
To fix this, find the file with:

```bash
open $HOME/Library/Group\ Containers/group.com.apple.replayd
```

and move or delete the `ScreenCaptureApprovals.plist` file from the opened folder. Then directly restart your Mac. The macOS system will automatically rewrite the file with corrected timestamps and the screen capture notifications don't show up anymore.
