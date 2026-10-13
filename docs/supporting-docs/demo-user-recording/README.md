# User Hands-on Screen Recording Demo

This version uses four raw videos recorded by the user on 2026-10-10, showing real browser operation of tournament **#13 / agent fight test 4**. It is arranged as platform introduction → creation, registration, sponsorship → LLM match and replay → submission, review, prize claim → audit and historical refund → architecture explanation.

The narration is an English synthetic voice, with bilingual English and Chinese subtitles. The opening explains that the local environment responds quickly, making a complete demonstration easy; DUSD is a local demo token. The main content shows features and implementation, without a chapter on project shortcomings.

## Delivered Videos

- [Full demo MP4: English voice-over + English and Chinese subtitles](../../../deliverables/demo-video-user/Agent-Arena-Demo-EN-ZH.mp4), 10:00, 1920×1080, 30 fps, about 35.9 MB.
- [Version without burned-in subtitles](../../../deliverables/demo-video-user/Agent-Arena-Demo-clean.mp4), for easier re-editing later.
- [Bilingual SRT](../../../deliverables/demo-video-user/subtitles-bilingual.srt), [standalone voice-over](../../../deliverables/demo-video-user/narration.m4a), [chapter timeline](../../../deliverables/demo-video-user/chaptertimestamps.md).
- [Media validation record](VALIDATION.json): full decoding passed, 90 subtitles with no time overlaps, and key frames for sponsorship receipt, review and prize claim have been re-checked.

The media files above are stored on the local machine and excluded from Git; the script and production tools are under version control. The media links on this page are for local working copies into which the output files have been placed; the GitHub repository does not contain the MP4, voice-over or subtitle outputs, and the submitted media is provided separately with the course delivery package.

## Materials and Facts

| User's original clip | Content | Source duration |
| --- | --- | --- |
| `1赛事准备.mov` | Organiser creates, designates auditor, Red and Blue register, 100 DUSD sponsorship | 165.833 s |
| `2 开始对战.mov` | Start, real-model match, evidence, replay, submit result | 123.433 s |
| `3审计检查.mov` | Auditor signs in, views evidence, approves review | 67.850 s |
| `4 领取奖励.mov` | Blue side signs in, claims 100 DUSD, receipt shown | 36.000 s |

The UI for this tournament shows `qwen3.5-plus`, 15 rounds, 30 successful model calls and 0 failed calls, with the Blue side winning with 10 HP remaining. The prize-claim footage shows the Blue side's balance going from 300→400 DUSD, and this tournament's escrow from 100→0 DUSD. The corresponding UI was re-checked read-only during supplementary recording; this round of video production did not start another match, wallet transaction or model call.

The opening home page and Overview, and the closing Audit page, were recorded separately from the real `localhost:3002` pages. The historical refund shots view tournament #6's 25 DUSD deposit, cancellation and refund records; no refund is executed in the new video. That historical record comes from earlier CLI testing and is distinct from the user's hands-on wallet operation on #13.

## Editing Rules

- The original MOV files stay on the desktop, unchanged; output goes to `deliverables/demo-video-user/`, and intermediate files to `.local/user-demo-edit/`.
- The irrelevant Chrome tabs at the top, the macOS menu bar and the Dock at the bottom are cropped out; the platform and the transaction confirmation side panel are kept.
- Most of the account selection lists, repeated viewing and idle waiting are removed; the operations and success screens for creation, registration, approval, sponsorship, start, submission, review and prize claim are kept.
- Each clip keeps its internal chronological order. Operation footage plays at original speed; when the narration needs reading time, the last frame is extended; subtitles do not describe edited-out waits as on-chain time.
- The original screen-recording audio track is removed and replaced with a local macOS `Samantha` English synthetic voice-over. The complete raw material was not uploaded to any external voice-over or editing service.
- English/Chinese subtitles sit in a separate bottom area, avoiding covering buttons, the board, wallet amounts and results.
- The architecture cards are explanatory diagrams drawn from the current course edition source code; the actual stack is Next.js, Go/Gin, SQLite, Solidity and LLM, and no narrative about MySQL, Redis or RAG, which have not been implemented, is introduced.

## Reusable Files

- [Voice-over and editing manifest](NARRATION.en-zh.json): chapter durations, sentence-by-sentence English and Chinese text, source clip selections and key frame alignment points.
- [Bilingual reading script](SCRIPT.en-zh.md): convenient for reviewing the project and preparing the demo and defence.
- [Architecture narration source](ARCHITECTURE_DRAFT.json): verified architecture descriptions and their source-code basis.
- `tooling/render-demo-explainer-cards.py`: generates the seven explanatory diagrams.
- `tooling/build-demo-video.py`: generates and caches the sentence-by-sentence English voice-over.
- `tooling/build-user-recorded-demo.py`: combines the actual video, supplementary recordings, architecture diagrams, voice-over and subtitles.

## Reproduction Commands

First place the four original MOV files on the local desktop, keeping the names above, and keep the actual supplementary recording frames in `.local/user-demo-edit/intro/`. Then run from the repository root:

```sh
python3 tooling/render-demo-explainer-cards.py
python3 tooling/build-demo-video.py --prepare-audio --narration docs/supporting-docs/demo-user-recording/NARRATION.en-zh.json --output .local/user-demo-edit/voice --voice Samantha --rate 150 --silence 0.2
python3 tooling/build-user-recorded-demo.py --all
```

Generating the explanatory diagrams requires Pillow; the voice-over depends on macOS `say`, and editing depends on FFmpeg/FFprobe. The Pillow Python path on the current computer is provided by the workspace runtime; if the system Python does not have Pillow, run the first line with the configured runtime. `--plan` shows the timeline on its own, and `--audio` outputs only the voice-over and subtitles. Each step must wait for the previous one to finish before the next is run.

Videos and local caches do not go into Git; version control holds the scripts, narration, editing manifest and production records. The final media information and validation results are in the [media validation record](VALIDATION.json).
