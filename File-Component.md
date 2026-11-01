# File Component

`ui.File` lets you display a file attachment **inline** inside a CV2 layout. This is how you show a transcript, log file, or any uploaded file as part of a structured message.

---

## Basic Usage

You need two things:
1. A `discord.File` object (the actual file to upload)
2. A `ui.File` component referencing it via `attachment://filename`

```python
file = discord.File(file_path, filename="transcript.txt")

container.add_item(ui.File(media="attachment://transcript.txt"))

await channel.send(view=layout, file=file)
```

---

## From an In-Memory Buffer

```python
import io

buffer = io.StringIO("Hello, this is the file content.")
file = discord.File(buffer, filename="output.txt")

container.add_item(ui.File(media="attachment://output.txt"))

await channel.send(view=layout, file=file)
```

---

## Sending to a User's DMs

```python
await user.send(view=layout, file=file)
```

---

## Inside a Log Message

```python
container.add_item(ui.TextDisplay("### Ticket Transcript"))
container.add_item(ui.Separator())
container.add_item(ui.TextDisplay(f"Ticket closed by {user.mention}"))
container.add_item(ui.File(media="attachment://ticket-0042-transcript.txt"))
```

---

## Notes

- The filename in `attachment://` must **exactly match** the filename you passed to `discord.File`.
- Always seek your buffer back to position `0` before passing it if you've already written to it: `buffer.seek(0)`.
- You can only attach **one** file per `send()` call this way (Discord limitation for single file uploads).
