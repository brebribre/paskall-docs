# Uploading Media

Upload pictures and videos in **Media**, or straight into a playlist or a scene. Drag files in, or click to pick them. Each file can be up to 500 MB.

## What you can upload

| | Formats |
|---|---|
| **Pictures** | JPEG, PNG, WebP, HEIC and HEIF (iPhone photos), AVIF, TIFF, BMP |
| **Videos** | MP4, MOV (iPhone and Mac videos included) |

GIF isn't accepted for now. Screens would only show its first frame, not the animation.

## What Paskall does with a file

Every screen, from a cheap Android box to a smart TV browser, has to be able to show every file smoothly. So Paskall brings each upload into a form they all handle, right after it arrives:

- **Pictures** in HEIC, HEIF, AVIF, TIFF or BMP become JPEG. A picture with see-through areas, like a logo, becomes PNG instead, so it keeps them.
- **Pictures larger than 4K** are made 4K. No screen shows more, and a smaller file loads faster.
- **Photos taken sideways** are turned upright, so they show the same way on every screen.
- **Videos** become MP4 in H.264, the one format every screen plays smoothly. A MOV from an iPhone is converted the same way.
- **Files that already fit** are left exactly as they are. A JPEG, PNG, WebP or MP4 that meets all of the above isn't touched, so it loses no quality.

Only the converted file is kept. After conversion, the file in Media is the converted one. `IMG_0042.HEIC` becomes `IMG_0042.jpg`, and the original is not stored.

!!! note "Optimising…"
    While a file is being converted, Media shows it as **Optimising…**. Pictures take a second or two. A long or high-quality video can take a few minutes. You can already add it to a playlist, and screens pick up the converted file as soon as it's ready.

!!! tip "If a file can't be converted"
    Its page in Media says so, with the reason. The file is kept as you uploaded it, so nothing is lost.
