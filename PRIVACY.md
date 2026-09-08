# Privacy and data handling

This page summarizes the SameSense Claude connector. The published
[SameSense Privacy Policy](https://livetranslate-legal.sacha1allard.workers.dev/privacy)
and [Terms of Use](https://livetranslate-legal.sacha1allard.workers.dev/terms)
govern the service.

## Data the connector can access

The connection requests only these SameSense capabilities:

- read the current Cloud plan and minute balance;
- prepare and inspect a temporary audio upload;
- start and read a transcription after trusted user approval; and
- delete temporary transcription job data.

The connector does **not** receive the user's StoreKit receipt, Apple Account,
payment details, iCloud identity, SameSense account identifier, or meeting
library. OAuth access is short-lived, audience- and scope-bound, and backed by a
rotating revocable connection.

## Audio and transcript flow

1. The user chooses a local file in a SameSense upload surface.
2. File bytes go directly to private temporary storage operated on Cloudflare.
3. SameSense measures the media and shows the filename, duration, and exact
   estimated Cloud-minute charge before any billable processing.
4. Only the trusted SameSense card or first-party confirmation page can
   approve the estimate. A model message cannot approve it.
5. SameSense Cloud may send the audio needed for transcription and speaker
   detection to configured speech providers, including AssemblyAI, Google
   Gemini, or Cloudflare Workers AI. Provider routing may change for reliability.
6. The resulting transcript is returned to Claude. Claude and Anthropic then
   process and retain that conversation copy under Anthropic's terms and user
   controls.

SameSense does not sell the content, use it for advertising, or use it to
train its own models.

## Retention

- Source audio is deleted after the transcription result is safely stored or
  when processing ends in a terminal failure.
- Incomplete or abandoned uploads are automatically deleted within 24 hours.
- Transcription results remain retrievable for up to 24 hours, then are
  automatically deleted from the SameSense job service.
- Explicitly deleting a temporary job removes its temporary audio and result
  early when available.
- Metering and purchase records are retained as needed to provide paid
  allowances, prevent duplicate transactions, resolve disputes, and meet legal
  or accounting obligations.
- Revoking the connector prevents new SameSense requests but does not erase
  copies already returned into a Claude conversation. Use Anthropic's controls
  for those copies.

## User choices

- Review the exact estimated charge before approving each transcription.
- Revoke Claude from **SameSense → Settings → General → AI assistants**.
- Ask Claude to delete a temporary transcription job early.
- Use SameSense's native on-device import instead of this cloud connector
  when supported by the user's Apple device.

Questions or privacy requests: use the [SameSense support page](https://livetranslate-legal.sacha1allard.workers.dev/support).
