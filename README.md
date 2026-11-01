# Discord-Guide-Cv2-Py-

# Discord.py Components V2 — Beginner Friendly Guide

> Made by **Varq** • Support Server: [discord.gg/astria](https://discord.gg/astria)

Components V2 is Discord's new message layout system that gives you **full control** over how your bot messages look. It's more powerful and flexible than the traditional `embed` system.

---

## What's New in Components V2?

- Fully customizable layouts
- Rich text with full markdown support
- Clean UI with sections, images, and spacing
- Buttons & dropdowns work right inside the layout
- `content`, `embeds`, `stickers`, and `polls` **cannot** be used alongside CV2 components

---

## Basic Building Blocks

| Component | What It Does |
|-----------|-------------|
| `ui.LayoutView` | The root wrapper — every CV2 message starts here |
| `ui.Container` | Outer layout block with optional accent color |
| `ui.TextDisplay` | Renders text with full markdown support |
| `ui.Section` | Groups text side-by-side with a thumbnail or button |
| `ui.Separator` | Adds a divider line or spacing between components |
| `ui.MediaGallery` | Displays images in a grid |
| `ui.File` | Attaches and shows files inline |
| `ui.ActionRow` | Adds buttons or select menus inside a layout |

---

## Simple Example

```python
from discord import ui
import discord

layout = ui.LayoutView()

container = ui.Container(accent_color=discord.Colour.blurple())
container.add_item(ui.TextDisplay("# Welcome!\n**Click the button below.**"))
container.add_item(ui.ActionRow(
    ui.Button(label="Say Hi", style=discord.ButtonStyle.primary, custom_id="hello_btn")
))

layout.add_item(container)

await interaction.response.send_message(view=layout)
```

---

## Button Click Handling

```python
# Inside your LayoutView subclass
row = ui.ActionRow(ui.Button(label="Say Hi", custom_id="hello_btn"))
row.children[0].callback = self.on_hello
container.add_item(row)

async def on_hello(self, interaction: discord.Interaction):
    await interaction.response.send_message("Hi! 👋", ephemeral=True)
```

---

## Visual Layout Example (Dashboard Style)

```python
container = ui.Container(accent_color=discord.Colour.dark_gray())
container.add_item(ui.TextDisplay("# Server Stats"))
container.add_item(ui.Separator())

section = ui.Section(
    accessory=ui.Button(label="Refresh", style=discord.ButtonStyle.secondary, custom_id="refresh_stats")
)
section.add_item(ui.TextDisplay("**Online Members:** 1,234"))
section.add_item(ui.TextDisplay("**Messages Today:** 5,678"))
container.add_item(section)
```

---

## Showing Images

```python
gallery = ui.MediaGallery()
gallery.add_item(media="https://example.com/image1.png")
gallery.add_item(media="https://example.com/image2.png")

container.add_item(gallery)
```

---

## Migrating from Embeds

| Old Embed | New CV2 Equivalent |
|-----------|-------------------|
| `embed.title` | `ui.TextDisplay("# Title")` |
| `embed.description` | `ui.TextDisplay("some text")` |
| `embed.add_field()` | `ui.Section` with text displays |
| `embed.set_image()` | `ui.MediaGallery` |
| `embed.set_thumbnail()` | `ui.Section(accessory=ui.Thumbnail(...))` |
| `embed.color` | `ui.Container(accent_color=...)` |

---

## Important Tips

- The **total component limit** is **40 per message** (containers + everything inside them)
- Install discord.py **from GitHub** — the PyPI version does not have CV2:
  ```
  pip install git+https://github.com/Rapptz/discord.py
  ```
- Old `ui.View` still works — CV2 is opt-in per message, not a full bot migration
- Use `ui.Separator` for spacing and visual breaks
- Always test how your layout looks on mobile too

---

## Advanced: Pagination Example

```python
class PaginatedLayout(discord.ui.LayoutView):
    def __init__(self, items: list, page: int = 0):
        super().__init__(timeout=120)
        self.items = items
        self.page = page
        self._build()

    def _build(self):
        chunk = self.items[self.page * 5 : (self.page + 1) * 5]

        container = discord.ui.Container(accent_color=None)
        container.add_item(discord.ui.TextDisplay(f"# Page {self.page + 1}"))
        container.add_item(discord.ui.Separator())

        for item in chunk:
            container.add_item(discord.ui.TextDisplay(f"**{item['title']}**\n{item['desc']}"))

        buttons = []
        if self.page > 0:
            buttons.append(discord.ui.Button(label="Prev", custom_id="prev_page", style=discord.ButtonStyle.secondary))
        if (self.page + 1) * 5 < len(self.items):
            buttons.append(discord.ui.Button(label="Next", custom_id="next_page", style=discord.ButtonStyle.secondary))

        if buttons:
            container.add_item(discord.ui.ActionRow(*buttons))

        self.add_item(container)
```

---

## Table of Contents

| # | Guide | Description |
|---|-------|-------------|
| 1 | [LayoutView](Layout-View.md) | The root of every CV2 message |
| 2 | [Container](Container.md) | Grouping components with optional accent color |
| 3 | [TextDisplay](Text-Display.md) | Rendering markdown text |
| 4 | [Separator](Separator.md) | Visual dividers and spacing |
| 5 | [Section & Thumbnail](Section-And-Thumbnail.md) | Side-by-side text + image or button |
| 6 | [MediaGallery](Media-Gallery.md) | Image grids |
| 7 | [File Component](File-Component.md) | Inline file attachments |
| 8 | [ActionRow](Action-Row.md) | Buttons and selects inside CV2 |
| 9 | [Real-World Examples](Real-World-Examples.md) | Full patterns from a production ticket bot |
| 10 | [Tips & Gotchas](Tips-And-Gotchas.md) | Limits, mistakes, and best practices |
