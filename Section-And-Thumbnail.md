# Section & Thumbnail

`ui.Section` is a layout component that places text **side-by-side** with an accessory (like a thumbnail image). It's useful for showing a user's avatar next to their info, or a server icon next to a title.

---

## Basic Usage

```python
section = ui.Section(accessory=ui.Thumbnail(media="https://example.com/image.png"))
section.add_item(ui.TextDisplay("**John Doe**\nSupport ticket opened."))

container.add_item(section)
```

---

## With a User's Avatar

```python
section = ui.Section(accessory=ui.Thumbnail(media=user.display_avatar.url))
section.add_item(ui.TextDisplay(f"**{user.display_name}**\nCategory: General"))

container.add_item(section)
```

---

## With a Server Icon

```python
icon_url = interaction.guild.icon.url if interaction.guild.icon else None

if icon_url:
    section = ui.Section(accessory=ui.Thumbnail(media=icon_url))
    section.add_item(ui.TextDisplay(f"**__{title}__**\n\n{description}"))
    container.add_item(section)
else:
    container.add_item(ui.TextDisplay(f"**__{title}__**\n\n{description}"))
```

---

## How it Renders

```
┌─────────────────────────────────────────┐
│  Text content here                 [img] │
│  More text on the left side              │
└─────────────────────────────────────────┘
```

The thumbnail appears on the **right**, text on the **left**.

---

## Button as Accessory

A `ui.Button` can also be used as a section accessory, placing it on the right side next to your text:

```python
section = ui.Section(accessory=ui.Button(label="+1", custom_id="increment_btn"))
section.add_item(ui.TextDisplay("The current count is 0.", id=COUNT_TRACKER_ID))

container.add_item(section)
```

This is useful for compact action + label combos without needing a full `ActionRow`.

---

## Notes

- `ui.Thumbnail` only accepts a URL string (no local files).
- You can add multiple `TextDisplay` items to a single section.
- The section itself goes inside a `Container`.
- The `accessory` can be a `ui.Thumbnail` **or** a `ui.Button` — both are supported.
