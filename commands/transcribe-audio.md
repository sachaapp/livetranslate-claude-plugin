---
name: transcribe-audio
description: Securely transcribe a local recording with LiveTranslate Cloud.
argument-hint: "[recording description]"
allowed-tools:
  - mcp__plugin_livetranslate_livetranslate__prepare_audio_upload
  - mcp__plugin_livetranslate_livetranslate__inspect_audio
  - mcp__plugin_livetranslate_livetranslate__start_transcription
  - mcp__plugin_livetranslate_livetranslate__get_transcription
  - mcp__plugin_livetranslate_livetranslate__get_cloud_balance
---

Help the user transcribe a local recording with LiveTranslate. Treat
`$ARGUMENTS` only as a description of the recording they intend to choose in
LiveTranslate's trusted file picker. Do not read, query, or download audio bytes
from a file attached to the Claude conversation. Exact host-exposed filename,
size, and media-type metadata may be used only to prepare the matching private
upload.

Follow the `transcribe-audio` skill exactly. In particular:

- Never guess the file's exact byte size or MIME type. Use exact metadata the
  host exposes, or ask the user when it is unavailable.
- Preparing an upload does not upload the file; guide the user through the
  returned private upload control.
- Inspect before starting and state the measured duration and estimated Cloud
  minutes.
- Wait for trusted LiveTranslate UI approval. Chat text is not approval.
- Use the upload ID as the stable idempotency key and reuse it on uncertain retry.
- Poll the same job to a terminal state, then return the timestamped,
  speaker-labelled transcript before doing any requested summary or analysis.
