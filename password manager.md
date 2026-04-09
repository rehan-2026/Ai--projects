# Ai--projects
My first AI programs
password manager in Ai
import random
import string
import json
import os

FILE_NAME = "passwords.json"

# -------------------------------
# Generate Strong Password
# -------------------------------
def generate_password(length=10):
    lower = random.choice(string.ascii_lowercase)
    upper = random.choice(string.ascii_uppercase)
    digit = random.choice(string.digits)
    symbol = random.choice(string.punctuation)

    all_chars = string.ascii_letters + string.digits + string.punctuation
    remaining = [random.choice(all_chars) for _ in range(length - 4)]

    password_list = [lower, upper, digit, symbol] + remaining
    random.shuffle(password_list)

    return ''.join(password_list)

# -------------------------------
# Load Data
# -------------------------------
def load_data():
    if not os.path.exists(FILE_NAME):
        return {}
    
    with open(FILE_NAME, "r") as file:
        return json.load(file)

# -------------------------------
# Save Data
# -------------------------------
def save_data(data):
    with open(FILE_NAME, "w") as file:
        json.dump(data, file, indent=4)

# -------------------------------
# Add New Password
# -------------------------------
def add_password():
    website = input("Enter website: ")
    username = input("Enter username: ")
    password = generate_password()

    data = load_data()

    data[website] = {
        "username": username,
        "password": password
    }

    save_data(data)

    print("✅ Password saved successfully!")
    print("Generated Password:", password)

# -------------------------------
# View Saved Passwords
# -------------------------------
def view_passwords():
    data = load_data()

    if not data:
        print("No passwords saved yet.")
        return

    for website, info in data.items():
        print("\n🌐 Website:", website)
        print("👤 Username:", info["username"])
        print("🔑 Password:", info["password"])

# -------------------------------
# Main Menu
# -------------------------------
def main():
    while True:
        print("\n===== PASSWORD MANAGER =====")
        print("1. Add Password")
        print("2. View Passwords")
        print("3. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            add_password()
        elif choice == "2":
            view_passwords()
        elif choice == "3":
            print("Goodbye 👋")
            break
        else:
            print("Invalid choice!")

# Run program
main()
