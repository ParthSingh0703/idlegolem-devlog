# Changelog

Newest first. Versions are the desktop and Android app.

## 16 Nov 2026: 1.2.0 (in testing)

**Spawner automation.** Some servers have "virtual" spawners that keep their drops in a
storage menu instead of on the ground. The new spawner job empties them on a timer:
- Each rule names a spawner, the item to keep and a shulker box colour, for example iron golem
  → iron ingots → light gray box, every hour.
- The bot opens the spawner's storage, shift-clicks out only that item, puts it in the box,
  then presses Sell All so the spawner fills up again.
- It never walks. Everything is clicked from where the bot stands, and anything out of reach
  is reported instead of walked to, because walking can trip the server's movement checks.
- It remembers each spawner's and box's exact position. Some servers hide blocks from clients,
  and clicking the saved spot still opens them.
- Every menu step retries a few seconds later if the server drops the click. Each spawner has
  its own Run / Retry button, and its own timer that survives disconnects and restarts.

**Map and movement.**
- A top-down map of each bot's surroundings on the Move screen: terrain colours, caves when
  underground, nearby players and mobs, and the bot's heading. Click a block to walk there.
- Draw an anti-AFK path on the map, up to 16 points. The bot walks it and back every few seconds.
- If a bot is kicked while walking, it holds still after rejoining until you resume movement.

**Admin features from Discord.** The newest features show only for admins, decided by your
role in the Discord server. The license server checks roles hourly and signs the result into
each app's plan pass.

## 16 Nov 2026: 1.1.0

- **Update notices**: an admin runs `/release` in Discord. It posts in #announcements, and
  every older app shows an "Update available" bar with a download button within 5 minutes.
- **Downloads** from Cloudflare R2 through personal, short-lived links, capped to stay free.
- **Custom plans** for single users, and DMs when a plan changes or is about to end.
- **Invite-only sign-in**: only members of the Discord server can use the app. The sign-in page
  names the Discord account you used, so a wrong-account sign-in is easy to spot.
- **Per-bot SOCKS5 proxies**: a bot connects from the proxy's IP and never falls back to a
  direct connection. Includes a setup script for a free cloud VM proxy.

## 15 Nov 2026

- **Android app**: a standalone APK with its own Node runtime, the full dashboard in a WebView,
  and a foreground service so bots keep running with the screen off.
- **Phone layout**: side drawer for pages and bots, bottom-sheet dialogs, fitted tabs.
- **Accounts and plans**: Discord sign-in, plan limits (bots, devices, bot-hours a day),
  Discord slash commands for admins, all on a Cloudflare Worker with a D1 database.
- **Inventory**: real item icons, including 3D block icons.
- **Auto-reply** recognises the server's private-message format.

## 14 Nov 2026

- **First version**: a dashboard to run many bots, with chat, inventory, clickable server menus
  and an in-game builder.
- **Automation**: join, death and repeat steps; whisper auto-replies; a "player nearby" alert;
  schedules; Discord webhook alerts; backup and restore.
- **Desktop app** (Electron) for macOS and Windows: tray icon, splash screen, encrypted saved
  passwords. Release builds are bundled and obfuscated, and fail if anything slips out of the bundle.
