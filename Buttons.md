# Buttons (discord.py)

Buttons go inside a `ui.ActionRow` and use decorator syntax to assign callbacks — similar to how `@ui.button` works in a regular `ui.View`.

> Requires **discord.py 2.7+**

---

## Button Styles

```python
import discord

discord.ButtonStyle.primary    # blurple
discord.ButtonStyle.secondary  # grey
discord.ButtonStyle.success    # green
discord.ButtonStyle.danger     # red
discord.ButtonStyle.link       # grey with external link icon
```

> `ButtonStyle.link` opens a URL — it does **not** fire a callback. No `custom_id` needed, use `url=` instead.

---

## Basic Button in a CV2 Message

```python
import discord
from discord import ui

class MyLayout(discord.ui.LayoutView):
    row: discord.ui.ActionRow["MyLayout"] = discord.ui.ActionRow()

    @row.button(label="Click Me", style=discord.ButtonStyle.primary)
    async def my_button(self, interaction: discord.Interaction, button: discord.ui.Button):
        await interaction.response.send_message("You clicked it!", ephemeral=True)

@bot.tree.command()
async def hello(interaction: discord.Interaction):
    await interaction.response.send_message(view=MyLayout())
```

---

## Multiple Buttons in One Row

```python
class ConfirmLayout(discord.ui.LayoutView):
    row: discord.ui.ActionRow["ConfirmLayout"] = discord.ui.ActionRow()

    @row.button(label="Accept", style=discord.ButtonStyle.success, custom_id="accept")
    async def accept(self, interaction: discord.Interaction, button: discord.ui.Button):
        await interaction.response.send_message("Accepted!", ephemeral=True)

    @row.button(label="Decline", style=discord.ButtonStyle.danger, custom_id="decline")
    async def decline(self, interaction: discord.Interaction, button: discord.ui.Button):
        await interaction.response.send_message("Declined!", ephemeral=True)

    @row.button(label="Info", style=discord.ButtonStyle.secondary, custom_id="info")
    async def info(self, interaction: discord.Interaction, button: discord.ui.Button):
        await interaction.response.send_message("Here is more information.", ephemeral=True)
```

Up to **5 buttons** per row. For more, add another `ActionRow`.

---

## Disabled Button

```python
ui.Button(
    label="Unavailable",
    style=discord.ButtonStyle.secondary,
    custom_id="disabled_btn",
    disabled=True
)
```

---

## Link Button

```python
ui.Button(
    label="Support Server",
    style=discord.ButtonStyle.link,
    url="https://discord.gg/astria"
)
```

---

## Button with an Emoji

```python
ui.Button(
    label="Approve",
    style=discord.ButtonStyle.success,
    custom_id="approve",
    emoji="✅"
)

# Custom Discord emoji:
ui.Button(
    label="Boost",
    style=discord.ButtonStyle.primary,
    custom_id="boost",
    emoji=discord.PartialEmoji(name="boost", id=1234567890)
)
```

---

## Button as Section Accessory

A button can sit **inline next to text** inside a `ui.Section` without needing a full `ActionRow`:

```python
section = ui.Section(
    accessory=ui.Button(label="Open Ticket", style=discord.ButtonStyle.primary, custom_id="open_ticket")
)
section.add_item(ui.TextDisplay("Open a support ticket using the button ➡️"))

container.add_item(section)
```

---

## Updating the Message After a Click

```python
async def on_confirm(self, interaction: discord.Interaction, button: discord.ui.Button):
    new_container = ui.Container(accent_color=discord.Colour.green())
    new_container.add_item(ui.TextDisplay("✅ Confirmed!"))

    new_layout = ui.LayoutView()
    new_layout.add_item(new_container)

    await interaction.response.edit_message(view=new_layout)
```

---

## Persistent Buttons (Survive Bot Restarts)

For buttons that need to keep working after the bot restarts, set `custom_id`, use `timeout=None`, and register the view on startup:

```python
class PersistentLayout(discord.ui.LayoutView):
    def __init__(self):
        super().__init__(timeout=None)

    row: discord.ui.ActionRow["PersistentLayout"] = discord.ui.ActionRow()

    @row.button(label="Verify", style=discord.ButtonStyle.success, custom_id="verify_button")
    async def verify(self, interaction: discord.Interaction, button: discord.ui.Button):
        await interaction.response.send_message("You are verified!", ephemeral=True)

# In bot startup:
bot.add_view(PersistentLayout())
```

---

## Notes

- Each `ActionRow` holds up to **5 buttons**.
- `custom_id` is required on all buttons except `ButtonStyle.link`.
- `custom_id` can be up to **100 characters** — use it to store state (e.g. `ticket_close_42`).
- Buttons time out after **15 minutes** (Discord interaction limit) unless registered as persistent.
