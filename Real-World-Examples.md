# Real-World Examples

These are complete, working patterns taken directly from a production ticket bot.

---

## 1. Simple Ephemeral Notification

The most common pattern — send a short message back to the user:

```python
layout = ui.LayoutView()
container = ui.Container(accent_color=None)
container.add_item(ui.TextDisplay("You are blacklisted from creating tickets."))
layout.add_item(container)

await interaction.response.send_message(view=layout, ephemeral=True)
```

---

## 2. Info Container with Title + Body

The standard "embed replacement" pattern:

```python
layout = ui.LayoutView()
container = ui.Container(accent_color=None)
container.add_item(ui.TextDisplay("### Ticket Reopened"))
container.add_item(ui.Separator())
container.add_item(ui.TextDisplay(f"Reopened by {interaction.user.mention}"))
layout.add_item(container)

await interaction.response.send_message(view=layout)
```

---

## 3. Ticket Control Panel (Section + Buttons)

A full ticket panel with avatar thumbnail and action buttons:

```python
layout = ui.LayoutView(timeout=None)
container = ui.Container(accent_color=None)

container.add_item(ui.TextDisplay(f"# Support Ticket"))
container.add_item(ui.Separator())

section = ui.Section(accessory=ui.Thumbnail(media=user.display_avatar.url))
section.add_item(ui.TextDisplay(
    f"**Welcome** {user.mention}\n"
    f"**Category:** {category}\n\n"
    f"Our team will assist you shortly."
))
container.add_item(section)
container.add_item(ui.Separator())

row = ui.ActionRow(
    ui.Button(label="Claim", style=discord.ButtonStyle.primary, custom_id="ticket_claim_btn"),
    ui.Button(label="Close", style=discord.ButtonStyle.danger, custom_id="ticket_close_btn")
)
container.add_item(row)
layout.add_item(container)

await channel.send(view=layout)
```

---

## 4. Panel with Dropdown Select

A ticket panel using a dropdown to select a category:

```python
layout = ui.LayoutView(timeout=None)
container = ui.Container(accent_color=discord.Colour.blurple())

section = ui.Section(accessory=ui.Thumbnail(media=guild.icon.url))
section.add_item(ui.TextDisplay(f"**__{title}__**\n\n{description}"))
container.add_item(section)
container.add_item(ui.Separator())

options = [discord.SelectOption(label=name, value=name) for name, _ in categories]
row = ui.ActionRow(
    ui.Select(placeholder="Select a category...", options=options, custom_id="ticket_category_select")
)
container.add_item(row)
layout.add_item(container)

await channel.send(view=layout)
```

---

## 5. Log Message with File Attachment

Sending a log to a staff channel, including a transcript file:

```python
transcript_file.seek(0)
file = discord.File(transcript_file, filename=f"ticket-{ticket_number:04d}-transcript.txt")

layout = ui.LayoutView()
container = ui.Container(accent_color=None)
container.add_item(ui.TextDisplay("### Logs - Ticket Deleted"))
container.add_item(ui.Separator())
container.add_item(ui.TextDisplay(
    f"Ticket `#{ticket_number:04d}` deleted by {interaction.user.display_name}\n"
    f"**Category:** {category}"
))
container.add_item(ui.Separator())
container.add_item(ui.File(media=f"attachment://ticket-{ticket_number:04d}-transcript.txt"))
layout.add_item(container)

await log_channel.send(view=layout, file=file)
```

---

## 6. Avatar Display with MediaGallery

Showing a user's avatar inside a container:

```python
layout = ui.LayoutView()
container = ui.Container(accent_color=None)
container.add_item(ui.TextDisplay(f"### {user.display_name}'s Avatar"))
container.add_item(ui.Separator())
container.add_item(ui.MediaGallery(discord.ui.MediaGalleryItem(media=user.display_avatar.url)))
container.add_item(ui.Separator())
container.add_item(ui.TextDisplay(f"[PNG]({avatar_url}.png) | [JPG]({avatar_url}.jpg)"))
layout.add_item(container)

await interaction.response.send_message(view=layout, ephemeral=True)
```

---

## 7. Subclassing LayoutView

For complex persistent views, subclass `ui.LayoutView` directly:

```python
class TicketConfirmView(ui.LayoutView):
    def __init__(self, bot, category, user_id):
        super().__init__(timeout=180)

        container = ui.Container(accent_colour=discord.Colour.blue())
        container.add_item(ui.TextDisplay("### Support Rules"))
        container.add_item(ui.Separator())
        container.add_item(ui.TextDisplay("Please read the rules before opening a ticket."))

        row = ui.ActionRow(
            ui.Button(label="Open Ticket", style=discord.ButtonStyle.primary, custom_id="confirm_open"),
            ui.Button(label="Cancel", style=discord.ButtonStyle.danger, custom_id="cancel_ticket")
        )
        row.children[0].callback = self.confirm
        row.children[1].callback = self.cancel
        container.add_item(row)

        self.add_item(container)

    async def confirm(self, interaction):
        await interaction.response.send_modal(TicketModal(...))

    async def cancel(self, interaction):
        await interaction.response.edit_message(content="Cancelled.", view=None)
```
