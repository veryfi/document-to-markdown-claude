---
name: convert-to-markdown
description: This skill should be used when the user asks to "convert this to markdown", "parse this PDF", "turn this document into text", "extract the text from this scan/image", "OCR this file", or otherwise wants a PDF, image, or Office document (docx, xlsx, pptx) turned into Markdown. Uses the Veryfi convert_document connector.
---

# Convert a Document to Markdown

Turn PDFs, images (PNG, JPG, TIFF, HEIC, etc.), and Office files into Markdown with the Veryfi connector's `convert_document` tool.

## Before starting

Confirm the Veryfi document conversion connector is available (tools named `convert_document` and `request_upload`). If it is not, tell the user to sign in to the Veryfi server bundled with this plugin (or reconnect it in their connector settings), and stop. Do not fall back to another OCR method without saying so.

## Choose how to send the document

Pass **exactly one** source to `convert_document`. Pick the first that applies:

1. **File attached to the chat, and the host can pass attachments** → pass it as `file` (the host fills `download_url` and `file_id`; never copy bytes).
2. **Public https link** → `file_url`. For several links that form one document, `file_urls`.
3. **File attached but cannot be passed as `file`, or a file on the user's computer / in the workspace** → call `request_upload` with the `file_name`. Then either:
   - if the file is in a shell-accessible sandbox, try once to upload it as a multipart form field named `file`: `curl -sS -F "file=@<path>" <upload_url>`, or
   - if there is no shell, or the upload is refused (network blocked, 403, proxy error), give the user the upload link right away, note it expires in one hour, and ask them to reply once the file is uploaded. Do not try other workarounds.
   Once uploaded, call `convert_document` with the `upload_id`. Do not call `convert_document` until the upload is confirmed.
4. **Very small file only (a few KB)** → `file_data` as base64 plus `file_name` with its extension.

Never retype file bytes as base64 by hand, and never pass a local file path — the server cannot read it.

## Options

- `document_type`: set a label when known (`invoice`, `receipt`, `bank_statement`, `contract`, …). Default `document`.
- `max_pages_to_process`: 1–50 (default 50). For documents over 50 pages, tell the user only the first 50 are converted, or split the file and convert each part.
- `details: true`: only when the user needs layout / bounding boxes (e.g. to locate fields on a page). Otherwise leave off to keep output small.
- `external_id`: pass through if the user supplies a reference ID.

## Deliver the result

- **Short result** (roughly under 80 lines) and the user just wants to read it → show the Markdown in the reply.
- **Longer result, or the source came from a file** → save it as a `.md` file named after the source (`invoice-123.pdf` → `invoice-123.md`). If the source is in a folder the user connected, write the file next to it. Then give a 1–2 sentence summary (page count, document type, anything notable such as tables or low-quality pages).
- Keep the converted Markdown exactly as returned. Do not rewrite, summarize, or "clean up" its contents unless the user asks.
- If the user asked a question about the document (e.g. "what's the total?"), answer it from the Markdown after converting.

## Multiple files

Convert each file with its own `convert_document` call (calls can run in parallel) and save one `.md` per source, unless the files are pages of one document — then use `file_urls` or merge before converting.

## Errors

See `references/troubleshooting.md` for common failures and fixes.
