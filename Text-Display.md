# TextDisplay

`ui.TextDisplay` renders markdown text inside a CV2 layout. It's the primary way to show any written content — titles, descriptions, field-like info, etc.

---

## Basic Usage

```python
container.add_item(ui.TextDisplay("Hello, world!"))
```

---

## Markdown is Fully Supported

```python
container.add_item(ui.TextDisplay("### Ticket Opened"))
container.add_item(ui.TextDisplay("**Status:** Open"))
container.add_item(ui.TextDisplay(">>> This is a blockquote"))
container.add_item(ui.TextDisplay("`inline code` works too"))
```

---

## Multi-line Text

```python
container.add_item(ui.TextDisplay(
    "**User:** John\n"
    "**Category:** Support\n"
    "**Created:** <t:1700000000:R>"
))
```

---

## Discord Timestamps

Discord's timestamp formatting works inside `TextDisplay`:

```python
import discord

ts = int(discord.utils.utcnow().timestamp())

container.add_item(ui.TextDisplay(f"Opened <t:{ts}:R>"))   # e.g. "5 minutes ago"
container.add_item(ui.TextDisplay(f"Opened <t:{ts}:F>"))   # e.g. "Monday, 1 January 2024 12:00"
```

---

## User & Channel Mentions

```python
container.add_item(ui.TextDisplay(f"Created by {user.mention}"))
container.add_item(ui.TextDisplay(f"Post in {channel.mention}"))
```

---

## Notes

- You can have as many `TextDisplay` items as you want in a container.
- There's no title/description split like in embeds — just use markdown headings (`###`, `**bold**`, etc.).
- Emojis (both unicode and custom Discord ones) render fine.
