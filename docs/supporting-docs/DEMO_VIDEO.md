# Assignment Demo Video

For the final course deliverable, see the [ten-minute hands-on video and bilingual script](demo-user-recording/README.md) and its [validation record](demo-user-recording/VALIDATION.json). This page keeps the production method of the earlier narrated version for learning and remake reference; it is not the entry point for the final submitted video.

## Re-recording Direction (confirmed by the user on 2026-10-10)

The 033 local functional acceptance added real-model tournament #5 (qwen3.5-plus, 14 rounds, 28 successful calls, 100 DUSD prize claimed) and refund tournament #6 (25 DUSD). See the [test report](../TEST_REPORT.md); this acceptance was signed with CLI test accounts, and no new live-operation footage has been produced yet, so the current replay must not be described as a match newly started during recording.

The 9-minute-56-second version below is kept as a narration reference. The user has asked that the next version focus on newly performed local business operations: creating a tournament, sponsoring, registering, a real LLM match, result review and prize claiming, and cancellation/refund. Complete the real functional acceptance first, then re-record; the existence of a model configuration, old match replays and mocked tests cannot be treated as successful real calls.

The new video continues to use English voice-over with English and Chinese subtitles. About 7 minutes shows actual operations, about 2 minutes explains the architecture and key technologies, and the remaining time covers issues and limitations. If model waits or local review time are cut or fast-forwarded, this must be labelled on screen; a real draw must not be given a fake winner. Every shot corresponds to a saved tournament ID, transaction receipt or model call record. CLI test signatures and MetaMask UI signatures must be explained separately.

The user has accepted the data-reset and cold-start limitations of the Render free demo environment. The following corresponding narration should be added when re-recording (use the future tense while the public deployment has not actually happened, and do not claim in advance that it is online):

> The main demonstration runs locally. The planned public demo uses Render's free tier, which may sleep when idle and reset its local database after a restart or redeployment. This is a coursework demo, and its long-term availability has not been validated.

> The main features are demonstrated locally. The planned public demo uses a Render free instance, which may sleep when idle, and its local database may be reset after a restart or redeployment. This is a course demo environment, and its long-term availability has not been validated.

Once the deployment actually succeeds, record additional footage of accessing the public URL and change "planned public demo" to "public demo". This section is a re-recording requirement, not yet a completion record for the new video.

## Existing Narrated Version

The main viewing file is [demo-captioned.mp4](../../deliverables/demo-video/demo-captioned.mp4): English voice-over with burned-in bilingual English and Chinese subtitles. Watching it full screen in a local player is recommended; before submitting to the course system, check its format, size and upload rules.

The finished video is **596.400 seconds (9 min 56 s), 1920 × 1080, H.264 + AAC**, and the subtitled version is about **30.3 MB**. The English voice-over uses the macOS Samantha synthetic voice with a rate parameter of 145. Full decoding passed; the 50 subtitle groups correspond one-to-one with the voice-over timeline, and the audio/video duration difference is 8 milliseconds. Frames and bilingual subtitle layout were spot-checked at 14 key points in time. See the [media validation record](evidence/demo-video-verification-2026-10-10.json) and the [cover image](evidence/demo-video-cover-2026-10-10.png).

## Files and Learning Materials

| File | Purpose |
| --- | --- |
| [demo-captioned.mp4](../../deliverables/demo-video/demo-captioned.mp4) | Main video, English voice-over, English and Chinese subtitles displayed directly in the picture |
| [demo-clean.mp4](../../deliverables/demo-video/demo-clean.mp4) | Same picture and voice-over without burned-in subtitles, for easier re-editing |
| [audio.m4a](../../deliverables/demo-video/audio.m4a) / [narration.wav](../../deliverables/demo-video/audio/narration.wav) | Complete English synthetic voice-over |
| [Bilingual SRT](../../deliverables/demo-video/subtitles-bilingual.srt), [English SRT](../../deliverables/demo-video/subtitles-en.srt), [Chinese SRT](../../deliverables/demo-video/subtitles-zh.srt), [ASS](../../deliverables/demo-video/subtitles.ass) | External or re-typeset subtitles; the main video already has burned-in subtitles and does not need them overlaid again |
| [Bilingual script](demo/SCRIPT.en-zh.md) / [Narration JSON](demo/NARRATION.en-zh.json) | Final 10 chapters and 50 corresponding text groups, traceable and reusable |
| [Explanation cards HTML](demo/EXPLANATION_CARDS.html) | Architecture and validation explanations, viewed via #architecture / #validation respectively |
| [Generation script](../../tooling/build-demo-video.py) | Local voice-over, timeline, subtitle and video composition |

Media, audio caches and UI screenshots are not committed to Git as source; the learning text and explanation cards remain in docs. A clean clone needs to obtain the media separately or re-capture the UI; the original screenshots cannot be restored from the narration JSON alone.

## Chapter Timing

The following is the voice-over timeline, in minutes:seconds.milliseconds. The final video may have encoding-level duration differences.

| Chapter | Time | Content |
| --- | --- | --- |
| 01 | 00:00.000–01:00.717 | Purpose and local demo boundaries |
| 02 | 01:00.717–02:00.876 | Users, roles and guide |
| 03 | 02:00.876–02:54.116 | Wallet entry and creation permissions |
| 04 | 02:54.116–03:52.348 | Registration and Agent definitions |
| 05 | 03:52.348–04:47.097 | Replay of an existing match |
| 06 | 04:47.097–05:41.219 | Evidence and hash verification |
| 07 | 05:41.219–06:40.380 | Review and prize claim records |
| 08 | 06:40.380–07:42.745 | Cancellation and sponsor refunds |
| 09 | 07:42.745–08:50.195 | Architecture and test evidence |
| 10 | 08:50.195–09:56.392 | Bilingual experience, limitations and closing |

## Footage and Content Boundaries

- The footage comes from sequences of actual UI screenshots of the current local platform (captured at 5 fps), edited by chapter. Chapters shorter than their narration hold the last frame; this is an edited walkthrough, not a continuous ten-minute screen recording of new transactions. The generator outputs 30 fps by default, which does not turn a 5 fps capture into a genuine 30 fps operation record.
- The platform uses the English UI; existing tournament names, strategies, descriptions and raw evidence from users are kept in their original language. The voice-over is an English synthetic voice, not a recording of the user; the subtitles are corresponding bilingual English and Chinese text.
- This session only views existing local Anvil records and adds no new MetaMask login signatures, registrations, creations or transfers. The wallet connection dialog is only shown and then closed; the creation entry point shows the permission notice. The existing local integration tests are signed by viem using public test accounts, not by live operation of the MetaMask extension.
- The displayed snapshot is 4 tournaments, cumulative sponsorship of 325 DUSD, prizes paid 100, refunds 225, escrow balance 0. #1 is a completed 12-round deterministic simulation match; #3 has a confirmed refund of 200 DUSD; the dispute/ruling/25 DUSD refund of #2 is a prepared contract dispute fixture, not a real customer complaint.
- The chapter 9 architecture and validation cards are project explanations, not platform transaction pages. The 49 contract tests and the backend race/vet checks etc. are historical validation records; the 27 frontend tests come from the bilingual English/Chinese refactor. The video did not re-run these tests, and it does not constitute an external security audit.
- Local DUSD carries no promise of real-world asset value. This round did not complete a public cloud deployment, new course Sepolia validation, real LLM evaluation, full manual MetaMask acceptance or real stakeholder validation; matching hashes cannot prove that a match was genuine or fair. The course report should continue to use the actual records in the [test report](../TEST_REPORT.md) and the [requirements evidence boundaries](../REQUIREMENTS.md).

## Generation and Reuse

The generator's --help has been viewed and executed; it only composes input materials and does not operate a browser or capture footage automatically. It requires the Python 3 standard library, macOS say, ffmpeg and ffprobe, and burning in subtitles requires ffmpeg with subtitles/libass support. The commands below are run from the repository root, and the build updates media files of the same names in the specified output directory.

First place the real screenshots for each chapter in .local/demo-video/captures/01 to 10. Each chapter should ideally provide a manifest.json recording the real relative time and image path, for example:

~~~json
{
  "duration": 12.8,
  "frames": [
    { "file": "frames001.png", "t": 0 },
    { "file": "frames002.png", "t": 0.2 }
  ]
}
~~~

Frame paths are relative to the chapter directory; numbered images without a manifest have their times inferred from --capture-fps. The current capture materials are working files from this machine and this round; when reusing them, first check the content, identities, amounts and chapter completeness, and do not pass off old material as a new deployment.

~~~bash
python3 tooling/build-demo-video.py --help

python3 tooling/build-demo-video.py --prepare-audio \
  --narration docs/supporting-docs/demo/NARRATION.en-zh.json \
  --output deliverables/demo-video \
  --voice Samantha --rate 145 --silence 0.25

python3 tooling/build-demo-video.py --render \
  --captures .local/demo-video/captures \
  --output deliverables/demo-video \
  --capture-fps 5 --fps 30
~~~

--prepare-audio and --render can also be combined. After changing only the subtitles or part of the narration, you can add --reuse-chapters to --render: the generator checks the screenshot manifest, modification times, resolution, frame rate and target duration before reusing unchanged chapters; when inputs do not match it still re-encodes, and the final audio track and subtitles are always re-composed. --ffmpeg, --ffprobe and --say can specify paths to existing executables. If the speech tool produces empty files, investigate the machine's audio file access permissions; do not treat empty audio as success. After changing the English text, speech rate or voice, prepare the audio again, then check shots and subtitle timing against the new timed-narration.json.

The current explanation cards are a standalone 1280 × 720 HTML page, which can be opened in a browser as EXPLANATION_CARDS.html#architecture or #validation. It is used to explain modules and validation boundaries and does not call wallet or fund interfaces. Full decoding, subtitle and timeline checks, and key-frame spot checks have been completed; these media validations do not replace real multi-role wallet operations, cloud deployment or user evaluation.
