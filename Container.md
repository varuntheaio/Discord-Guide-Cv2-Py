# Container

`ui.Container` is the main building block of a CV2 layout. It groups components together and optionally adds a colored left-side accent bar — similar to the color strip on a Discord embed.

---

## Basic Usage

```python
container = ui.Container()
container.add_item(ui.TextDisplay("Hello!"))

layout.add_item(container)
```

---

## With an Accent Color

```python
container = ui.Container(accent_color=discord.Colour.blurple())
```

You can use any `discord.Colour`:

```python
ui.Container(accent_color=discord.Colour.green())
ui.Container(accent_color=discord.Colour.red())
ui.Container(accent_color=discord.Colour.from_rgb(255, 165, 0))
```

Both `accent_color` and `accent_colour` are accepted (both spellings work).

---

## No Color (Default)

```python
container = ui.Container(accent_color=None)
```

Passing `None` renders the container with no accent bar — just a plain outlined box.

---

## Adding Multiple Items

```python
container = ui.Container(accent_color=discord.Colour.blue())
container.add_item(ui.TextDisplay("### Title"))
container.add_item(ui.Separator())
container.add_item(ui.TextDisplay("Some body text here."))
```

---

## Notes

- A `LayoutView` can hold **multiple** containers.
- Containers can hold: `TextDisplay`, `Separator`, `Section`, `MediaGallery`, `File`, `ActionRow`.
- Containers **cannot** be nested inside other containers.
