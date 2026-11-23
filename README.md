# Veryfi Document to Markdown

Veryfi Document to Markdown converts PDFs, images, and Office files into structured Markdown that keeps headings, tables, and reading order. Attach a file, share a link, or use the integrated upload, and Claude sends it to Veryfi’s document parsing API and returns Markdown it can quote, summarize, and answer questions from. It handles invoices, receipts, contracts, and other documents up to 50 pages, and signs in with your Veryfi account, so no API keys are stored.

## What you can ask

- "Convert this PDF to markdown"
- "Parse the invoice I attached"
- "Extract the text from this scanned receipt"
- "Turn the contract at this link into markdown and tell me the termination clause"

Claude chooses the right way to send the document, converts it, and either shows the Markdown in the reply or saves a `.md` file named after the original. Tables, headings, and reading order are preserved, and the text is returned as Veryfi produced it, without rewriting.

## What's included

| Component | Purpose |
|---|---|
| `convert-to-markdown` skill | Instructions for picking an upload method, setting options, delivering results, and handling errors |
| Veryfi MCP server | Remote server at `https://api.veryfi.com/api/v8/partner/parse/mcp` exposing `convert_document` and `request_upload` |

## Setup

1. Install the plugin.
2. Connect the **veryfi** connector when prompted and sign in with your Veryfi account. No API keys are stored in the plugin.
3. Optional: if Claude runs in a sandbox with restricted network access, allow `api.veryfi.com` in your Claude network settings so Claude can upload attached files for you. Otherwise Claude gives you a one-time upload link.

## Gemini CLI

This repository is also a Gemini CLI extension.

```bash
gemini extensions install https://github.com/veryfi/document-to-markdown-claude
```

Start Gemini CLI and run `/mcp auth veryfi` to sign in with your Veryfi account in the browser. Then ask Gemini to convert a document, for example "convert ./invoice.pdf to markdown".

## Supported documents and limits

- PDF (including scanned), PNG, JPG, TIFF, HEIC and other common image formats, DOCX, XLSX, PPTX
- Up to 50 pages per conversion; longer documents can be split and converted in parts

## Data handling

- **What is sent:** only the documents you ask Claude to convert, plus optional labels (document type, your reference ID). Files go to Veryfi through the MCP server above, either by a public link you provide or a one-time upload link that expires after one hour.
- **Where it goes:** Veryfi's API at `api.veryfi.com`. No other third parties are contacted by this plugin.
- **What is returned:** the Markdown text, and page layout data only when you ask for it.
- **Retention and privacy:** governed by your Veryfi account settings and the [Veryfi Privacy Policy](https://www.veryfi.com/privacy).
- The plugin runs no local code, hooks, or scripts.

## Support

Questions or issues: support@veryfi.com · https://www.veryfi.com

## License

MIT, see [LICENSE](LICENSE).
