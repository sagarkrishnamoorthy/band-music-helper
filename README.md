# Band Music Helper 🎵

A tool for converting between sheet music and audio. Upload sheet music to hear it played back with different instruments, or upload audio to get sheet music notation.

## What It Does

The app has two main features:

**Sheet Music → Audio**
- Upload an image or PDF of sheet music
- Choose an instrument (piano, trombone, or trumpet)
- Get an MP3 file with the music played in that instrument sound

**Audio → Sheet Music**
- Upload an audio file (MP3, WAV, etc.)
- Get a PDF of the sheet music notation

## Why I Built This

I play trombone in my school band, and practicing outside rehearsals is harder than it should be. If I want to hear what a piece sounds like with my part, or if I want to practice along with a recording but need the sheet music, there wasn't a simple way to do both.

This tool makes practice more flexible. I can work on pieces when band directors aren't around and hear different parts to understand how they fit together.

## How It Works

**Sheet Music → Audio Pipeline:**
1. OMR (Optical Music Recognition) reads the notation from the image
2. Converts it to MIDI format
3. Applies the selected instrument sound
4. Generates the MP3 audio file using FluidSynth

Note: Works best with 1-2 staff scores. Full orchestra pieces with many staves can confuse the OMR.

**Audio → Sheet Music Pipeline:**
1. AMT (Automatic Music Transcription) using Spotify's Basic Pitch
2. Converts detected notes to MusicXML format
3. Renders professional sheet music as PDF using LilyPond

The transcription isn't perfect with complex harmonies or background noise, but it works well for single-instrument practice recordings.

## Tech Stack

**Backend:**
- FastAPI (Python web framework)
- Oemer for OMR
- Basic Pitch for AMT
- music21 for format conversions
- FluidSynth for audio synthesis
- LilyPond for PDF rendering

**Frontend:**
- React with TypeScript
- Simple UI for file upload and conversion

## Project Structure

```
band-music-helper/
├── backend/
│   ├── main.py              # FastAPI application
│   ├── src/
│   │   ├── omr/             # Optical Music Recognition
│   │   ├── amt/             # Audio transcription
│   │   ├── processing/      # Format conversions
│   │   └── pipeline/        # Processing workflows
│   ├── uploads/             # Temporary file storage
│   ├── outputs/             # Generated files
│   └── logs/                # Application logs
└── frontend/
    └── src/
        ├── components/      # React UI components
        └── services/        # API client
```

## Getting Started

See [QUICKSTART.md](QUICKSTART.md) for setup instructions.

Requirements:
- Python 3.11+
- Node.js 18+
- System dependencies: FluidSynth, LilyPond, FFmpeg

## What I Learned

Building this taught me about making infrastructure decisions. Things like:
- Handling file uploads and processing status
- Making sure temporary files get cleaned up properly
- Giving useful error messages when something goes wrong
- Managing different processing pipelines that hand off data to each other

Each part (OMR, conversion, synthesis, transcription) has to work on its own, but they also need to pass data cleanly between steps. If one step fails, you need to catch it early so you're not wasting time on the next steps.

## Current Status

This is a working tool that handles the conversions I need for band practice. The core pipelines are reliable, but error handling could be better for edge cases like unusual file formats or very complex scores.

The bidirectional conversion (sheet ↔ audio) was the interesting challenge—making both pipelines work correctly and handling failures gracefully.

## Contributing

If you find bugs or have suggestions, feel free to open an issue.

## License

MIT License
