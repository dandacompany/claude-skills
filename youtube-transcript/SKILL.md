---
name: youtube-transcript
description: Extract YouTube video transcripts/subtitles using yt-dlp. Handles rate limiting, auto-generated captions, multiple languages, and VTT parsing.
---


# YouTube Transcript Extraction Skill

## Overview
Extract YouTube transcripts and download videos using yt-dlp via Desktop Commander MCP. Executes commands directly on your local machine with automatic error handling and cross-platform support.

## How It Works
- Claude uses `start_process` to run yt-dlp commands on your local system
- Automatically checks tool installation and offers to install if missing
- Handles YouTube bot detection and common errors automatically
- Downloads to ~/Downloads by default (customizable)
- Monitors progress and provides real-time status updates

## Prerequisites Check & Installation

### Claude Will Automatically:
1. Check if yt-dlp and ffmpeg are installed
2. Detect your OS and available package managers
3. Offer installation options if tools are missing
4. Verify installation and update if needed

### Installation Commands by Platform:
```bash
# macOS (Homebrew)
brew install yt-dlp ffmpeg

# Windows (Chocolatey - run as Admin)
choco install yt-dlp ffmpeg -y

# Linux (apt)
sudo apt update && sudo apt install yt-dlp ffmpeg -y

# Linux (snap)
sudo snap install yt-dlp ffmpeg
```

## Core Usage

### Request Examples:
- "유튜브 자막 추출해줘: [URL]" → Korean subtitles only
- "영상도 다운로드해줘: [URL]" → Video + Korean subtitles (720p)
- "영어 자막도 추가해줘" → Multiple languages
- "/path/to/folder에 저장해줘" → Custom output directory

### What Claude Executes:

**Subtitles Only (Default):**
```bash
yt-dlp --extractor-args "youtube:player_client=android" \
  --write-auto-sub --sub-lang ko \
  --convert-subs srt --skip-download \
  "VIDEO_URL"
```

**Video + Subtitles:**
```bash
yt-dlp --extractor-args "youtube:player_client=android" \
  --write-auto-sub --sub-lang ko,en \
  --convert-subs srt \
  -f "best[height<=720]" \
  "VIDEO_URL"
```

## Error Handling (Automatic)

### Bot Detection Error
**Error**: "Sign in to confirm you're not a bot"
**Solution**: Use Android client (already included in commands above)

### HTTP 403 Forbidden
**Error**: Video download fails with 403
**Claude's Response**:
1. Retry with 720p quality (automatic)
2. Try iOS client if 720p fails
3. Offer browser cookies as last resort (requires approval)

### HTTP 429 Rate Limited
**Error**: "Too Many Requests" for subtitles
**Claude's Response**:
1. Add `--sleep-subtitles 2` delay
2. Download languages separately with 5-second intervals

### Missing Subtitles
**Error**: "No subtitles available"
**Claude's Response**:
1. Check with `--list-subs` first
2. Try auto-generated if manual unavailable
3. Report if video has no captions

### Tool Not Found
**Error**: "command not found: yt-dlp"
**Claude's Response**:
1. Detect OS and package manager
2. Offer installation with commands
3. Install with user approval
4. Verify and retry download

## Key Parameters Explained

| Parameter | Purpose | When Used |
|-----------|---------|-----------|
| `--extractor-args "youtube:player_client=android"` | Bypass bot detection | Always (most reliable) |
| `--write-auto-sub` | Download auto-generated subtitles | When manual subs unavailable |
| `--sub-lang ko,en` | Specify languages | Default: ko (Korean) |
| `--convert-subs srt` | Convert to SRT format | Always (best compatibility) |
| `--skip-download` | Subtitles only, no video | Subtitle-only requests |
| `-f "best[height<=720]"` | 720p quality | Video downloads (avoids 403) |
| `--cookies-from-browser safari` | Use browser session | Last resort for persistent errors |

## Claude's Execution Flow

### Step 1: Verify Tools
```bash
which yt-dlp && yt-dlp --version
which ffmpeg && ffmpeg -version
```

### Step 2: Navigate to Output Directory
```bash
cd ~/Downloads  # or user-specified path
```

### Step 3: Execute Download
```bash
# Start process with 180-second timeout for videos
start_process("yt-dlp [options] VIDEO_URL", timeout_ms=180000)
```

### Step 4: Monitor Progress
```bash
# Check output every 10 seconds
read_process_output(pid, timeout_ms=10000)
# Watch for: completion, errors, progress percentage
```

### Step 5: Handle Errors
- Detect error type from output
- Apply appropriate solution automatically
- Report status to user

### Step 6: Verify & Report
```bash
ls -la *.srt *.mp4  # List downloaded files
```
- Provide absolute file paths
- Confirm successful downloads
- Mention any issues encountered

## Best Practices for Claude

1. **Always start with Android client** - highest success rate
2. **Use 720p for videos** - best balance of quality and reliability
3. **Check subtitles first** - run `--list-subs` if uncertain
4. **Handle errors gracefully** - try alternatives before asking user
5. **Ask before cookies** - browser cookies need user approval
6. **Provide absolute paths** - always report full file locations
7. **Clean up .part files** - remove incomplete downloads on failure
8. **Update when needed** - suggest upgrade if repeated failures

## Timeout Guidelines

```python
# Short: Check/list operations (5-10s)
start_process("yt-dlp --list-subs URL", timeout_ms=10000)

# Medium: Subtitle downloads (30-60s)
start_process("yt-dlp --skip-download ...", timeout_ms=60000)

# Long: Video downloads (2-3 minutes)
start_process("yt-dlp -f best[height<=720] ...", timeout_ms=180000)
```

## Common Request Patterns

| User Request | Claude Action | Output |
|-------------|---------------|---------|
| "자막 추출해줘: [URL]" | Subtitles only, Korean | .srt file |
| "영상도 다운로드" | 720p video + subtitles | .mp4 + .srt |
| "영어 자막도 추가" | Add `--sub-lang ko,en` | Multiple .srt files |
| "1080p로 다운로드" | Try, fallback to 720p if 403 | Best available quality |
| "/path에 저장" | Navigate to path first | Files in specified directory |

## Advanced Options

### Playlist Download
```bash
yt-dlp --extractor-args "youtube:player_client=android" \
  --write-auto-sub --sub-lang ko \
  "PLAYLIST_URL"
```

### Custom Filename Template
```bash
yt-dlp -o "%(uploader)s_%(title)s_%(upload_date)s.%(ext)s" VIDEO_URL
```

### Resume Interrupted Download
```bash
yt-dlp --continue "VIDEO_URL"
```

### Multiple URLs (Batch Processing)
Claude will process each URL sequentially with appropriate delays between downloads.

## Troubleshooting Quick Reference

| Symptom | Cause | Quick Fix |
|---------|-------|-----------|
| Bot detection error | Default client blocked | Use `player_client=android` |
| 403 on video | High quality blocked | Lower to 720p or below |
| 429 on subtitles | Rate limited | Add delays between languages |
| Cookie decrypt failed | Browser security | Close browser, grant permissions |
| No subtitles found | Video has no captions | Check `--list-subs`, try auto-generated |
| Command not found | Tools not installed | Install via package manager |

## Platform-Specific Notes

**macOS**: Safari cookies work best, may need Full Disk Access for Terminal
**Windows**: Use PowerShell/CMD, Chrome cookies recommended
**Linux**: Firefox or Chrome cookies, may need sudo for installation

## Support
- GitHub: https://github.com/yt-dlp/yt-dlp
- Update regularly - YouTube API changes frequently