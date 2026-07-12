# Gargoyle NVR 

This repository contains the configuration for a local Frigate NVR utilizing a Mac Mini's Apple Neural Engine for object detection.

## Disaster Recovery / Fresh Install

### 1. Prerequisites

* macOS running on Apple Silicon
* Docker & Docker Compose
* Homebrew & Python 3.11 (`brew install python@3.11`)
* FrigateDetector.app installed in `/Applications/`

### 2. Environment Setup

Create the hidden `.env` file for Frigate (this is ignored by Git to protect the credentials):

```bash
cd ~/gargoyle/frigate
cat << 'SECRET' > .env
FRIGATE_REOLINK_PASSWORD=your_password_here
SECRET
```

### 3. Apple Neural Engine Daemon

Copy the daemon configuration to the OS, lock the permissions, and bootstrap it so it runs permanently in the background:

```
cp ~/gargoyle/daemons/com.gargoyle.frigatedetector.plist ~/Library/LaunchAgents/
chmod 644 ~/Library/LaunchAgents/com.gargoyle.frigatedetector.plist
launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.gargoyle.frigatedetector.plist
```

### 4. Boot the Stack

Once the daemon is running, start Frigate:

```
cd ~/gargoyle/frigate
docker compose up -d
```
