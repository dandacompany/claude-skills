# Claude Skills by Dante Labs

Public repository of custom Claude Skills for enhanced AI workflows.

## 🎥 Introduction Video

[![Claude Skills Tutorial](https://img.youtube.com/vi/vqLONWXfMsI/0.jpg)](https://www.youtube.com/watch?v=vqLONWXfMsI)

Watch the tutorial on YouTube: [Claude Skills by Dante Labs](https://www.youtube.com/watch?v=vqLONWXfMsI)

## 🎯 About

A collection of production-ready Claude Skills developed by Dante Labs to extend Claude's capabilities with specialized workflows and integrations.

## 📦 Available Skills

### Platform Compatibility

| Skill | Claude Code | Claude Desktop | Claude Web | Claude Mobile | Requirements |
|-------|-------------|----------------|------------|---------------|--------------|
| youtube-summary | ✅ | ✅ | ✅ | ✅ | Requires youtube-video-info & youtube-transcript skills |
| youtube-transcript | ✅ | ✅ | ❌ | ❌ | **Desktop Commander MCP** required for Desktop |
| storage-cleaner | ✅ | ✅ | ❌ | ❌ | **Desktop Commander MCP** required for Desktop |

**Note:** Skills marked with ❌ require system command execution, which is only available in Claude Code CLI and Claude Desktop with Desktop Commander MCP installed.

---

### [youtube-summary](./youtube-summary)

Create structured, educational summaries of YouTube videos with comprehensive analysis format.

**Features:**

- Structured video summaries with key topics and insights
- Timestamp-based navigation
- Data-driven analysis and exploratory questions
- Educational content breakdown

### [youtube-transcript](./youtube-transcript)

Extract YouTube video transcripts/subtitles using yt-dlp with advanced error handling.

**Features:**

- Multi-language subtitle extraction
- Auto-generated caption support
- Rate limiting and bot detection handling
- VTT/SRT format conversion
- Cross-platform support (macOS/Windows/Linux)
- **Requires:** yt-dlp, ffmpeg

### [storage-cleaner](./storage-cleaner)

Manage computer storage and clean up disk space with comprehensive analysis and safe deletion workflows.

**Features:**

- Find large files consuming storage space
- Detect duplicate files by content hash
- Identify unused applications
- Clean system caches and temporary files
- Docker resource cleanup (images, containers, volumes)
- Cross-platform support (macOS/Windows/Linux)
- Analysis-first workflow with user approval required for all deletions

## 🚀 Installation

Each skill can be installed independently:

1. Download the skill directory
2. Place in your Claude Code skills folder
3. Restart Claude Code to load the skill

## 📚 Documentation

Detailed documentation is available in each skill's `SKILL.md` file.

## 🔗 Connect

- **Homepage:** [dante-datalab.com](https://dante-datalab.com)
- **YouTube:** [@dante-labs](https://youtube.com/@dante-labs)
- **Email:** [datapod.k@gmail.com](mailto:datapod.k@gmail.com)
- **Discord:** [Dante Labs Community](https://discord.com/invite/rXyy5e9ujs)

## ☕️ Support

Enjoying these skills? Support the development!

[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-Support-yellow?style=for-the-badge&logo=buy-me-a-coffee)](https://buymeacoffee.com/dante.labs)

**☕️ 커피 한 잔 후원:** <https://buymeacoffee.com/dante.labs>

**💬 단테랩스 커뮤니티 디스코드:** <https://discord.com/invite/rXyy5e9ujs>

## 📄 License

MIT License - See individual skill directories for details.

---

Built with ❤️ by [Dante Labs](https://dante-datalab.com)
