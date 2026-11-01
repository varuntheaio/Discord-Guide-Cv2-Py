# MediaGallery

`ui.MediaGallery` displays one or more images in a grid inside a container. It's the CV2 way to show images — replacing what you'd normally do with `embed.set_image()` or attachment previews.

---

## Single Image

```python
gallery = ui.MediaGallery()
gallery.add_item(media="https://example.com/banner.png")

container.add_item(gallery)
```

---

## Multiple Images (Grid)

```python
gallery = ui.MediaGallery()
gallery.add_item(media="https://example.com/image1.png")
gallery.add_item(media="https://example.com/image2.png")
gallery.add_item(media="https://example.com/image3.png")

container.add_item(gallery)
```

Multiple images render as a tiled grid in Discord.

---

## Using a MediaGalleryItem Directly

```python
item = discord.ui.MediaGalleryItem(media="https://example.com/avatar.png")
gallery = ui.MediaGallery(item)

container.add_item(gallery)
```

---

## User Avatar Example

```python
gallery = ui.MediaGallery()
gallery.add_item(media=user.display_avatar.url)

container.add_item(gallery)
```

---

## Placement Pattern

Galleries are usually placed after a separator for clean visual separation:

```python
container.add_item(ui.Separator())
gallery = ui.MediaGallery()
gallery.add_item(media=image_url)
container.add_item(gallery)
```

---

## Notes

- Only URL strings are accepted — no local file paths directly.
- To show a locally uploaded file, use `attachment://filename.png` and pass the file via `discord.File`.
- Up to 10 images per gallery.
