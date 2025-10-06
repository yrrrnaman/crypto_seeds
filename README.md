# Realme Phone Wallet Recovery Instructions

## ⚠️ AUTHORIZED PENETRATION TESTING ONLY

This document provides instructions for conducting authorized security testing on Realme devices to locate potential cryptocurrency wallet artifacts.

## Prerequisites

1. **Legal Authorization**: Ensure you have written permission to test the device
2. **Device Access**: Physical access to the Realme phone with appropriate unlock credentials
3. **USB Debugging**: Enable Developer Options and USB Debugging on the device
4. **Computer Setup**: A computer with Python 3 installed

## Step-by-Step Instructions

### 1. Prepare the Realme Device
- Enable Developer Options:
  - Go to Settings > About Phone
  - Tap "Build Number" 7 times
- Enable USB Debugging:
  - Settings > Additional Settings > Developer Options
  - Toggle "USB Debugging" ON

### 2. Install Required Tools
Connect your Realme phone to computer via USB and install ADB:
```bash
# On Ubuntu/Debian
sudo apt install android-tools-adb

# On Windows
# Download Android SDK Platform Tools
```

### 3. Verify Connection
```bash
adb devices
# Should show your Realme device
```

### 4. Pull Data for Analysis
```bash
# Create working directory
mkdir realme_wallet_test && cd realme_wallet_test

# Pull common directories where wallets might be stored
adb pull /sdcard/ .
adb pull /data/data/com.android.browser/ ./browser_data/
```

### 5. Run Wallet Recovery Script
Save the Python script as `wallet_recovery.py` and run:
```bash
python3 wallet_recovery.py
```

### 6. Check Results
Look for `found.txt` in the same directory containing any discovered wallet artifacts.

## Common File Locations to Check

1. **Internal Storage**:
   - `/sdcard/Download/`
   - `/sdcard/Documents/`
   - `/sdcard/Android/data/`

2. **App-specific Directories**:
   - Crypto wallet app directories
   - Browser data directories
   - Notes app storage

## Legal Compliance

- Only perform these tests on devices you own or have explicit written authorization to test
- Document all findings appropriately
- Securely handle any sensitive information discovered
- Follow responsible disclosure practices if testing third-party devices

## Additional Notes

- Realme phones use ColorOS (based on Android) - file structure is similar to standard Android
- Some directories may require root access for full access
- Always disconnect device safely after testing
- Delete any temporary files created during testing

## Support

For questions about these instructions, refer to your authorized penetration testing documentation or contact your security team.
