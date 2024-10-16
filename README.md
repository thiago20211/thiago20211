import requests
from flask import Flask, request

app = Flask(__name__)

# Configurações
INSTAGRAM_TOKEN = 'seu_token_instagram'
WHATSAPP_TOKEN = 'seu_token_whatsapp'
WHATSAPP_NUMBER = 'seu_numero_whatsapp'

def enviar_whatsapp(mensagem, numero_destinatario):
    url = f'https://api.twilio.com/2010-04-01/Accounts/seu_sid/messages.json'
    data = {
        'From': WHATSAPP_NUMBER,
        'To': numero_destinatario,
        'Body': mensagem
    }
    response = requests.post(url, data=data, auth=('seu_sid', 'seu_token_twilio'))
    return response.status_code

@app.route('/instagram-webhook', methods=['POST'])
def instagram_webhook():
    data = request.json
    mensagem = data['message']
    numero_destinatario = data['from']  # Assumindo que você extrai o número

    # Responder via WhatsApp
    enviar_whatsapp(mensagem, numero_destinatario)

    return 'Mensagem recebida e respondida.', 200

if __name__ == '__main__':
    app.run(port=5000)
