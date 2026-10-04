# Omarchy Debug Log - Symlink Attack (CWE-59 / CWE-377)

## Summary
File: bin/omarchy-debug
Vulnerable line: LOG_FILE="/tmp/omarchy-debug.log"
Type: Insecure Temporary File

## Description
The script writes to fixed predictable path in world-writable /tmp without mktemp or O_EXCL. Local attacker can pre-create symlink at /tmp/omarchy-debug.log -> ~/.bashrc and victim's debug run overwrites it.

## PoC
ln -s ~/.bashrc /tmp/omarchy-debug.log
omarchy debug --print
# .bashrc overwritten with system logs

## Fix
LOG_FILE=$(mktemp /tmp/omarchy-debug.XXXXXX.log)
trap 'rm -f "$LOG_FILE"' EXIT

## Impact
CVSS 6.3 - Arbitrary file overwrite

## Reporter
Ibrahim Babagana Masba - Security Researcher, Maiduguri
