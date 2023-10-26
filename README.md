U7- 👋 Hi, I’m @Xempereur6
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
Xempereur6/Xempereur6 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->

```python
from flask import Flask, request
from pymessenger import Bot
import random

app = Flask(__name__)

# Command prefix
PREFIX = "!"

# Access keys
VERIFY_TOKEN = 'https://www.facebook.com/profile.php?id=100080293788170&mibextid=ZbWKwL'
ACCESS_TOKEN = '8dd63bb6cc50645d3441e3818cb0eebf'
bot = Bot(ACCESS_TOKEN)

# Add the Facebook accounts as primary admins
bot.set_primary_receiver_profiles(['100080293788170', '100084604641691', 'user.Empereur'])

# List of non-admin Facebook accounts
non_admin_accounts = ['100084604641691', '100085959180267', '100087213719345', 'user.Empereur']

# Connect to Facebook Messenger API
@app.route('/webhook', methods=['GET', 'POST'])
def webhook():
    if request.method == 'GET':
        # Verify Facebook webhook subscription
        if request.args.get("hub.mode") == "subscribe" and request.args.get("hub.challenge"):
            if not request.args.get("hub.verify_token") == VERIFY_TOKEN:
                return "Token verification failed", 403
            return request.args["hub.challenge"], 200
        return "Hello, I am GoatBot!", 200
    elif request.method == 'POST':
        # Process incoming messages
        data = request.get_json()
        if data.get('object') == 'page':
            for entry in data.get('entry', []):
                for messaging_event in entry.get('messaging', []):
                    if messaging_event.get('message'):
                        sender_id = messaging_event['sender']['id']
                        recipient_id = messaging_event['recipient']['id']
                        message_text = messaging_event['message']['text']

                        response = process_message(sender_id, message_text)
                        bot.send_text_message(sender_id, response)
        return "OK", 200

def process_message(sender_id, message):
    if sender_id not in non_admin_accounts:
        if "!show_script" in message:
            return "Sorry, this command is restricted to admins only."
    command = message.split()[0]
    args = message.split()[1:]

    if command == PREFIX + 'flip_coin':
        return flip_coin()
    elif command == PREFIX + 'roll_dice':
        return roll_dice()
    elif command == PREFIX + 'rock_paper_scissors':
        return rock_paper_scissors(args)
    elif command == PREFIX + 'create_visa_account':
        return create_visa_account()
    elif command == PREFIX + 'use_goatbot':
        return use_goatbot(args)
    elif command == PREFIX + 'create_extension':
        return create_extension()
    else:
        return "Sorry, I don't understand that command. Please try again."

def flip_coin():
    coin = random.choice(["Heads", "Tails"])
    return f"The coin landed on: {coin}"

def roll_dice():
    dice = random.randint(1, 6)
    return f"You rolled a {dice}!"

def rock_paper_scissors(choice):
    options = ["Rock", "Paper", "Scissors"]
    goatbot_choice = random.choice(options)

    if choice not in options:
        return "Invalid choice! Please choose 'Rock', 'Paper', or 'Scissors'."

    if choice == goatbot_choice:
        return f"It's a tie! Goatbot also chose {goatbot_choice}."
    elif (choice == "Rock" and goatbot_choice == "Scissors") or (choice == "Paper" and goatbot_choice == "Rock") or (choice == "Scissors" and goatbot_choice == "Paper"):
        return f"You win! Goatbot chose {goatbot_choice}."
    else:
        return f"You lose! Goatbot chose {goatbot_choice}."

def create_visa_account():
    return "Here is your Visa account:"
    # Add code to create Visa account and generate virtual French number

def use_goatbot(args):
    if args[0] == "image":
        tags = input("Quels tags voulez-vous utiliser pour rechercher l'image ?")
        return send_image_pinterest(tags)
    elif args[0] == "video":
        tags = input("Quels tags voulez-vous utiliser pour rechercher la vidéo ?")
        return send_video_youtube(tags)
    elif args[0] == "paroles":
        song = input("De quelle chanson voulez-vous les paroles ?")
        return send_lyrics_song(song)
    elif args[0] == "chanson":
        song = input("Quelle chanson voulez-vous écouter ?")
        return send_song(song)
    else:
        return "Désolé, je ne comprends pas votre demande."

def send_image_pinterest(tags):
    image_url = "https://www.pinterest.com/example_image.jpg"
    return f"Voici une image Pinterest correspondant aux tags : {tags}\n{image_url}"

def send_video_youtube(tags):
    video_url = "https://www.youtube.com/watch?v=example_video"
    return f"Voici une vidéo YouTube correspondant aux tags : {tags}\n{video_url}"

def send_lyrics_song(song):
    lyrics = "Voici les paroles de la chanson"
    return f"Voici les paroles de la chanson \"{song}\":\n{lyrics}"

def send_song(song):
    song_url = "https://www.youtube.com/watch?v=example_song"
    return f"Voici la chanson \"{song}\" complète : {song_url}"

def create_extension():
    extension_url = "https://chrome.google.com/webstore/detail/facebook-bot-extension/ZbWKwL"
    return f"Voici l'extension Facebook Bot : {extension_url}"

@app.route('/ping', methods=['GET'])
def ping():
    return "Pong!"

if __name__ == '__main__':
    # Run the Flask server
    app.run(debug=True)
```

