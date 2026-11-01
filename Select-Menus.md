# Select Menus (discord.py)

Select menus go inside a `ui.ActionRow` and use decorator syntax or direct instantiation to assign callbacks. One select menu per row.

> Requires **discord.py 2.7+**

---

## Types of Select Menus

| Class | What It Does |
|-------|-------------|
| `ui.Select` | Custom list of options you define |
| `ui.UserSelect` | Pick a user from the server |
| `ui.RoleSelect` | Pick a role |
| `ui.ChannelSelect` | Pick a channel |
| `ui.MentionableSelect` | Pick a user or role |

---

## String Select Menu

The most common dropdown type — you define the options yourself:

```python
import discord
from discord import ui

class MyLayout(discord.ui.LayoutView):
    container = discord.ui.Container(
        discord.ui.TextDisplay("Select a support category below.")
    )
    row: discord.ui.ActionRow["MyLayout"] = discord.ui.ActionRow()

    @row.select(
        placeholder="Choose a category...",
        options=[
            discord.SelectOption(label="General",   value="general",   description="General support"),
            discord.SelectOption(label="Billing",   value="billing",   description="Payment issues"),
            discord.SelectOption(label="Technical", value="technical", description="Technical help"),
        ]
    )
    async def category_select(self, interaction: discord.Interaction, select: discord.ui.Select):
        chosen = select.values[0]
        await interaction.response.send_message(f"You selected: **{chosen}**", ephemeral=True)
```

---

## Handling Multiple Selections

Allow users to select more than one item:

```python
@row.select(
    placeholder="Pick up to 3 roles",
    min_values=1,
    max_values=3,
    options=[
        discord.SelectOption(label="Announcements", value="ann"),
        discord.SelectOption(label="Updates",       value="upd"),
        discord.SelectOption(label="Events",        value="evt"),
    ]
)
async def role_picker(self, interaction: discord.Interaction, select: discord.ui.Select):
    selected_roles = ", ".join(select.values)
    await interaction.response.send_message(f"Selected: {selected_roles}", ephemeral=True)
```

---

## Option with Emoji and Default

```python
discord.SelectOption(
    label="Billing",
    value="billing",
    description="Payment and subscription issues",
    emoji="💳",
    default=False
)
```

---

## User Select Menu

Lets the user pick someone from the server — no options to manually define:

```python
class UserLayout(discord.ui.LayoutView):
    row: discord.ui.ActionRow["UserLayout"] = discord.ui.ActionRow()

    @row.user_select(placeholder="Pick a member...")
    async def on_user(self, interaction: discord.Interaction, select: discord.ui.UserSelect):
        user = select.values[0]
        await interaction.response.send_message(f"Selected user: {user.mention}", ephemeral=True)
```

---

## Role Select Menu

```python
class RoleLayout(discord.ui.LayoutView):
    row: discord.ui.ActionRow["RoleLayout"] = discord.ui.ActionRow()

    @row.role_select(placeholder="Pick a role...")
    async def on_role(self, interaction: discord.Interaction, select: discord.ui.RoleSelect):
        role = select.values[0]
        await interaction.response.send_message(f"Selected role: {role.name}", ephemeral=True)
```

---

## Channel Select Menu

```python
class ChannelLayout(discord.ui.LayoutView):
    row: discord.ui.ActionRow["ChannelLayout"] = discord.ui.ActionRow()

    @row.channel_select(
        placeholder="Pick a text channel...",
        channel_types=[discord.ChannelType.text]
    )
    async def on_channel(self, interaction: discord.Interaction, select: discord.ui.ChannelSelect):
        channel = select.values[0]
        await interaction.response.send_message(f"Selected channel: {channel.mention}", ephemeral=True)
```

---

## Disabled Select Menu

```python
row = ui.ActionRow(
    ui.Select(
        placeholder="Not available right now",
        disabled=True,
        options=[discord.SelectOption(label="Option", value="x")]
    )
)
```

---

## Updating the Message After a Selection

```python
async def category_select(self, interaction: discord.Interaction, select: discord.ui.Select):
    category = select.values[0]

    updated = ui.Container(accent_color=discord.Colour.blurple())
    updated.add_item(ui.TextDisplay(f"**Category:** {category}\n\nA staff member will be with you shortly."))

    new_layout = ui.LayoutView()
    new_layout.add_item(updated)

    await interaction.response.edit_message(view=new_layout)
```

---

## Notes

- Only **1 select menu** per `ActionRow`.
- Up to **25 options** in a `ui.Select`.
- `custom_id` can be up to **100 characters**.
- Use `select.values` to access the selected item(s).
