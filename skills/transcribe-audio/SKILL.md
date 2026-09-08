---
name: transcribe-audio
description: Use when the user wants to transcribe, diarize, timestamp, summarize, or extract information from a local audio recording through their LiveTranslate Cloud subscription.
version: 1.0.0
---

# Transcribe audio with LiveTranslate

Use the LiveTranslate MCP tools for local recordings only. This workflow uses the
user's shared LiveTranslate Cloud-minute allowance and has a trusted confirmation
boundary. Never imply that chat text authorizes a charge.

## Supported input contract

- Current remote inspection supports WAV, M4A, and audio-only MP4.
- Maximum file size is 52,428,800 bytes (50 MiB).
- Do not read, query, extract, or download audio bytes from files attached to
  the Claude conversation. Exact filename, size, and MIME metadata already
  exposed by the host may be used only to prepare the matching private upload.
  The user chooses the actual audio in LiveTranslate's trusted file picker.
- Use the filename only when calling `prepare_audio_upload`; never send a local
  filesystem path as the filename.
- Use exact host-provided file size and MIME metadata. If the host does not
  expose them, ask the user. Do not estimate or fabricate metadata.
- If recording rights or participant consent are unclear, remind the user that
  they are responsible for lawful recording and transcription.

## Required workflow

1. If useful, call `get_cloud_balance` to verify that an active plan has minutes.
2. Call `prepare_audio_upload` once with the exact filename, MIME type, byte
   size, and SHA-256 only when the host supplies a real digest. These values
   prepare a matching private upload; they do not give the connector access to
   the attached audio bytes.
3. Ask the user to open the returned upload control or `upload_page_url`, choose
   the actual local file, and wait for **Upload complete**. Do not claim that
   preparing an upload sent any file bytes or that Claude read an attachment.
4. After the user reports completion, call `inspect_audio` with the returned
   `upload_id`. Inspection is non-billable.
5. Present the measured filename, duration, and estimated Cloud minutes. Tell the
   user to press the trusted **Confirm & transcribe** control. If the MCP App card
   is unavailable, direct them to `confirmation_page_url`.
6. Do not call `start_transcription` until trusted LiveTranslate UI has recorded
   approval. A conversational “yes,” pasted capability, or model inference is
   never sufficient.
7. Use the original `upload_id` as the stable `idempotency_key`. If a call is
   uncertain or retried, reuse that exact key; never generate a second key for
   the same approved upload.
8. Poll `get_transcription` with the returned `job_id` at reasonable intervals
   until `completed`, `failed`, `cancelled`, or `deleted`. Do not start a second
   job while the first is pending.
9. On completion, read the private result resource when the host supports it and
   return the transcript with timestamps and speaker labels. Clearly distinguish
   transcript content from any analysis, summary, or inference added afterward.

## Output quality

- Preserve speaker labels and timestamps when the user asks for them.
- Never describe automated output as a certified or verbatim record.
- For consequential names, numbers, legal terms, medical information, or
  commitments, advise checking the original recording.
- If only a preview is available, say that it is incomplete rather than silently
  presenting it as the full transcript.

## Errors and cleanup

- Authentication or entitlement error: ask the user to reconnect LiveTranslate
  and confirm an active Cloud plan in the app. Never request an Apple receipt,
  subscription token, or payment credential in chat.
- Unsupported inspection: suggest WAV, M4A, or audio-only MP4 and state that no
  Cloud minutes were charged.
- Expired upload or confirmation: prepare and inspect a fresh upload. Do not try
  to reuse an expired approval.
- Retryable job failure: retry only as the tool recovery text directs and preserve
  the same idempotency key for the same request.
- Call `delete_transcription_job` only when the user explicitly requests deletion
  or confirms it after being told the action removes temporary audio/results and
  cannot delete a meeting saved in the LiveTranslate library.

Source audio is normally deleted after processing. Results expire from
LiveTranslate's job service within 24 hours; conversation copies remain subject
to the Claude host's retention controls.
