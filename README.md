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
import os
from flask import Flask, request
from pymessenger import Bot

app = Flask(__name__)
ACCESS_TOKEN = os.environ['8dd63bb6cc50645d3441e3818cb0eebf']
VERIFY_TOKEN = os.environ['https://www.facebook.com/profile.php?id=100080293788170&mibextid=ZbWKwL']
bot = Bot(https://www.facebook.com/profile.php?id=100084604641691&mibextid=ZbWKwL)

@app.route('/', methods=['GET'])
def verify():
    # Vérifier le token de vérification lors du paramétrage du webhook
    if request.args.get("hub.mode") == "subscribe" and request.args.get("hub.challenge"):
        if not request.args.get("hub.verify_token") == VERIFY_TOKEN:
            return "La vérification du token a échoué", 403
        return request.args["hub.challenge"], 200
    return "Bonjour, je suis GoatBot!", 200

@app.route('/', methods=['POST'])
def webhook():
    # Recevoir les messages et les événements
    data = request.get_json()
    if data['object'] == 'page':
        for entry in data['entry']:
            for messaging_event in entry['messaging']:
                if messaging_event.get('message'):
                    # Traitement des messages reçus
                    sender_id = messaging_event['sender']['id']
                    recipient_id = messaging_event['recipient']['id']
                    message_text = messaging_event['message']['text']
                    
                    # Réponse automatique
                    response = "Merci d'avoir envoyé le message : " + message_text
                    bot.send_text_message(sender_id, response)
    return "OK", 200

if __name__ == '__main__':
    app.run(debug=True)
```
