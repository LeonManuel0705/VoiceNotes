# VoiceNotes

A voice to text note taking application for macOS that runs entirely offline. Speech
recognition, grammar correction, summarisation and speaker filtering all happen on the
machine. No recording ever leaves the device.

## The problem it was built for

Recording a lecture in a classroom gives you a file in which the teacher is one voice among
twenty. Generic transcription turns that into an unusable wall of overlapping speech.

VoiceNotes solves it with speaker verification. Record thirty seconds of the person you
actually want, and the filter transcribes only them. Everyone else in the room becomes
silence.

## What it does

**Transcription.** Live recording with real time transcription in German and English through
Whisper, with automatic language detection. Existing files work too: MP3, MP4, WAV, M4A,
FLAC, OGG and WebM, and video files can produce SRT subtitles.

**Speaker filtering.** A voice profile built with TitaNet, NVIDIA's speaker verification
model, chosen because it holds up in noisy environments where simpler embeddings do not.

**Custom vocabulary.** Whisper reliably mangles domain terms, names and abbreviations. You
teach it: enter how a word sounds and how it should be written, and the correction is applied
to every future transcription. "crisp er" becomes CRISPR.

**Post processing on device.** Grammar correction through MLX on Apple Silicon with
LanguageTool as the fallback, summarisation and topic detection across sixteen subject
categories through Qwen2.5 running locally, and automatic tagging from content.

**Organisation.** Nested folders, tags, full text search, editable transcripts, and export to
Word, Markdown or plain text, either per note or a whole folder at once.

**Adaptive learning.** Corrections feed back in, so the same mistake gets made less often.

## Requirements

macOS, ideally Apple Silicon, Python 3.9 or newer, and ffmpeg.

```bash
brew install ffmpeg
./setup_offline.sh    # first run only
./start.sh            # http://localhost:5050
```

First setup downloads roughly 2 GB of models: Whisper, TitaNet, LanguageTool language data
and the MLX model. That happens once, and afterwards the application never needs a network
connection again.

## Stack

| Component | Used for |
|:---|:---|
| Whisper | Speech recognition |
| TitaNet | Speaker verification and voice filtering |
| MLX with Qwen2.5 | Summarisation, topic detection, grammar, on Apple Silicon |
| LanguageTool | Offline grammar fallback |
| Flask and SocketIO | Real time web interface |
| SQLite | Notes, tags, vocabulary |

## Privacy

Everything is local. Recordings, transcripts and voice profiles live in
`~/Documents/voice-notes/`, the database is a plain SQLite file, and there is no server to
send anything to.

Detailed usage instructions and troubleshooting are in [USAGE.md](USAGE.md).

## License

MIT
