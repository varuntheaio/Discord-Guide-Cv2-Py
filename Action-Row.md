# ActionRow

In CV2, `ui.ActionRow` is used to place **buttons and select menus** directly inside a container. This replaces the old way of adding items to a `discord.ui.View` — instead, you build the row manually and add it to a container.

---

## Buttons Inside a Container

```python
row = ui.ActionRow(
    ui.Button(label="Confirm", style=discord.ButtonStyle.success, custom_id="confirm"),
    ui.Button(label="Cancel", style=discord.ButtonStyle.danger, custom_id="cancel")
)

container.add_item(row)
```

---

## Assigning Callbacks

Since you're not using decorators, you attach callbacks manually:

```python
row = ui.ActionRow(
    ui.Button(label="Close Ticket", style=discord.ButtonStyle.danger, custom_id="close_btn")
)
row.children[0].callback = self.close_callback

container.add_item(row)
```

---

## Select Menu Inside a Container

```python
options = [
    discord.SelectOption(label="General", value="general"),
    discord.SelectOption(label="Billing", value="billing"),
]

row = ui.ActionRow(
    ui.Select(
        placeholder="Choose a category...",
        options=options,
        custom_id="category_select"
    )
)
row.children[0].callback = self.select_callback

container.add_item(row)
```

---

## Up to 5 Buttons Per Row

```python
buttons = [ui.Button(label=name, custom_id=f"btn_{name}") for name in categories[:5]]
row = ui.ActionRow(*buttons)
container.add_item(row)
```

For more than 5 buttons, use multiple `ActionRow`s:

```python
for i in range(0, len(buttons), 5):
    row = ui.ActionRow(*buttons[i:i+5])
    container.add_item(row)
```

---

## Notes

- Each `ActionRow` holds up to **5 buttons** or **1 select menu**.
- Unlike the old `View` system, you don't use `@discord.ui.button` decorators inside a `LayoutView`.
- `custom_id` is required for persistent buttons (ones that survive bot restarts).
