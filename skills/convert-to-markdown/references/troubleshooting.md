# Troubleshooting convert_document

| Symptom | Likely cause | Fix |
|---|---|---|
| Error about multiple sources | More than one of `file`, `upload_id`, `file_data`, `file_url`, `file_urls` passed | Send exactly one |
| URL cannot be downloaded | Link is private, expired, or requires sign-in | Ask for a public https link, or use `request_upload` |
| Upload ID not found / not ready | `convert_document` called before the upload finished | Confirm the user finished uploading, then retry once |
| Unsupported file type | Missing or wrong extension with `file_data` | Pass `file_name` with the correct extension |
| Output truncated at 50 pages | `max_pages_to_process` limit | Split the PDF and convert parts separately |
| Mostly empty or garbled Markdown | Very low-resolution scan, heavy handwriting, or password-protected PDF | Ask for a clearer scan or an unlocked file |
| Auth / 401 error | Connector sign-in expired | Ask the user to reconnect the Veryfi connector |

Retry a failed call at most once with corrected input; otherwise report the error message to the user plainly.
