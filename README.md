# Paskall Docs

The guides for running screens on Paskall. Published at **https://brebribre.github.io/paskall-docs/** — it rebuilds itself about a minute after any change here lands on `main`.

## Editing a page (no tools needed)

1. Open the page on the site and click the **pencil** at the top right. (Or open the file under `docs/` here on GitHub and click the pencil.)
2. Change the text. The **Preview** tab shows how it will look.
3. Press **Commit changes** — leave the options as they are.

That's it. About a minute later the site shows your change. You need a free GitHub account and to be a collaborator on this repo — ask Bryan to add you.

## Writing tips

Pages are Markdown. The few things worth knowing:

- `# Title` once at the top; `## Section` and `### Sub-section` for headings.
- `1.` `2.` `3.` for numbered steps. To add a note under a step, indent the next line by four spaces.
- Commands go between triple backticks: ```` ```bash ```` … ```` ``` ````.
- A highlighted box:

  ```
  !!! note "Optional title"
      The text, indented by four spaces.
  ```

  Use `note`, `tip`, `info` or `warning`.

- A link to another guide: `[Connecting a Screen](connecting-a-screen.md)`.

## Adding a page

1. Add a new `.md` file in `docs/` (all lowercase, hyphens for spaces, e.g. `power-schedules.md`).
2. Add it to the `nav` list in `mkdocs.yml` so it appears in the sidebar.
3. Add a row for it on `docs/index.md`.

## Adding an image

Drag the image into the `docs/` folder on GitHub (or a `docs/images/` folder), then in the page write `![What it shows](images/the-file.png)`.

## If the site didn't update

Look at the **Actions** tab. A red run means the build failed — usually a link to a page that doesn't exist, or a page not listed in `nav`. The run's log says which.
