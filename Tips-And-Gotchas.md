# Tips & Gotchas

Common mistakes, limits, and things to know before you ship.

---

## Install the Right Version

```
git+https://github.com/Rapptz/discord.py
```

CV2 is **not** on PyPI yet. If you get `AttributeError: module 'discord.ui' has no attribute 'LayoutView'`, you're on the wrong version.

---

## Old `ui.View` Still Works

CV2 is **opt-in per message** — you don't have to migrate your whole bot. You can have some messages using `ui.LayoutView` and others using the old `ui.View` in the same bot. They don't interfere with each other.

---

## No Embeds, Content, Stickers, or Polls Alongside CV2

When sending a CV2 message, you **cannot** include any of the following in the same call:
- `embed=` / `embeds=`
- `content=`
- `stickers=`
- `poll=`

CV2 components **replace** all of that. Use `ui.TextDisplay` instead of content or embeds.

```python
# Wrong
await channel.send(embed=my_embed, view=layout)
await channel.send(content="Hello", view=layout)

# Right — CV2 handles it all
await channel.send(view=layout)
```

> **Editing tip:** If you want to edit a non-CV2 message into a CV2 one, clear `content` and `embeds` to `None` in the edit call.

---

## Containers Can't Be Nested

You cannot put a `Container` inside another `Container`. Keep your structure flat:

```
LayoutView
├── Container   ✅
└── Container   ✅
    └── Container  ❌  (not allowed)
```

---

## accent_color vs accent_colour

Both spellings are accepted — discord.py supports either:

```python
ui.Container(accent_color=discord.Colour.red())   # ✅
ui.Container(accent_colour=discord.Colour.red())  # ✅ also works
```

---

## Seek Your Buffer Before Sending

If you're working with an `io.StringIO` or `io.BytesIO` and you've already written to it, always seek back to the start before attaching it:

```python
buffer.seek(0)
file = discord.File(buffer, filename="transcript.txt")
```

Forgetting this sends an empty file.

---

## attachment:// Filename Must Match Exactly

```python
file = discord.File(buf, filename="report.txt")
container.add_item(ui.File(media="attachment://report.txt"))  # ✅

container.add_item(ui.File(media="attachment://Report.txt"))  # ❌ case sensitive
```

---

## Persistent Views Need custom_id

For buttons and selects that should survive a bot restart, always set `custom_id` and register the view on startup:

```python
bot.add_view(MyPersistentView())
```

Without this, buttons stop working after the bot goes offline and comes back.

---

## Callbacks on ActionRow Buttons

You can't use `@discord.ui.button` decorators inside a `LayoutView`. Assign callbacks manually:

```python
row = ui.ActionRow(
    ui.Button(label="Click me", custom_id="my_btn")
)
row.children[0].callback = self.my_function
```

---

## Ephemeral Messages Can't Be Edited by Others

If you `send_message(..., ephemeral=True)`, only the bot can edit/delete it. Other users can't see it at all.

---

## Component Limits

| Component | Limit |
|-----------|-------|
| **Total components per LayoutView** | **40 (including nested)** |
| Buttons per ActionRow | 5 |
| Select options | 25 |
| Images in MediaGallery | 10 |
| TextDisplay length | ~2000 chars per block |

The 40-component limit is the most important one to watch. Every `Container`, `TextDisplay`, `Separator`, `Section`, `ActionRow`, `Button`, etc. counts toward it — including ones nested inside containers.
