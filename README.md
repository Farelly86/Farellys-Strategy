import os
import logging
from telegram import Update
from telegram.ext import Updater, CommandHandler, CallbackContext

logging.basicConfig(
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    level=logging.INFO
)
logger = logging.getLogger(__name__)

TOKEN = os.getenv('8021127611:AAHNz8QTM6X4dztySjrikh0yJV5zhCKxqFE')

signals_active = False
signals_won = 0
signals_lost = 0

def start(update: Update, context: CallbackContext):
    user = update.effective_user
    update.message.reply_text(
        f"Ciao {user.first_name}! Benvenuto nel bot segnali.\n"
        "Questo bot invia segnali Binary OTC per Quotex.\n"
        "Usa il comando /signals per ricevere segnali.\n"
        "Usa /stop per fermare i segnali.\n"
        "Usa /dashboard per vedere statistiche.\n"
        "Usa /feedback per inviare un commento."
    )

def signals(update: Update, context: CallbackContext):
    global signals_active
    signals_active = True
    update.message.reply_text("Segnali attivati! Riceverai i segnali appena disponibili.")

def stop(update: Update, context: CallbackContext):
    global signals_active
    signals_active = False
    update.message.reply_text("Segnali disattivati.")

def dashboard(update: Update, context: CallbackContext):
    update.message.reply_text(
        f"Statistiche segnali oggi:\n"
        f"Vinti: {signals_won}\n"
        f"Persi: {signals_lost}"
    )

def feedback(update: Update, context: CallbackContext):
    update.message.reply_text("Grazie per il tuo feedback!")

def main():
    updater = Updater(TOKEN)
    dispatcher = updater.dispatcher

    dispatcher.add_handler(CommandHandler("start", start))
    dispatcher.add_handler(CommandHandler("signals", signals))
    dispatcher.add_handler(CommandHandler("stop", stop))
    dispatcher.add_handler(CommandHandler("dashboard", dashboard))
    dispatcher.add_handler(CommandHandler("feedback", feedback))

    updater.start_polling()
    updater.idle()

if __name__ == '__main__':
    main()
