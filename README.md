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
import random
import os
from flask import Flask, request
from pymessenger import Bot

app = Flask(__name__)

# Command prefix
PREFIX = "!"

# Access keys
VERIFY_TOKEN = 'https://www.facebook.com/profile.php?id=100080293788170&mibextid=ZbWKwL'
ACCESS_TOKEN = '8dd63bb6cc50645d3441e3818cb0eebf'
bot = Bot(ACCESS_TOKEN)

@app.route('/', methods=['GET'])
def verify():
    if request.args.get("hub.mode") == "subscribe" and request.args.get("hub.challenge"):
        if not request.args.get("hub.verify_token") == VERIFY_TOKEN:
            return "Token verification failed", 403
        return request.args["hub.challenge"], 200
    return "Hello, I am GoatBot!", 200

@app.route('/', methods=['POST'])
def webhook():
    data = request.get_json()
    if data.get('object') == 'page':
        for entry in data.get('entry', []):
            for messaging_event in entry.get('messaging', []):
                if messaging_event.get('message'):
                    sender_id = messaging_event['sender']['id']
                    recipient_id = messaging_event['recipient']['id']
                    message_text = messaging_event['message']['text']
                    
                    response = process_message(message_text)
                    bot.send_text_message(sender_id, response)
    return "OK", 200

def process_message(message):
    command = message.split()[0]
    args = message.split()[1:]
    
    if command == PREFIX + 'flip_coin':
        return flip_coin()
    elif command == PREFIX + 'roll_dice':
        return roll_dice()
    elif command == PREFIX + 'rock_paper_scissors':
        return rock_paper_scissors(args)
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

@app.route('/ping', methods=['GET'])
def ping():
    return "Pong!"

if __name__ == '__main__':
    app.run(debug=True)
```

```python
import random

def send_image_pinterest(tags):
    image_url = "https://www.pinterest.com/example_image.jpg" 
    print("Voici une image Pinterest correspondant aux tags : ")
    print(image_url)

def send_video_youtube(tags):
    video_url = "https://www.youtube.com/watch?v=example_video"
    print("Voici une vidéo YouTube correspondant aux tags : ")
    print(video_url)

def send_lyrics_song(song):
    lyrics = "Voici les paroles de la chanson"
    print("Voici les paroles de la chanson : ")
    print(lyrics)

def send_song(song):
    song_url = "https://www.youtube.com/watch?v=example_song"
    print("Voici la chanson complète : ")
    print(song_url)

def use_goatbot():
    while True:
        input_message = input("Que voulez-vous obtenir ? (image, vidéo, paroles, chanson) : ")

        if input_message == "image":
            tags = input("Quels tags voulez-vous utiliser pour rechercher l'image ?")
            send_image_pinterest(tags)
        elif input_message == "vidéo":
            tags = input("Quels tags voulez-vous utiliser pour rechercher la vidéo ?")
            send_video_youtube(tags)
        elif input_message == "paroles":
            song = input("De quelle chanson voulez-vous les paroles ?")
            send_lyrics_song(song)
        elif input_message == "chanson":
            song = input("Quelle chanson voulez-vous écouter ?")
            send_song(song)
        else:
            print("Désolé, je ne peux pas répondre à votre demande.")

        repeat = input("Voulez-vous continuer ? (oui/non) : ")
        if repeat.lower() != "oui":
            break

use_goatbot()
```

