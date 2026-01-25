# Voice to Text for VS Code

Voice-to-text extension for VS Code that uses your local WhisperX service for transcription. Perfect for dictating prompts to Claude Code or any other text input in VS Code.

## Features

- 🎤 **Voice Recording**: Press `Cmd+Shift+C` (Mac) or `Ctrl+Shift+C` (Windows/Linux) to start/stop recording
- 🤖 **WhisperX Integration**: Uses your local WhisperX service for high-quality transcription
- 💬 **Works Everywhere**: Inserts text into any VS Code editor, terminal, or input field (including Claude Code chat)
- ⚙️ **Configurable**: Set WhisperX URL, language, and diarization options

## Prerequisites

### 1. SoX (Sound eXchange)

The extension uses SoX for audio recording. Install it:

**macOS:**
```bash
brew install sox
```

**Ubuntu/Debian:**
```bash
sudo apt-get install sox libsox-fmt-all
```

**Windows:**
Download from [SoX SourceForge](https://sourceforge.net/projects/sox/files/sox/)

### 2. WhisperX Service

Make sure your WhisperX service is running:

```bash
cd whisperx-service
pip install -r requirements.txt
pip install whisperx
uvicorn main:app --host 0.0.0.0 --port 48001
```

## Installation

### Option A: Install from VSIX (Recommended)

1. Build the extension:
   ```bash
   cd vscode-voice-to-text
   npm install
   npm run compile
   ```

2. Install in VS Code:
   - Open VS Code
   - Press `Cmd+Shift+P` (Mac) or `Ctrl+Shift+P` (Windows/Linux)
   - Type "Extensions: Install from VSIX"
   - Select the generated `voice-to-text-1.0.0.vsix` file

### Option B: Development Mode

1. Install dependencies:
   ```bash
   cd vscode-voice-to-text
   npm install
   npm run compile
   ```

2. Open in VS Code:
   ```bash
   code .
   ```

3. Press `F5` to launch Extension Development Host

## Usage

### Basic Usage

1. **Start Recording**: Press `Cmd+Shift+C` (Mac) or `Ctrl+Shift+C` (Windows/Linux)
   - Status bar will show "🎤 Recording..."

2. **Speak**: Say what you want to transcribe

3. **Stop Recording**: Press `Cmd+Shift+C` again
   - Extension sends audio to WhisperX
   - Transcribed text appears at cursor position

### Using with Claude Code

1. Open Claude Code chat (`Esc` to focus input)
2. Press `Cmd+Shift+C`
3. Speak your prompt
4. Press `Cmd+Shift+C` to stop
5. Text appears in Claude Code input

### Alternative: Status Bar Button

Click the "🔊 Voice" button in the status bar (bottom right) to start/stop recording.

## Configuration

Open VS Code settings (`Cmd+,`) and search for "Voice to Text":

- **WhisperX URL**: Default is `http://localhost:48001`
- **Language**: Language code (e.g., `en`, `es`). Leave empty for auto-detection.
- **Diarization**: Enable speaker labels (requires HF_TOKEN in WhisperX service)

Or edit `.vscode/settings.json`:

```json
{
  "voiceToText.whisperxUrl": "http://localhost:48001",
  "voiceToText.language": "en",
  "voiceToText.diarization": false
}
```

## Troubleshooting

### "sox not found"

Install SoX (see Prerequisites above).

### "WhisperX service is not running"

Start the WhisperX service:
```bash
cd whisperx-service
uvicorn main:app --port 48001
```

### "No speech detected"

- Speak closer to the microphone
- Check system microphone permissions
- Ensure microphone is not muted

### "Transcription failed: timeout"

The audio file might be too long. Try shorter recordings (under 30 seconds).

## How It Works

1. **Recording**: Uses SoX to capture audio from your default microphone
2. **File**: Saves as 16kHz mono WAV file (optimized for speech)
3. **Upload**: Sends to WhisperX `/transcribe` endpoint
4. **Response**: Gets transcript text
5. **Insertion**: Inserts text at cursor position or copies to clipboard

## Keyboard Shortcuts

| Command | Mac | Windows/Linux |
|---------|-----|---------------|
| Start/Stop Recording | `Cmd+Shift+C` | `Ctrl+Shift+C` |

## Commands

Open Command Palette (`Cmd+Shift+P` / `Ctrl+Shift+P`):

- **Voice to Text: Start Recording** - Start/stop voice recording

## Requirements

- VS Code 1.85.0 or higher
- Node.js (for development)
- SoX (for audio recording)
- WhisperX service running on localhost:48001

## License

MIT

## Credits

Built for the MeetingMind AI project. Uses [WhisperX](https://github.com/m-bain/whisperX) for transcription.
