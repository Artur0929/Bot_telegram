# Bot_telegram

import telebot
import config as cfg
from telebot import types
from pyqiwip2p import QiwiP2P

bot = telebot.TeleBot(cfg.Token)
p2p = QiwiP2P(auth_key=cfg.QIWI_TOKEN)

bill1 = p2p.bill(amount=1, lifetime=1,comment="1")
bill2 = p2p.bill(amount=2, lifetime=2,comment="2")
bill3 = p2p.bill(amount=3, lifetime=3,comment="3")
bill4 = p2p.bill(amount=4, lifetime=4,comment="4")
bill5 = p2p.bill(amount=5, lifetime=5,comment="5")
bill6 = p2p.bill(amount=6, lifetime=6,comment="6")
bill7 = p2p.bill(amount=7, lifetime=7,comment="7")

@bot.message_handler(commands=['start'])
def start(message):
    mess = 'Доброе время суток🙍‍♀ \n\nБлагодаря этому боту, вы сможете приобрести\nподписку на запрещённые каналы автономно и иметь\nвсегда актуальную ссылку на купленные каналы. 🔞 '
    bot.send_message(message.chat.id, mess, parse_mode='html')
    markup2 = types.ReplyKeyboardMarkup(resize_keyboard=True,row_width=1)
    buttons1 = types.KeyboardButton('Приобрести доступ')
    buttons2 = types.KeyboardButton('Помощь проекту')
    buttons3 = types.KeyboardButton('Стутус подписки')
    markup2.add(buttons1,buttons2,buttons3)
    bot.send_message(message.chat.id,'Главное меню:',reply_markup=markup2)


@bot.message_handler(content_types = ['text'])
def oplata(message):
    if message.chat.type == 'private':
        if message.text == 'Приобрести доступ':
            oplata1 = types.InlineKeyboardMarkup()
            oplata1.add(types.InlineKeyboardButton("| 1 руб.",url=bill1.pay_url))#1
            oplata1.add(types.InlineKeyboardButton(" | 2 руб.", url=bill2.pay_url))#2
            oplata1.add(types.InlineKeyboardButton(" | 3 руб.",url=bill3.pay_url))#3
            oplata1.add(types.InlineKeyboardButton("| 4 руб.", url=bill4.pay_url))#4
            oplata1.add(types.InlineKeyboardButton("| 5 руб.", url=bill5.pay_url))#5
            oplata1.add(types.InlineKeyboardButton("| 6 руб.", url=bill7.pay_url))#6
            bot.send_message(message.chat.id,'ℹ️ Чтобы ознакомиться с каналом, выбери необходимый, нажав на соответствующую кнопку',reply_markup=oplata1)
        elif message.text == 'Помощь проекту':
            oplata2 = types.InlineKeyboardMarkup()
            oplata2.add(types.InlineKeyboardButton("Донат", url="https://my.qiwi.com/Artur-BpgW6UoIli"))
            bot.send_message(message.chat.id,'Спасибо)))',reply_markup=oplata2)


bot.polling(none_stop=True)
