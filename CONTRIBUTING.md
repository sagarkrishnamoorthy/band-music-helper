# Contributing

Thanks for checking out this project. I built this for band practice, but I'm glad if others find it helpful.

## Reporting Issues

If something doesn't work:
- Describe what you were trying to convert
- Include error messages from logs
- Mention your OS and Python/Node versions
- Note the file format you were using

## Suggesting Features

Open an issue describing:
- What problem the feature would solve
- How you think it should work

I might not get to everything right away, but I appreciate the suggestions.

## Code Contributions

If you want to contribute:

1. Fork the repository
2. Create a feature branch
3. Test your changes with different file types
4. Submit a pull request explaining what changed

**Code style:**
- Python: Follow PEP 8 (I use Black)
- TypeScript: Follow the ESLint config
- Add comments for non-obvious processing steps

**Commit messages:**
```
type: brief description

Examples:
fix: handle PDF files with multiple pages correctly
feat: add violin to instrument options
docs: update OMR limitations
```

## Development Setup

See [QUICKSTART.md](QUICKSTART.md) for local setup.

Requirements:
- Python 3.11+
- Node.js 18+
- System tools: FluidSynth, LilyPond, FFmpeg

## Areas for Improvement

Things that could be better:

**High Priority:**
- Better error handling for unusual audio formats
- Progress indicators for long conversions
- More comprehensive testing

**Medium Priority:**
- Support for additional instruments
- Batch processing multiple files
- Better handling of multi-staff scores

**Lower Priority:**
- UI design improvements
- Performance optimizations for large files
- Additional output format options

## Questions

Feel free to open a discussion or issue. I'm in school and on the band, so it might take a day or two to respond.

## License

MIT License - contributions will be under the same license.
