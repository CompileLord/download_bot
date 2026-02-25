# 🚀 Enhanced Telegram Download Bot

A powerful Telegram bot for downloading files with advanced features like **parallel downloading**, **SourceForge optimization**, and **smart retry mechanisms**.

## ✨ Key Features

### 🔥 Performance
- **Parallel Downloading**: Uses 8 simultaneous connections for 5-10x faster speeds
- **Smart Chunking**: Efficiently splits files into optimal chunks
- **Connection Pooling**: Reuses connections for better performance
- **Resume Support**: Automatically resumes failed downloads

### 🎯 SourceForge Optimization
- **Automatic Detection**: Recognizes SourceForge URLs automatically
- **Direct URL Extraction**: Converts project pages to direct download links
- **Mirror Selection**: Uses fastest available mirrors
- **ZIP File Support**: Optimized for ZIP archives from SourceForge

### 💪 Reliability
- **Exponential Backoff**: Smart retry with increasing delays (2s, 4s, 8s, 16s, 32s)
- **5 Retry Attempts**: Automatically retries failed chunks
- **Connection Recovery**: Handles network interruptions gracefully
- **Timeout Protection**: Prevents hanging connections

### 📊 User Experience
- **Real-time Progress**: Live percentage, speed, and ETA
- **Visual Progress Bar**: Beautiful ASCII progress indicator
- **Speed Tracking**: Shows current download speed in MB/s
- **Smart File Detection**: Extracts filenames from headers and URLs

## 📋 Requirements

- Python 3.8+
- Telegram Bot Token (from [@BotFather](https://t.me/botfather))
- 2GB free disk space (for temporary downloads)

## 🛠️ Installation

### 1. Clone or Download

```bash
git clone <your-repo-url>
cd telegram-download-bot
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Configure Bot Token

Open `enhanced_download_bot.py` and replace:

```python
BOT_TOKEN = "YOUR_BOT_TOKEN_HERE"
```

With your actual bot token from [@BotFather](https://t.me/botfather).

### 4. Run the Bot

```bash
python enhanced_download_bot.py
```

## 🎮 Usage

### Basic Commands

- `/start` - Welcome message and feature overview
- `/help` - Detailed help and usage guide
- `/stats` - View current download statistics
- `/cancel` - Cancel active download

### Download Files

Simply send any URL to the bot:

**SourceForge Examples:**
```
https://sourceforge.net/projects/sevenzip/files/7-Zip/23.01/7z2301-x64.exe/download
https://sourceforge.net/projects/winmerge/files/latest/download
https://sourceforge.net/projects/notepadplusplus/files/v8.5.8/npp.8.5.8.Installer.x64.exe/download
```

**Direct Download Examples:**
```
https://github.com/user/repo/releases/download/v1.0/file.zip
https://downloads.example.com/software.exe
https://cdn.example.com/archive.tar.gz
```

### Progress Display

```
📥 Downloading: filename.zip

[████████████░░░░░░░░]
Progress: 65.3%
Downloaded: 650 MB / 1.0 GB
Speed: 8.5 MB/s
ETA: 42s

⚡ Using 8 parallel connections
```

## ⚙️ Configuration

Customize settings in the bot file:

```python
# Download Configuration
DOWNLOAD_PATH = "downloads"              # Temp download directory
MAX_FILE_SIZE = 2 * 1024 * 1024 * 1024  # 2GB max
CHUNK_SIZE = 512 * 1024                  # 512KB per chunk
PARALLEL_CHUNKS = 8                      # Parallel connections
RETRY_ATTEMPTS = 5                       # Retry count
RETRY_DELAY = 2                          # Initial retry delay (seconds)
```

## 🔧 Advanced Features

### Parallel Download Algorithm

The bot uses intelligent parallel downloading:

1. **File Analysis**: Checks if server supports range requests
2. **Chunk Calculation**: Divides file into N equal parts
3. **Parallel Fetching**: Downloads all chunks simultaneously
4. **Smart Assembly**: Combines chunks in correct order
5. **Verification**: Checks final file size

```
File (1000MB)
    ├── Chunk 0: 0-124MB    [Thread 1]
    ├── Chunk 1: 125-249MB  [Thread 2]
    ├── Chunk 2: 250-374MB  [Thread 3]
    ├── Chunk 3: 375-499MB  [Thread 4]
    ├── Chunk 4: 500-624MB  [Thread 5]
    ├── Chunk 5: 625-749MB  [Thread 6]
    ├── Chunk 6: 750-874MB  [Thread 7]
    └── Chunk 7: 875-999MB  [Thread 8]
```

### SourceForge URL Detection

The bot intelligently handles various SourceForge URL formats:

- **Direct Downloads**: `https://downloads.sourceforge.net/project/...`
- **Project Pages**: `https://sourceforge.net/projects/name/files/...`
- **Latest Releases**: `https://sourceforge.net/projects/name/files/latest/download`
- **Specific Versions**: Extracts project name and file path automatically

### Retry Logic with Exponential Backoff

```python
Attempt 1: Wait 2s  → Download
Attempt 2: Wait 4s  → Download  (if failed)
Attempt 3: Wait 8s  → Download  (if failed)
Attempt 4: Wait 16s → Download  (if failed)
Attempt 5: Wait 32s → Download  (if failed)
```

This prevents overwhelming servers while maximizing success rate.

## 📊 Performance Comparison

| Feature | Basic Download | Enhanced Bot |
|---------|---------------|--------------|
| **Connections** | 1 | 8 |
| **Speed (100MB file)** | 1-2 MB/s | 8-15 MB/s |
| **Retry Logic** | None | Exponential backoff |
| **Resume Support** | ❌ | ✅ |
| **Progress Tracking** | Basic | Real-time with ETA |
| **SourceForge** | Manual links | Auto-detection |
| **Mirror Selection** | None | Automatic |

## 🐛 Troubleshooting

### Bot Not Responding
- Check if bot token is correct
- Verify bot is running: `python enhanced_download_bot.py`
- Check internet connection

### Download Fails Immediately
- Verify URL is accessible in browser
- Check if file size exceeds 2GB
- Try direct download link instead of page URL

### Slow Download Speed
- Increase `PARALLEL_CHUNKS` (up to 16)
- Check your internet connection
- Server may have rate limiting

### SourceForge Links Not Working
- Try copying the direct download link
- Ensure URL includes `/download` at the end
- Check if project/file is still available

## 🔒 Security Notes

- Bot only downloads from provided URLs
- No automatic execution of files
- Temporary files are deleted after upload
- Each user can only have one active download

## 📝 Technical Details

### Architecture

```
User → Telegram → Bot Handler
                    ↓
              URL Analyzer
                    ↓
         SourceForge Parser (if needed)
                    ↓
            File Info Extractor
                    ↓
          Parallel Downloader
          ↓        ↓        ↓
       Chunk 1  Chunk 2  Chunk N
          ↓        ↓        ↓
            File Assembler
                    ↓
           Telegram Upload
```

### Technologies Used

- **python-telegram-bot**: Async Telegram API
- **aiohttp**: Async HTTP client with connection pooling
- **aiofiles**: Async file I/O
- **asyncio**: Concurrent task execution
- **humanize**: Human-readable file sizes and times

## 📈 Future Enhancements

- [ ] Video streaming support
- [ ] Automatic extraction of ZIP files
- [ ] Multiple file downloads from page
- [ ] Torrent support
- [ ] Custom mirror selection
- [ ] Download scheduling
- [ ] User quota management
- [ ] Cloud storage integration

## 📄 License

MIT License - Feel free to modify and distribute

## 🤝 Contributing

Contributions welcome! Please feel free to submit pull requests or open issues.

## 💬 Support

If you encounter issues:
1. Check the troubleshooting section
2. Review bot logs for error messages
3. Open an issue with details

## 🌟 Credits

Created with ❤️ for efficient file downloading on Telegram

---

**Note**: Respect server bandwidth and terms of service when downloading files. This bot is for personal use and legitimate downloads only.