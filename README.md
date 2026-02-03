
## Check status and speed of loading in Steam

### Determines the Steam installation path through the Windows registry
 
### Reads the content_log.txt log file 

### Every minute (for 5 minutes): 

- Determines whether the download is ongoing or paused

- Shows which game is being downloaded

- Displays the download speed (as indicated in the Steam logs)
```python
import time
import winreg
from pathlib import Path

key = winreg.OpenKey(
    winreg.HKEY_CURRENT_USER,
    r"Software\Valve\Steam"
)
steam_path, _ = winreg.QueryValueEx(key, "SteamPath")

log_path = Path(steam_path) / "logs" / "content_log.txt"

for minute in range(5):
    print(f"\nMinute {minute + 1}")

    with open(log_path, "r", errors="ignore") as file:
        lines = file.readlines()[-200:]

    info_line = None

    for line in reversed(lines):
        if "Downloading app" in line and "MB" in line:
            info_line = line.strip()
            break

    if info_line:
        print("Download:", info_line)
    else:
        print("No active download found")

    time.sleep(60)
```
