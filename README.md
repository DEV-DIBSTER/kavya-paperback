# Kavya ![Generic badge](https://img.shields.io/badge/version-1.3.6-green.svg)
Kavya, A [Kavita](https://www.kavitareader.com/) client extension, for [Paperback](https://paperback.moe/)

GitHub: [DEV-DIBSTER/kavya-paperback](https://github.com/DEV-DIBSTER/kavya-paperback)

## Installation source
You can install the Paperback extension source from the URL below, or tap the button to add it directly in the app.

[`https://DEV-DIBSTER.github.io/kavya-paperback`](https://DEV-DIBSTER.github.io/kavya-paperback)

[![Add to Paperback](https://img.shields.io/badge/Add%20to-Paperback-blue)](paperback://addRepo?displayName=DEV-DIBSTER%27s%20Extensions&url=https%3A%2F%2FDEV-DIBSTER.github.io%2Fkavya-paperback)

## Requirements
- 0.8 or newer version of Paperback
- Stable version of Kavita (Tested with [linuxserver/kavita](https://docs.linuxserver.io/images/docker-kavita))


## Setting up Page Size
Set up page size in kavya setting page, 20 for iOS and 40 for iPadOS (Default is set to 40)

## Limitations

- Each series you want to track have to be added to a collection and track list.
- Tracking only works when you read the comic in the viewer (does not work with mark as read).

## Unsupported Formats

Paperback's reader only handles image-based pages. The following content types will **not** work through this extension:

- **EPUB** — EPUBs are reflowable HTML/text documents that require a dedicated renderer. Kavita serves them through its own web-based reader, which cannot be replicated in Paperback's image viewer. Attempting to open an EPUB chapter will result in every page returning a 404 error.
- **Book/Novel libraries** — Libraries of type Book or Novel in Kavita are primarily EPUB-based and will have the same issue.

**What does work:**
- Manga and comics (CBZ, CBR, ZIP — image-based)
- PDFs (Kavita extracts each page as an image server-side)

To keep your home screen clean, enable **Exclude Book & Novel Type Libraries** in the Kavya source settings. This hides unsupported libraries entirely.