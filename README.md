# NetLocker 🔒

**Monitor & Reduce IP Exposure** 

Python tool that monitors your computer's public IP and restricts network access when it detects the use of an unapproved, exposed, or non-VPN IP address. 
<br>
<br>
Automatically disable network interfaces when your VPN is off or your IP is too exposed. Useful for malware analysis and minimizing unwanted connections to/from your personal IP address.

## 🚀 Features
- **Dynamic Network Control** - Automatically disables network adapters when an exposed IP is detected  
- **IP Monitoring** - Continuously checks your public IP on intervals
- **Explicit IP Control** - Set explicitly approved or blocked IPs and subnets via config file
- **Windows Integration** - Management via tray icon, alerts via system notifications
- **Malware Analysis** - Monitoring pauses when FakeNet.exe is detected

## 🔍 Network Blocking 

When an exposed IP is detected in Full Protection mode:

<br>

<img width="600" height="411" alt="image" src="https://github.com/user-attachments/assets/5c193c65-0e8c-4188-ab6b-4c9656fc38fc" />

<img width="358" height="129" alt="image" src="https://github.com/user-attachments/assets/fcbde657-6a8a-402d-8fe8-d39847556914" />

## 🛠️ Usage

### System Tray Menu
Right-click the NetLocker system tray icon:

<br>

<img width="300" height="275" alt="image" src="https://github.com/user-attachments/assets/08128888-16ac-4831-bb57-3ff7e35a4e06" />


### Command Line Options
```bash
NetLocker.exe [options]

Options:
  --mode {full,monitor,silent}  Protection mode (default: full)
  --config CONFIG              Custom config file path
  --interval SECONDS           Check interval (default: 30)
  --no-tray                    Disable system tray icon
```

### CLI Menu Commands (--no-tray)
```
netlocker> status    # Show current status
netlocker> full      # Switch to full protection
netlocker> monitor   # Switch to monitor only
netlocker> silent    # Switch to silent mode
netlocker> pause     # Pause/resume monitoring
netlocker> unblock   # Force unblock network
netlocker> config    # Open config file
netlocker> quit      # Exit
```

## 🔧 Installation & Setup

### Option 1: Run Executable (Recommended)
1. Download the [Executable](https://github.com/blwhit/NetLocker/raw/refs/heads/main/NetLocker.exe)
2. Configure your IP ranges in `config.json`
3. Run `NetLocker.exe`

### Option 2: Download Executable (Recommended)
1. Download the Python source code release from [Releases page](https://github.com/blwhit/NetLocker/releases)
2. Configure your IP ranges in `config.json`
3. Run the Script:
```
pip install requests psutil plyer pystray pillow
python NetLocker.py
```

## ⚙️ Configuration

Create or edit `config.json` in the **same** directory as NetLocker:

```json
{
  "allowed_ranges": [
    "10.0.0.0/8",
    "192.168.1.0/24",
    "203.0.113.0/24"
  ],
  "blocked_ranges": [
    "8.8.8.0/24",
    "1.1.1.0/24"
  ]
}
```

### Configuration Options:
- **allowed_ranges** - IPs that are safe to be publicly exposed (VPN/Proxy IPs)
- **blocked_ranges** - IPs that should trigger network restriction/disable (Your Personal IPs)
- You can specify either or both types of ranges in `config.json`
- Supports individual IPs (`192.168.1.1`) or CIDR notation (`192.168.1.0/24`)

## 🏃 Run Automatically (Recommended)

To run NetLocker in the background at login (via Scheduled Task):

```powershell
# Navigate to NetLocker directory
cd NetLocker

# Create NetLocker Scheduled Task
$currentDir = Get-Location
$exePath = Join-Path $currentDir "NetLocker.exe"
$configPath = Join-Path $currentDir "config.json"
$action = New-ScheduledTaskAction -Execute $exePath -Argument "--config `"$configPath`""
$trigger = New-ScheduledTaskTrigger -AtLogOn -User $env:USERNAME
$settings = New-ScheduledTaskSettingsSet -AllowStartIfOnBatteries -RestartCount 5 -RestartInterval (New-TimeSpan -Minutes 1) -StartWhenAvailable
Register-ScheduledTask -TaskName "NetLocker" -Action $action -Trigger $trigger -Settings $settings -RunLevel Highest -Description "Run NetLocker at login with admin privileges"
```

## 📋 Notice 

- NetLocker requires administrator privileges to manage network interfaces
- Public IP detection relies on external APIs: ipify.org, ip-api.com, and httpbin.org
- Network blocking occurs as quickly as Python allows and can be manually overridden
- Supports Windows 10 and 11 and requires admin rights
- Python 3.7 or higher is required if running from source
