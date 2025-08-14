# 🤖 Complete Setup Guide: Tally Birthday Bot + GitHub Pages

## 📋 **Table of Contents**
1. [Prerequisites](#prerequisites)
2. [Creating the Telegram Bot](#creating-telegram-bot)
3. [Setting up GitHub Pages](#setting-up-github-pages)
4. [Getting File IDs](#getting-file-ids)
5. [Configuring the Bot](#configuring-the-bot)
6. [Running the Bot](#running-the-bot)
7. [Testing Everything](#testing-everything)
8. [Troubleshooting](#troubleshooting)

---

## 🎯 **Prerequisites**

### Required Software:
- **Python 3.8+** (Download from [python.org](https://python.org))
- **Git** (Download from [git-scm.com](https://git-scm.com))
- **Text Editor** (VS Code, Sublime Text, or any editor)
- **GitHub Account** (Sign up at [github.com](https://github.com))
- **Telegram Account** (Mobile app required)

### Required Files:
- **Sticker files** (.webp format)
- **Audio files** (.mp3, .ogg, or .wav)
- **Image files** (.jpg, .png for birthday remix)

---

## 🤖 **Part 1: Creating the Telegram Bot**

### Step 1: Create Bot with BotFather

1. **Open Telegram** and search for `@BotFather`
2. **Start conversation** with BotFather
3. **Send command**: `/newbot`
4. **Choose bot name**: `Tally Birthday Bot` (or any name you like)
5. **Choose username**: `tally_birthday_bot` (must end with 'bot')
6. **Copy the token** that looks like: `123456789:ABCdefGhIjKlMnOpQrStUvWxYz`

### Step 2: Configure Bot Settings

1. **Send to BotFather**: `/setcommands`
2. **Select your bot**
3. **Set commands** (copy-paste this):
```
start - Initialize the bot
tallybday - Start birthday celebration
help - Show help and current settings
```

4. **Enable Inline Mode** (optional):
   - Send: `/setinline`
   - Select your bot
   - Enter: `Search birthday roasts...`

### Step 3: Get Group Chat IDs

#### Method 1: Using Web Browser
1. **Open Telegram Web**: [web.telegram.org](https://web.telegram.org)
2. **Go to your test group**
3. **Look at URL**: `https://web.telegram.org/k/#-1001234567890`
4. **Copy the number** (including the minus sign): `-1001234567890`

#### Method 2: Using Bot
1. **Add your bot** to the test group
2. **Make bot admin** (optional, for better functionality)
3. **Send a message** in the group
4. **Go to**: `https://api.telegram.org/bot<YOUR_BOT_TOKEN>/getUpdates`
5. **Look for** `"chat":{"id":-1001234567890}`

---

## 🌐 **Part 2: Setting up GitHub Pages**

### Step 1: Create GitHub Repository

1. **Go to** [github.com](https://github.com)
2. **Click** "New repository" (green button)
3. **Repository name**: `tally-birthday-app`
4. **Make it public** ✅
5. **Add README** ✅
6. **Click** "Create repository"

### Step 2: Upload Mini-Game HTML

1. **In your new repository**, click "Add file" → "Create new file"
2. **Name the file**: `index.html`
3. **Copy the HTML code** from the bot artifact (the long HTML code at the bottom)
4. **Paste it** into the file editor
5. **Scroll down** and click "Commit new file"

### Step 3: Enable GitHub Pages

1. **Go to repository Settings** (tab at the top)
2. **Scroll down** to "Pages" section (left sidebar)
3. **Source**: Select "Deploy from a branch"
4. **Branch**: Select "main" (or "master")
5. **Folder**: Select "/ (root)"
6. **Click** "Save"
7. **Wait 2-3 minutes** for deployment
8. **Your URL** will be: `https://yourusername.github.io/tally-birthday-app/`

### Step 4: Test GitHub Pages

1. **Open your URL** in browser
2. **You should see** the birthday mini-game
3. **Test touch/click** functionality
4. **If it doesn't work**, wait a few more minutes

---

## 🎵 **Part 3: Getting File IDs**

### Step 1: Create File ID Bot (Temporary Helper)

Create a simple bot to get file IDs:

```python
# file_id_getter.py
import logging
from telegram import Update
from telegram.ext import Application, MessageHandler, filters, ContextTypes

logging.basicConfig(level=logging.INFO)

BOT_TOKEN = "YOUR_BOT_TOKEN_HERE"  # Same token as main bot

async def handle_media(update: Update, context: ContextTypes.DEFAULT_TYPE):
    message = update.message
    
    if message.sticker:
        await message.reply_text(f"🎯 Sticker ID:\n`{message.sticker.file_id}`")
    elif message.audio:
        await message.reply_text(f"🎵 Audio ID:\n`{message.audio.file_id}`")
    elif message.voice:
        await message.reply_text(f"🎤 Voice ID:\n`{message.voice.file_id}`")
    elif message.photo:
        photo_id = message.photo[-1].file_id  # Get highest quality
        await message.reply_text(f"🖼️ Photo ID:\n`{photo_id}`")
    elif message.document:
        await message.reply_text(f"📄 Document ID:\n`{message.document.file_id}`")

def main():
    app = Application.builder().token(BOT_TOKEN).build()
    app.add_handler(MessageHandler(filters.ALL, handle_media))
    print("🤖 File ID Bot running... Send media files to get their IDs!")
    app.run_polling()

if __name__ == "__main__":
    main()
```

### Step 2: Get Your File IDs

1. **Install python-telegram-bot**:
   ```bash
   pip install python-telegram-bot
   ```

2. **Run the helper bot**:
   ```bash
   python file_id_getter.py
   ```

3. **Send media files** to your bot:
   - Send sticker → Copy the sticker ID
   - Send audio → Copy the audio ID  
   - Send photo → Copy the photo ID
   - Repeat for all files

4. **Keep track** of all IDs in a text file:
   ```
   Stickers:
   - Party sticker: CAACAgIAAxkBAAICGmF8X7yVq...
   - Birthday cake: CAACAgIAAxkBAAICGmF8X7yVq...
   
   Audios:
   - Chipi chipi song: CQACAgIAAxkBAAICGmF8X7yVq...
   - Cat meow: CQACAgIAAxkBAAICGmF8X7yVq...
   
   Images:  
   - Birthday remix: AgACAgIAAxkBAAICGmF8X7yVq...
   ```

---

## ⚙️ **Part 4: Configuring the Bot**

### Step 1: Create Bot Directory

```bash
mkdir tally_birthday_bot
cd tally_birthday_bot
```

### Step 2: Install Dependencies

```bash
pip install python-telegram-bot asyncio
```

### Step 3: Create Main Bot File

1. **Create file**: `tally_bot.py`
2. **Copy the main bot code** from the artifact
3. **Replace these values**:

```python
# Configuration - REPLACE THESE VALUES!
BOT_TOKEN = "123456789:ABCdefGhIjKlMnOpQrStUvWxYz"  # Your bot token
TARGET_GROUP_ID = -1002371078887  # Production group ID
TALLY_USER_ID = 1712674240  # Tally's user ID
TALLY_USERNAME = "@blabla_here"  # Tally's username

# Test mode configuration
TEST_MODE = True  # Set to False for production
TEST_GROUP_ID = -1001234567890  # Your test group ID

# File IDs - Replace with your actual file IDs
STICKER_IDS = [
    "CAACAgIAAxkBAAICGmF8X7yVq...",  # Replace with real sticker IDs
    "CAACAgIAAxkBAAICHmF8X7yVq...",
    "CAACAgIAAxkBAAICImF8X7yVq...",
    # Add more sticker IDs
]

AUDIO_IDS = {
    "main_birthday": "CQACAgIAAxkBAAICGmF8X7yVq...",  # Chipi chipi song
    "cat_meow": "CQACAgIAAxkBAAICHmF8X7yVq...",      # Cat meow audio
    "cockroach_confession": "CQACAgIAAxkBAAICImF8X7yVq...",  # "I love cockroach"
    "singing": "CQACAgIAAxkBAAICJmF8X7yVq..."        # Tally singing
}

IMAGE_IDS = {
    "birthday_remix": "AgACAgIAAxkBAAICGmF8X7yVq..."  # Birthday remix image
}

# Mini App URL - Replace with your GitHub Pages URL
MINI_APP_URL = "https://yourusername.github.io/tally-birthday-app/"
```

### Step 4: Create Requirements File

Create `requirements.txt`:
```
python-telegram-bot==20.7
asyncio
```

---

## 🚀 **Part 5: Running the Bot**

### Step 1: Test Mode First

1. **Make sure** `TEST_MODE = True` in your code
2. **Add your bot** to your test group
3. **Make bot admin** in test group (recommended)
4. **Run the bot**:
   ```bash
   python tally_bot.py
   ```

### Step 2: Test Basic Functionality

1. **In test group**, send: `/start`
2. **Bot should respond** with test mode message
3. **Send**: `/tallybday`
4. **Bot should start** the birthday sequence
5. **Test buttons** (anyone can press in test mode)

### Step 3: Test Mini-Game

1. **Complete the quiz** to get mini-game button
2. **Press "Open Your Birthday Surprise"**
3. **Mini-game should open** in Telegram's WebApp
4. **Test tapping cockroaches**
5. **Reach 10 points** for victory

### Step 4: Switch to Production

1. **Change** `TEST_MODE = False`
2. **Add bot** to production group
3. **Test with Tally's account** only
4. **Others should get** "hands off" message

---

## 🧪 **Part 6: Testing Everything**

### Test Checklist:

#### ✅ **Bot Setup**
- [ ] Bot responds to `/start`
- [ ] Bot shows correct mode (Test/Production)
- [ ] Bot works only in authorized groups
- [ ] `/help` command shows all info

#### ✅ **Birthday Flow**
- [ ] `/tallybday` starts celebration
- [ ] Intro message appears with roast
- [ ] Stickers send (or emoji fallbacks)
- [ ] Audio plays (or text fallbacks)
- [ ] Surprise button appears

#### ✅ **Interactive Features**
- [ ] Surprise button works for authorized users
- [ ] Others get "hands off" message (production)
- [ ] Quiz game starts properly
- [ ] All quiz questions work
- [ ] Quiz responses are funny
- [ ] Final victory message appears

#### ✅ **Mini-Game**
- [ ] GitHub Pages URL loads
- [ ] Mini-game opens in Telegram WebApp
- [ ] Cockroaches appear and move
- [ ] Tapping works (touch/click)
- [ ] Score increases correctly
- [ ] Victory celebration at 10 points
- [ ] Fireworks animation works

#### ✅ **Error Handling**
- [ ] Missing stickers → emoji fallbacks
- [ ] Missing audio → text fallbacks  
- [ ] Network errors don't crash bot
- [ ] Invalid button presses handled

---

## 🔧 **Part 7: Advanced Configuration**

### Option 1: Running on VPS/Server

**For 24/7 operation:**

1. **Get a VPS** (DigitalOcean, AWS, etc.)
2. **Install Python** on server
3. **Upload your bot code**
4. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```
5. **Run with screen/tmux**:
   ```bash
   screen -S tally_bot
   python tally_bot.py
   # Press Ctrl+A, then D to detach
   ```

### Option 2: Using systemd (Linux)

Create `/etc/systemd/system/tally-bot.service`:
```ini
[Unit]
Description=Tally Birthday Bot
After=multi-user.target

[Service]
Type=simple
User=your-username
WorkingDirectory=/path/to/your/bot
ExecStart=/usr/bin/python3 /path/to/your/bot/tally_bot.py
Restart=always

[Install]
WantedBy=multi-user.target
```

Enable and start:
```bash
sudo systemctl enable tally-bot
sudo systemctl start tally-bot
```

### Option 3: Using Docker

Create `Dockerfile`:
```dockerfile
FROM python:3.11-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY tally_bot.py .
CMD ["python", "tally_bot.py"]
```

Run:
```bash
docker build -t tally-bot .
docker run -d --restart always tally-bot
```

---

## 🐛 **Part 8: Troubleshooting**

### Common Issues:

#### **Bot doesn't respond**
- ✅ Check bot token is correct
- ✅ Make sure bot is added to group
- ✅ Check group ID is correct
- ✅ Ensure bot has necessary permissions

#### **Stickers/Audio don't send**
- ✅ Verify file IDs are correct and valid
- ✅ Check file size limits (stickers: 512KB, audio: 50MB)
- ✅ Ensure files are in correct format

#### **Mini-game doesn't load**
- ✅ Check GitHub Pages URL is accessible
- ✅ Verify `index.html` is in repository root
- ✅ Wait 5-10 minutes after GitHub Pages deployment
- ✅ Check browser console for JavaScript errors

#### **Buttons don't work**
- ✅ Check user ID restrictions in production mode
- ✅ Verify callback handlers are registered
- ✅ Look for error messages in bot logs

#### **Permission errors**
- ✅ Make bot admin in group (optional but recommended)
- ✅ Check bot permissions in group settings
- ✅ Ensure bot can send messages, stickers, audio

### Debug Commands:

Add these to your bot for debugging:
```python
@bot.command()
async def debug(update: Update, context: ContextTypes.DEFAULT_TYPE):
    info = f"""
    Chat ID: {update.effective_chat.id}
    User ID: {update.effective_user.id}
    Username: @{update.effective_user.username}
    Test Mode: {TEST_MODE}
    """
    await update.message.reply_text(f"🔍 Debug Info:\n```\n{info}\n```", parse_mode='Markdown')
```

---

## 🎉 **Part 9: Final Steps**

### Production Deployment:

1. **Test everything** thoroughly in test mode
2. **Get all file IDs** for production media
3. **Set** `TEST_MODE = False`
4. **Update group and user IDs** for production
5. **Deploy to server** for 24/7 operation
6. **Monitor logs** for any issues

### Maintenance:

- **Check logs** regularly for errors
- **Update file IDs** if media changes
- **Monitor GitHub Pages** uptime
- **Keep bot token secure** (never share!)

### Security:

- **Never commit** bot token to git
- **Use environment variables** for sensitive data:
  ```python
  import os
  BOT_TOKEN = os.getenv('BOT_TOKEN')
  ```
- **Set repository** to private if it contains tokens
- **Regenerate token** if compromised

---

## 📞 **Need Help?**

If you encounter issues:

1. **Check bot logs** for error messages
2. **Test with** `@userinfobot` to get user/chat IDs
3. **Use** `@BotFather` to verify bot settings
4. **Check GitHub Pages** status and deployment
5. **Verify all file IDs** are valid and current

**Common Commands for Testing:**
- `/start` - Test basic functionality
- `/tallybday` - Start full celebration
- `/help` - Check current configuration
- Send media files to file ID bot to get IDs

---

**🎊 Congratulations! Your Tally Birthday Bot should now be fully functional with an amazing mini-game! 🎊**