# IdleGolem: devlog

IdleGolem keeps Minecraft accounts online and busy while their owners are away. It runs bots
on a Mac or Windows desktop app or on an Android phone, and you control everything from a
dashboard. This repo is the public build log: what changed, why, and the interesting problems
along the way. The source code is private.

**Latest: 1.2.0**, a top-down map of each bot's surroundings, paths you draw for anti-AFK
walking, and a spawner automation that collects items, sorts them into storage and sells
the rest every hour. See [CHANGELOG.md](CHANGELOG.md).

## What it does

- **Runs many bots at once**, each with its own server, login steps, schedule and proxy.
  Bots reconnect with backoff, rejoin the right game mode, and handle death and drowning.
- **Live dashboard**, the same on desktop and phone: chat, inventory with item icons, menus
  the server opens (clickable), stats, alerts to Discord, backup and restore.
- **Anti-AFK movement** that the server's movement checks accept: small steps, wandering near
  home, or a path you draw on the map. If a bot is kicked while walking, it holds still after
  rejoining until you turn movement back on.
- **Automation**: join, death and repeat steps, whisper auto-replies, a "player nearby" alert,
  and a spawner job that empties virtual spawners on a timer.
- **Accounts and plans**: sign in with Discord, plan limits (bots, devices, bot-hours a day),
  and admin commands right inside the Discord server.

## How it's built

| Part | What it is |
|---|---|
| Bot engine | Node.js on [mineflayer](https://github.com/PrismarineJS/mineflayer), one process for every bot |
| Dashboard | A single HTML file with no framework, served by the bot engine; responsive down to phone width |
| Desktop app | Electron for macOS (universal) and Windows; tray icon, splash screen, encrypted passwords |
| Android app | A real APK with a Node runtime inside it, a foreground service and a WebView dashboard |
| Accounts | Cloudflare Worker + D1 database, Discord OAuth and slash commands, Ed25519-signed plan passes |
| Downloads | Cloudflare R2, with hard caps that keep it inside the free tier |
| Release build | esbuild bundle, own code obfuscated, Minecraft data trimmed from ~500 MB to a few MB |

## Problems worth a story

- **Getting kicked for walking.** The server's anti-cheat flagged the bot's movement. The cause
  was the order of the movement packets the bot sent each tick compared with a real client.
  Fixing the order, and keeping walks short and in straight lines, stopped the kicks.
- **One app, three platforms.** The same engine runs on a desktop and inside an Android APK.
  That meant shipping a Node runtime in the APK and keeping the phone from killing it in the
  background (foreground service, battery-optimisation checks on every launch).
- **Seeing where a bot is.** A browser 3D view of the bot's world turned out too heavy for
  busy servers, so it's on hold. A top-down map built from the bot's own chunk data does the
  job instead: block colours from the game's textures, cave mode underground, and about 10 ms
  per scan. Click a block to walk there, or draw a path for anti-AFK.
- **Clicking menus that fight back.** Server menus are inventories with buttons. Taking items
  needed shift-clicks, because a plain click only works once per opening. Some clicks are
  dropped while the server is busy, so every menu step retries a few times.
- **Staying free.** File downloads, accounts and update notices all run on Cloudflare's free
  tiers, with limits built into the code so a busy month can't turn into a bill.

## Timeline

| Date | Milestone |
|---|---|
| 14 Nov 2026 | First bot manager, dashboard and in-game builder; desktop app |
| 15 Nov 2026 | Android app; phone layout; Discord accounts and plans |
| 16 Nov 2026 | Proxies, invite-only sign-in, update notices (1.1.0); movement, map, spawner job (1.2.0) |

Full details in [CHANGELOG.md](CHANGELOG.md).
