# Contributing to Band Music

Thank you for your interest in contributing to Band Music! This document provides guidelines and instructions for contributing to the project.

## Code of Conduct

This project and everyone participating in it is governed by our [Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code. Please report unacceptable behavior to the maintainers.

## 📋 Getting Started

### Prerequisites

- Python 3.11+
- Node.js 18+
- Git
- System dependencies: FFmpeg, FluidSynth (see [QUICKSTART.md](QUICKSTART.md))

### Development Setup

1. **Fork the repository** on GitHub

2. **Clone your fork**:
   ```bash
   git clone https://github.com/YOUR_USERNAME/band-music-helper.git
   cd band-music-helper
   ```

3. **Create a virtual environment** (backend):
   ```bash
   cd backend
   python3.11 -m venv venv
   source venv/bin/activate  # macOS/Linux
   # or
   venv\Scripts\activate  # Windows
   ```

4. **Install dependencies** (backend):
   ```bash
   pip install -r requirements.txt
   ```

5. **Install dependencies** (frontend):
   ```bash
   cd ../frontend
   npm install
   ```

6. **Configure environment**:
   ```bash
   cd ../backend
   cp .env.example .env
   # Edit .env with your configuration
   ```

### Running Locally

**Backend (Terminal 1)**:
```bash
cd backend
source venv/bin/activate
python main.py
```

**Frontend (Terminal 2)**:
```bash
cd frontend
npm start
```

**Access the application**:
- Frontend: http://localhost:3000
- API Docs: http://localhost:8000/docs

## 🌿 Git Workflow

### Branch Naming Conventions

Create branches with clear, descriptive names:

- `feature/` - New features
  - Example: `feature/add-tempo-detection`
- `fix/` - Bug fixes
  - Example: `fix/omr-pdf-timeout`
- `docs/` - Documentation updates
  - Example: `docs/update-setup-guide`
- `refactor/` - Code refactoring without feature changes
  - Example: `refactor/simplify-conversion-pipeline`
- `test/` - Test improvements
  - Example: `test/add-omr-tests`
- `chore/` - Maintenance tasks
  - Example: `chore/update-dependencies`

**Example**:
```bash
git checkout -b feature/add-tempo-detection
```

### Commit Message Conventions

Follow conventional commits format:

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Type**: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

**Scope**: Component or module affected (optional)

**Subject**: Clear, imperative, present tense, lowercase, no period

**Examples**:

```bash
# Feature
git commit -m "feat(omr): add support for multi-page PDF processing"

# Bug fix
git commit -m "fix(amt): handle edge case in silence detection"

# Documentation
git commit -m "docs: update FFmpeg installation instructions"

# Refactoring
git commit -m "refactor(services): simplify file conversion pipeline"

# Test
git commit -m "test(backend): add comprehensive OMR test suite"
```

## 📝 Code Style Guide

### Python Code Style

Follow [PEP 8](https://pep8.org/) with the following additions:

**Tool**: Use [black](https://github.com/psf/black) for formatting (88-char line length)

```bash
# Format code
cd backend
black src/ main.py

# Check formatting
black --check src/ main.py
```

**Linting**: Use [ruff](https://github.com/astral-sh/ruff)

```bash
# Check for issues
ruff check src/ main.py

# Fix automatically
ruff check --fix src/ main.py
```

**Type Hints**: All functions must have type hints

```python
# ✅ Good
def convert_omr_to_midi(
    image_path: str,
    output_path: str,
) -> bool:
    """Convert sheet music image to MIDI format."""
    pass

# ❌ Avoid
def convert_omr_to_midi(image_path, output_path):
    """Convert sheet music image to MIDI format."""
    pass
```

**Docstrings**: Follow [PEP 257](https://peps.python.org/pep-0257/)

```python
def process_audio_transcription(audio_file: str) -> Dict[str, Any]:
    """
    Transcribe audio file to MIDI using Basic Pitch.
    
    Uses Spotify's Basic Pitch model for automatic music transcription (AMT),
    supporting MP3, WAV, OGG, M4A, and FLAC formats.
    
    Args:
        audio_file: Path to the audio file
        
    Returns:
        Dictionary containing MIDI data and transcription metadata
        
    Raises:
        FileNotFoundError: If audio file does not exist
        ValueError: If audio format is not supported
        RuntimeError: If transcription fails
        
    Example:
        >>> result = await process_audio_transcription("song.mp3")
        >>> print(f"Notes detected: {len(result['notes'])}")
    """
    pass
```

**Imports**: Organize imports

```python
# Standard library
import asyncio
from typing import Dict, List

# Third-party
from fastapi import FastAPI
from loguru import logger

# Local
from src.services import ConversionService
from src.models import ConversionJob
```

### TypeScript/React Code Style

Follow [ESLint](https://eslint.org/) configuration in `frontend/.eslintrc.js`:

```bash
# Check for issues
cd frontend
npm run lint

# Fix automatically
npm run lint -- --fix
```

**Type Hints**: Use TypeScript for all components

```typescript
// ✅ Good
interface FileConversionProps {
  fileType: 'omr' | 'amt';
  onConversionStart: (jobId: string) => void;
  onConversionComplete: (result: ConversionResult) => void;
}

export const FileConversion: React.FC<FileConversionProps> = ({
  fileType,
  onConversionStart,
  onConversionComplete,
}) => {
  // Component implementation
};

// ❌ Avoid
export const FileConversion = ({ fileType, onConversionStart }) => {
  // Component implementation
};
```

## ✅ Testing

### Running Tests

**Backend** (pytest):
```bash
cd backend
source venv/bin/activate

# Run all tests
pytest

# Run specific test file
pytest tests/test_omr.py

# Run with coverage
pytest --cov=src tests/

# Run with verbose output
pytest -v
```

**Frontend** (Jest):
```bash
cd frontend

# Run all tests
npm test -- --watchAll=false

# Run specific test file
npm test -- FileUpload

# Run with coverage
npm test -- --coverage --watchAll=false
```

### Writing Tests

**Backend** (pytest):

```python
# tests/test_omr_service.py
import pytest
from src.services import OMRService
from src.models import ConversionJob

@pytest.fixture
def omr_service():
    """Create an OMR service instance for testing."""
    return OMRService()

@pytest.mark.asyncio
async def test_omr_image_conversion(omr_service, sample_sheet_music):
    """Test conversion of sheet music image to MusicXML."""
    # Arrange
    image_path = sample_sheet_music
    
    # Act
    result = await omr_service.process_image(image_path)
    
    # Assert
    assert result is not None
    assert result.format == "musicxml"
```

**Frontend** (Jest + React Testing Library):

```typescript
// src/components/__tests__/FileUpload.test.tsx
import { render, screen, waitFor } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import { FileUpload } from "../FileUpload";

describe("FileUpload", () => {
  it("should upload file and start conversion", async () => {
    const user = userEvent.setup();
    const handleUpload = jest.fn();
    
    render(<FileUpload onUpload={handleUpload} />);
    
    const uploadButton = screen.getByRole("button", { name: /upload/i });
    await user.click(uploadButton);
    
    await waitFor(() => {
      expect(handleUpload).toHaveBeenCalled();
    });
  });
});
```

## 📖 Documentation

When making changes, update relevant documentation:

1. **Code Comments**: Explain *why*, not *what*

```python
# ❌ Bad - Explains what the code does
notes = score.notes  # Get notes from score

# ✅ Good - Explains the reasoning
# Filter to only note objects (exclude rests and other elements)
notes = [n for n in score.notes if isinstance(n, Note)]
```

2. **README Updates**: Update if your changes affect setup or usage

3. **API Documentation**: Add/update docstrings for new endpoints

4. **Architecture Changes**: Update [ARCHITECTURE.md](ARCHITECTURE.md)

5. **Setup Changes**: Update [QUICKSTART.md](QUICKSTART.md)

## 🔄 Pull Request Process

1. **Keep it focused**: Each PR should address one feature or fix

2. **Update your branch** with latest main:
   ```bash
   git fetch origin
   git rebase origin/main
   ```

3. **Push to your fork**:
   ```bash
   git push origin feature/your-feature-name
   ```

4. **Create Pull Request** on GitHub with:
   - Clear title following commit conventions
   - Description of changes
   - Link to related issues
   - Screenshots for UI changes
   - Checklist completion

5. **PR Template** (auto-filled):
   ```markdown
   ## Description
   Brief description of what this PR does
   
   ## Type
   - [ ] Feature
   - [ ] Bug Fix
   - [ ] Documentation
   - [ ] Refactoring
   
   ## Changes
   - Change 1
   - Change 2
   
   ## Testing
   - [ ] Unit tests added/updated
   - [ ] Manual testing completed
   - [ ] No breaking changes
   
   ## Checklist
   - [ ] Code follows style guidelines
   - [ ] Documentation updated
   - [ ] Tests pass locally
   - [ ] No new warnings generated
   ```

6. **Address Review Comments**: Update based on feedback

7. **Maintainer Merge**: Project maintainers will merge when approved

## 🐛 Bug Reports

Use GitHub Issues with this template:

```markdown
## Description
Brief description of the bug

## Steps to Reproduce
1. Step 1
2. Step 2
3. Step 3

## Expected Behavior
What should happen

## Actual Behavior
What actually happens

## Environment
- OS: [e.g., macOS 14.1]
- Python Version: 3.11.0
- Node.js Version: 18.0.0

## Input File
- File Type: [MP3/PNG/JPG/PDF]
- File Size: [approximate]

## Logs
```
Error message and stack trace
```

## Additional Context
Any other information
```

## ✨ Feature Requests

Use GitHub Issues with this template:

```markdown
## Description
Clear description of the feature

## Motivation
Why is this feature useful?

## Use Cases
How would users benefit?

## Proposed Solution
How should this be implemented?

## Alternatives Considered
Other approaches considered

## Additional Context
Any other information
```

## 🏗️ Architecture Contributions

When contributing to core architecture:

1. **Design First**: Discuss design in an issue before implementing
2. **Update Architecture**: Document in [ARCHITECTURE.md](ARCHITECTURE.md)
3. **Add Tests**: Comprehensive test coverage required
4. **Performance**: Include performance impact analysis
5. **Migration**: Plan for any breaking changes

### Adding New Conversion Handlers

To add support for new audio/notation formats:

1. **Create handler** in `backend/src/services/`:
   ```python
   class NewFormatHandler:
       """Handle conversion for new format."""
       
       async def process(self, input_path: str) -> str:
           """Process input and return output path."""
           pass
   ```

2. **Register in router** (`backend/src/api/routes/`)

3. **Add tests** in `backend/tests/`

4. **Update documentation** in [QUICKSTART.md](QUICKSTART.md)

5. **Add to ARCHITECTURE.md**

## 📊 Code Review Checklist

Before submitting PR, ensure:

- [ ] Code follows style guide
- [ ] All tests pass (`pytest`, `npm test`)
- [ ] New tests added for new functionality
- [ ] Documentation updated
- [ ] No commented-out code
- [ ] No hardcoded values (use config)
- [ ] Type hints on all functions
- [ ] Error handling is comprehensive
- [ ] No breaking changes (or documented)
- [ ] Commit messages follow convention

## 🎓 Learning Resources

- [Python Best Practices](https://pep8.org/)
- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [React Documentation](https://react.dev/)
- [Music21 Documentation](https://web.mit.edu/music21/)
- [Basic Pitch Documentation](https://github.com/spotify/basic-pitch)
- [Band Music Architecture](ARCHITECTURE.md)
- [Band Music Development Guide](DEVELOPMENT.md)

## 🤔 Questions?

- Check existing [GitHub Issues](https://github.com/sagarkrishnamoorthy/band-music-helper/issues)
- Review [GitHub Discussions](https://github.com/sagarkrishnamoorthy/band-music-helper/discussions)
- Review project [documentation](QUICKSTART.md)

## 📜 License

By contributing to Band Music, you agree that your contributions will be licensed under its [MIT License](LICENSE).

---

**Thank you for contributing to Band Music!** 🎵
