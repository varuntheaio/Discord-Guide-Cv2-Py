# LayoutView

`ui.LayoutView` is the **root** of every Components V2 message. You add all your components into it, then pass it as the `view=` argument when sending a message.

Think of it like the outer wrapper — nothing CV2-related works without it.

---

## Basic Usage

```python
from discord import ui

layout = ui.LayoutView()
# add components to layout...
await channel.send(view=layout)
```

---

## Sending Ephemerally

```python
await interaction.response.send_message(view=layout, ephemeral=True)
```

---

## With a Timeout

By default `LayoutView` has no timeout (persistent). You can set one:

```python
layout = ui.LayoutView(timeout=180)  # expires after 3 minutes
```

---

## How it Fits Together

```
LayoutView
└── Container
    ├── TextDisplay
    ├── Separator
    └── ActionRow
        └── Button
```

Everything lives inside `LayoutView`. You never pass `LayoutView` to `discord.ui.View` — it **is** the view.

---

## Class Attribute Style (Alternative)

Instead of calling `add_item()`, you can declare components directly as class attributes when subclassing:

```python
class MyLayout(discord.ui.LayoutView):
    text = discord.ui.TextDisplay("Hello, Components V2!")
    row  = discord.ui.ActionRow()
```

This is cleaner for static layouts that don't change at runtime.

---

## Finding Nested Items with `find_item()`

Every component has an `id` property (separate from `custom_id`). You can use it to locate and update a specific component inside a layout after it's already built:

```python
COUNTER_ID = 99

class CounterLayout(discord.ui.LayoutView):
    container = discord.ui.Container(
        discord.ui.TextDisplay("Count: 0", id=COUNTER_ID)
    )

# Later, in a button callback:
async def callback(self, interaction):
    display = self.view.find_item(COUNTER_ID)
    display.content = "Count: 1"
    await interaction.response.edit_message(view=self.view)
```

`find_item(id)` searches recursively through all nested components and returns the match.

---

## Notes

- CV2 messages (`LayoutView`) and old messages (`ui.View`) **can coexist in the same bot** — it's opt-in per message, not per bot.
- You **cannot** mix `LayoutView` with `ui.View` in the **same message**.
- `LayoutView` itself is invisible — the visual content comes from the components inside it.
- Use `timeout=None` for persistent messages that survive bot restarts.
- The total component limit across a single `LayoutView` is **40**, including nested ones.
