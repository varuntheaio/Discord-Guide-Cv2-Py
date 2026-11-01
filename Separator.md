# Separator

`ui.Separator` adds a horizontal dividing line between components inside a container. It's the CV2 equivalent of a visual break — great for splitting a title from body content, or separating sections.

---

## Basic Usage

```python
container.add_item(ui.TextDisplay("### Title"))
container.add_item(ui.Separator())
container.add_item(ui.TextDisplay("Body content here."))
```

---

## Spacing Variants

You can control the size of the gap around the separator:

```python
container.add_item(ui.Separator(spacing=discord.SeparatorSpacing.small))
container.add_item(ui.Separator(spacing=discord.SeparatorSpacing.large))
```

---

## Invisible Separator (Spacer)

If you want spacing without the visible line:

```python
container.add_item(ui.Separator(divider=False))
```

This adds whitespace between components without drawing a line.

---

## Common Pattern

```python
container.add_item(ui.TextDisplay("### Ticket Closed"))
container.add_item(ui.Separator())
container.add_item(ui.TextDisplay(f"Closed by {user.mention}"))
container.add_item(ui.Separator())
container.add_item(ui.TextDisplay("A transcript has been sent to your DMs."))
```

---

## Notes

- Separators are purely visual — they don't carry any data.
- You can use as many separators as you like in a container.
