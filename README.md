# UDP-Chat
##1st Step
```bash
sudo apt install python3
```

##Make a new file like "file.py" and copy this script , paste it. 
##Note it only run in the same network.
```bash
import socket
import threading
from datetime import datetime
import os

# --- COLORS ---
GREEN = '\033[92m'
CYAN = '\033[96m'
YELLOW = '\033[93m'
RESET = '\033[0m'

# --- LOGO SECTION ---
def print_logo():
    # Clears the screen for a fresh start
    os.system('clear' if os.name != 'nt' else 'cls')
    
    # You can change 'ROOT VOID' to your actual name/handle
    author_name = "ROOT VOID"
    
    logo = f"""
{CYAN}    #################################################
    #                                               #
    #   ██████╗██╗  ██╗ █████╗ ████████╗            #
    #  ██╔════╝██║  ██║██╔══██╗╚══██╔══╝            #
    #  ██║     ███████║███████║   ██║               #
    #  ██║     ██╔══██║██╔══██║   ██║               #
    #  ╚██████╗██║  ██║██║  ██║   ██║               #
    #   ╚═════╝╚═╝  ╚═╝╚═╝  ╚═╝   ╚═╝               #
    #                                               #
    #        UDP REAL-TIME CHAT v1.0                #
    #        {YELLOW}Author: {author_name}{CYAN}             #
    #################################################{RESET}
    """
    print(logo)

# --- CONFIGURATION ---
MY_IP = "192.168.x.x" #Enter your IP
MY_PORT = 9999
TARGET_IP = "192.168.x.x"  # Update this to your friend's/laptop IP
TARGET_PORT = 9999
LOG_FILE = "chat_history.txt"

# --- NETWORK SETUP ---
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
sock.bind((MY_IP, MY_PORT))

def save_to_log(sender, message):
    timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    with open(LOG_FILE, "a") as f:
        f.write(f"[{timestamp}] {sender}: {message}\n")

def receive_msg():
    while True:
        try:
            data, addr = sock.recvfrom(4096)
            decoded_msg = data.decode()
            save_to_log("Them", decoded_msg)
            # Incoming messages will appear in GREEN
            print(f"\n{GREEN}Incoming: {decoded_msg}{RESET}\nYou: ", end="")
        except:
            break

# --- EXECUTION ---
print_logo()
threading.Thread(target=receive_msg, daemon=True).start()

print(f"[*] {YELLOW}Listening on:{RESET} {MY_PORT}")
print(f"[*] {YELLOW}Target IP:{RESET}    {TARGET_IP}")
print(f"[*] {YELLOW}Log File:{RESET}     {LOG_FILE}")
print(f"{CYAN}" + "-" * 49 + f"{RESET}")

while True:
    msg = input("You: ")
    save_to_log("You", msg)
    sock.sendto(msg.encode(), (TARGET_IP, TARGET_PORT))
```
## After it; Type this for run the script....
```bash
python3 filename.py
```
