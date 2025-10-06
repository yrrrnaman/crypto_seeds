# crypto_seeds
#!/usr/bin/env python3
"""
Cryptocurrency Wallet Recovery Script
For authorized penetration testing only
"""

import os
import hashlib
import binascii
import json
from pathlib import Path
import re

def search_common_wallet_locations():
    """Search common locations where wallet files might be stored"""
    common_paths = [
        os.path.expanduser("~/.bitcoin/wallet.dat"),
        os.path.expanduser("~/.ethereum/keystore/"),
        os.path.expanduser("~/AppData/Roaming/Bitcoin/wallet.dat"),
        "/Applications/Bitcoin-Qt.app/Contents/MacOS/wallet.dat"
    ]
    
    found_wallets = []
    
    for path in common_paths:
        if os.path.exists(path):
            found_wallets.append(path)
            print(f"[+] Found potential wallet: {path}")
            
    return found_wallets

def scan_for_seed_phrases(directory):
    """Scan text files for potential seed phrases"""
    seed_patterns = [
        r'\b([a-z]{3,10}\s+){11}[a-z]{3,10}\b',  # 12-word seed phrase pattern
        r'\b([a-z]{3,10}\s+){23}[a-z]{3,10}\b'   # 24-word seed phrase pattern
    ]
    
    found_seeds = []
    
    try:
        for root, dirs, files in os.walk(directory):
            for file in files:
                if file.endswith(('.txt', '.log', '.doc', '.docx')):
                    filepath = os.path.join(root, file)
                    try:
                        with open(filepath, 'r', encoding='utf-8', errors='ignore') as f:
                            content = f.read()
                            
                        for pattern in seed_patterns:
                            matches = re.findall(pattern, content, re.IGNORECASE)
                            if matches:
                                for match in matches:
                                    found_seeds.append((filepath, match))
                                    print(f"[+] Potential seed phrase found in: {filepath}")
                    except Exception as e:
                        continue
    except Exception as e:
        print(f"Error scanning directory: {e}")
        
    return found_seeds

def save_findings(findings, output_file="found.txt"):
    """Save all findings to a file"""
    try:
        with open(output_file, 'w') as f:
            f.write("=== Wallet Recovery Results ===\n\n")
            
            if findings['wallet_files']:
                f.write("Found Wallet Files:\n")
                for wallet in findings['wallet_files']:
                    f.write(f"- {wallet}\n")
                f.write("\n")
                
            if findings['seed_phrases']:
                f.write("Potential Seed Phrases:\n")
                for filepath, seed in findings['seed_phrases']:
                    f.write(f"- File: {filepath}\n")
                    f.write(f"  Seed: {seed}\n")
                f.write("\n")
                
            if findings['addresses']:
                f.write("Found Addresses:\n")
                for addr in findings['addresses']:
                    f.write(f"- {addr}\n")
                    
        print(f"[+] Results saved to {output_file}")
        return True
        
    except Exception as e:
        print(f"Error saving findings: {e}")
        return False

def main():
    """Main function for wallet recovery"""
    print("=== Authorized Cryptocurrency Wallet Recovery ===")
    print("This tool is for authorized penetration testing only.\n")
    
    findings = {
        'wallet_files': [],
        'seed_phrases': [],
        'addresses': []
    }
    
    # Search for wallet files
    print("[*] Searching for wallet files...")
    findings['wallet_files'] = search_common_wallet_locations()
    
    # Scan for seed phrases in user directory
    print("[*] Scanning for seed phrases...")
    home_dir = Path.home()
    findings['seed_phrases'] = scan_for_seed_phrases(str(home_dir))
    
    # Save findings
    if any(findings.values()):
        print("\n[*] Saving findings...")
        save_findings(findings)
    else:
        print("[-] No wallet artifacts found.")
        # Create empty file to indicate completion
        with open("found.txt", "w") as f:
            f.write("No wallet artifacts found during authorized penetration test.\n")

if __name__ == "__main__":
    main()
