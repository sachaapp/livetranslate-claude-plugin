# SameSense for Claude

**Different words. Same sense.**

Live captions, notes, no bot.

Turn a downloaded recording into a timestamped, speaker-aware transcript from
Claude. The plugin connects to SameSense's hosted MCP service and uses the same
Cloud-minute balance as the SameSense app on iPhone, iPad, and Mac.

You stay in control of every billable transcription: SameSense first
inspects the uploaded file and shows its measured duration and exact estimated
minute cost. Processing starts only after you approve that estimate in a
trusted SameSense card or first-party confirmation page. Saying “yes” in
chat is not approval.

## What it can do

- Privately upload and inspect one local recording.
- Transcribe WAV, M4A, or audio-only MP4 files up to 50 MB.
- Detect speakers and return timestamps with the transcript.
- Show the Cloud plan and minute balance shared with the Apple apps.
- Delete a temporary transcription job early when you ask.

The connector cannot read your SameSense meeting library, iCloud data,
Apple Account, payment details, or StoreKit receipt.

## Before you begin

You need:

1. SameSense on iPhone, iPad, or Mac.
2. An active SameSense Cloud trial or subscription with available minutes.
3. A Claude edition that supports plugins and remote MCP connections.

[Download SameSense on the App Store](https://apps.apple.com/app/id6782147492)

## Connect once

1. Install and enable the SameSense plugin in Claude.
2. Open Claude's MCP connections view (or run `/mcp`) and choose
   **SameSense → Connect**.
3. Claude opens SameSense's secure authorization page and shows a short,
   one-time code.
4. In the SameSense app, open **Settings → General → AI assistants**.
5. Enter the code. Review the connector origin, verification status, and the
   requested capabilities, then choose **Allow** only if they look right.
6. Return to Claude. The connection finishes automatically.

SameSense never asks you to paste an Apple receipt or subscription token
into Claude. If a page or prompt asks for one, stop and contact support.

## Transcribe a recording

Ask Claude to transcribe a recording, then choose the local file in
SameSense's private upload card:

> Transcribe this recording with speaker labels and timestamps using SameSense.

Or run `/transcribe-audio` and name the recording you intend to choose. Claude
will:

1. Prepare a private upload and give you an **Upload audio** control or secure
   first-party upload page.
2. Wait while you choose the actual local file and the page reports
   **Upload complete**.
3. Inspect the uploaded media without charging minutes.
4. Show the measured duration and estimated Cloud-minute cost.
5. Wait for you to press **Confirm & transcribe** in SameSense's trusted UI.
6. Start the job, follow its progress, and return the transcript when ready.

The plugin does not inspect, read, or download audio bytes from files attached
to the Claude conversation. It may use exact filename, size, and media-type
metadata already exposed by the host to prepare a matching upload; it must ask
instead of guessing when that metadata is unavailable. The actual audio
selection and upload happen in SameSense's trusted file picker. The
original local file is never modified.

## Example prompts

- “How many SameSense Cloud minutes do I have left?”
- “Transcribe this M4A, label the speakers, and list the decisions with timestamps.”
- “Transcribe this audio-only MP4, then make a concise set of meeting notes.”
- “Delete the temporary SameSense data for the transcription job we just used.”

## Privacy and data handling

The file is uploaded directly to temporary private SameSense storage; it is
not embedded as base64 in the MCP conversation. Source audio is deleted after
processing. Incomplete or abandoned uploads are deleted within 24 hours.
Completed result data is available for up to 24 hours so Claude can retrieve it,
then it is automatically deleted from SameSense's job service. A copy
returned into the conversation follows Anthropic's data controls and retention.

Cloud speech providers process the audio needed to produce the requested result.
SameSense does not use meeting content for advertising or to train its own
models. See [Privacy and data handling](PRIVACY.md) for the complete connector
summary and the published [SameSense Privacy Policy](https://livetranslate-legal.sacha1allard.workers.dev/privacy).

You are responsible for having permission to record and transcribe the audio.
Automated transcripts can contain errors; verify important details against the
recording.

## Disconnect or troubleshoot

- To revoke access, open **SameSense → Settings → General → AI assistants**
  and remove the connection. This does not cancel your Apple subscription.
- If tools do not appear, run `/mcp`, reconnect SameSense, and start a new
  Claude session after enabling the plugin.
- If inspection says the format is unsupported, convert the file to WAV, M4A,
  or audio-only MP4. No minutes are charged for a failed inspection.
- If the current entitlement does not allow transcription, SameSense stops
  without uploading more audio or using minutes.

Support: [SameSense Help](https://livetranslate-legal.sacha1allard.workers.dev/support).

Report a security issue privately to
the [SameSense support page](https://livetranslate-legal.sacha1allard.workers.dev/support). Do not include private
recordings, access tokens, Apple receipts, or account credentials in the first
message.

[Terms of Use](https://livetranslate-legal.sacha1allard.workers.dev/terms) ·
[Privacy Policy](https://livetranslate-legal.sacha1allard.workers.dev/privacy)

## Source license

Copyright © 2026 Sacha Allard. All rights reserved. This repository is public
only to support Claude Plugin Directory review and installation. It is not open
source, and no permission is granted to copy, modify, or redistribute its
contents except as required by the applicable platform terms.
