import telebot
from telebot import types

bot = telebot.TeleBot('6101691514:AAEJgSHr6KE9zpxfiWfCIFEqmHmjiBzDWs0')

other = types.KeyboardButton(text='ДРУГОЕ')

@bot.message_handler(commands=['start'])
def start(message):
    bot.send_message(message.chat.id, "Привет! Это бот от Lifecell для подборки тарифа именно для тебя. Со всеми тарифами ты можешь ознакомиться на сайте: https://www.lifecell.ua/uk/mobilnij-zvyazok/taryfy/vilniy-life-2021/")
    bot.send_message(message.chat.id, "Или хочешь пройти тест для выбора тарифа?")
    keyboard = types.ReplyKeyboardMarkup(row_width=1, resize_keyboard=True)
    yes_button = types.KeyboardButton(text='Пройти тест')
    keyboard.add(yes_button, other)
    bot.send_message(message.chat.id, 'Выбери вариант:', reply_markup=keyboard)

@bot.message_handler(func=lambda message: message.text == 'Пройти тест')
def handle_test(message):
    bot.send_message(message.chat.id, "Отлично! Введи данные для выбора тарифа.")
    bot.send_message(message.chat.id, "Сколько интернета вы желаете?")
    keyboard = types.ReplyKeyboardMarkup(row_width=2, resize_keyboard=True)
    unlimited_button = types.KeyboardButton(text='Безлимит')
    seven_gb_button = types.KeyboardButton(text='7 Gb')
    eight_gb_button = types.KeyboardButton(text='8 Gb')
    twenty_gb_button = types.KeyboardButton(text='20 Gb')
    thirty_gb_button = types.KeyboardButton(text='30 Gb')
    fifty_gb_button = types.KeyboardButton(text='50 Gb')
    keyboard.add(unlimited_button, seven_gb_button, eight_gb_button, twenty_gb_button, thirty_gb_button, fifty_gb_button, other)
    bot.send_message(message.chat.id, 'Выбери вариант:', reply_markup=keyboard)
    bot.register_next_step_handler(message, process_mob_internet)

def process_mob_internet(message):
    mob_internet = message.text
    bot.send_message(message.chat.id, "Сколько минут для иностранных номеров?")
    keyboard = types.ReplyKeyboardMarkup(row_width=2, resize_keyboard=True)
    no_button = types.KeyboardButton(text='Ненужно')
    greater_than_50_button = types.KeyboardButton(text='>50')
    less_than_50_button = types.KeyboardButton(text='<50')
    less_than_100_button = types.KeyboardButton(text='<100')
    less_than_500_button = types.KeyboardButton(text='<500')
    keyboard.add(no_button, greater_than_50_button, less_than_50_button, less_than_100_button, less_than_500_button, other)
    bot.send_message(message.chat.id, 'Выбери вариант:', reply_markup=keyboard)
    bot.register_next_step_handler(message, process_num_mint_an, mob_internet)

def process_num_mint_an(message, mob_internet):
    num_mint_an = message.text
    if num_mint_an.lower() == "ненужно":
        num_mint_an = None
    elif not num_mint_an.isdigit():
        bot.send_message(message.chat.id, "Некорректное значение для количества минут на иностранные номера.")
        return

    bot.send_message(message.chat.id, "Какая цена вам подходит?")
    keyboard = types.ReplyKeyboardMarkup(row_width=2, resize_keyboard=True)
    less_than_200_button = types.KeyboardButton(text='<200')
    greater_than_200_button = types.KeyboardButton(text='>200')
    less_than_500_button = types.KeyboardButton(text='<500')
    less_than_1000_button = types.KeyboardButton(text='<1000')
    keyboard.add(less_than_200_button, greater_than_200_button, less_than_500_button, less_than_1000_button, other)
    bot.send_message(message.chat.id, 'Выбери вариант:', reply_markup=keyboard)
    bot.register_next_step_handler(message, process_price, mob_internet, num_mint_an)

def process_price(message, mob_internet, num_mint_an):
    price = message.text
    bot.send_message(message.chat.id, "Сколько минут для украинских номеров?")
    keyboard = types.ReplyKeyboardMarkup(row_width=2, resize_keyboard=True)
    less_than_300_button = types.KeyboardButton(text='<300')
    less_than_500_button = types.KeyboardButton(text='<500')
    less_than_1000_button = types.KeyboardButton(text='<1000')
    keyboard.add(less_than_300_button, less_than_500_button, less_than_1000_button, other)
    bot.send_message(message.chat.id, 'Выбери вариант:', reply_markup=keyboard)
    bot.register_next_step_handler(message, process_num_mint_uk, mob_internet, num_mint_an, price)

def process_num_mint_uk(message, mob_internet, num_mint_an, price):
    num_mint_uk = message.text

    # Удаляем символы '<' и '>' из значения num_mint_uk
    num_mint_uk = num_mint_uk.strip('<>')

    # Проверяем, является ли значение num_mint_uk числом
    if not num_mint_uk.isdigit():
        bot.send_message(message.chat.id, "Некорректное значение для количества минут на украинские номера.")
        return

    # Оставшиеся действия
    platinum_life = {
        "name": "Platinum Life",
        "price": 450,
        "internet": "Безлимит",
        "Num_Mint_An": 50,
        "Num_Mint_Uk": 3000
    }

    school_life = {
        "name": "School Life",
        "price": 200,
        "internet": "10 Gb",
        "Num_Mint_Uk": 1000
    }

    smart_life_s = {
        "name": "Smart Life S",
        "price": 150,
        "internet": "5 Gb",
        "Num_Mint_Uk": 500
    }

    free_life = {
        "name": "Вільний Лайф",
        "price": 180,
        "internet": "Безліміт",
        "Num_Mint_Uk": 1600
    }

    custom_life_1 = {
        "name": "Кастом лайф 1",
        "price": 450,
        "internet": "40 Gb",
        "Num_Mint_Uk": 1000
    }

    custom_life_2 = {
        "name": "Кастом лайф 2",
        "price": 550,
        "internet": "100 Gb",
        "Num_Mint_Uk": 300
    }

    custom_life_3 = {
        "name": "Кастом лайф 3",
        "price": 150,
        "internet": "10 Gb",
        "Num_Mint_Uk": 100
    }
    # ... Остальные тарифы ...

    tariffs = [platinum_life, school_life, smart_life_s, free_life, custom_life_1, custom_life_2, custom_life_3]

    suitable_tariffs = []
    for tariff in tariffs:
        if "price" in tariff and tariff["price"] < int(price.strip('<>=')):
            if num_mint_an is None or (tariff["Num_Mint_Uk"] > int(num_mint_uk) and tariff.get("Num_Mint_An", float('inf')) > num_mint_an):
                suitable_tariffs.append(tariff)

    if len(suitable_tariffs) > 0:
        bot.send_message(message.chat.id, "Вот некоторые подходящие тарифы:")
        for tariff in suitable_tariffs:
            message_text = f"Тариф: {tariff['name']}\nЦена: {tariff['price']} грн\nИнтернет: {tariff['internet']}"
            if "Num_Mint_Uk" in tariff:
                message_text += f"\nМинуты на украинские номера: {tariff['Num_Mint_Uk']}"
            if "Num_Mint_An" in tariff:
                message_text += f"\nМинуты на иностранные номера: {tariff['Num_Mint_An']}"
            bot.send_message(message.chat.id, message_text)  # Move this line inside the for loop to send each tariff separately
        keyboard = types.InlineKeyboardMarkup()
        website_button = types.InlineKeyboardButton(text='Оформить тариф',
                                                    url='https://www.lifecell.ua/uk/mobilnij-zvyazok/taryfy/vilniy-life-2021/')
        keyboard.add(website_button)
        bot.send_message(message.chat.id, 'Вы можете оформить тариф на сайте Lifecell, нажав кнопку ниже:', reply_markup=keyboard)

    else:
        bot.send_message(message.chat.id, "К сожалению, подходящих тарифов не найдено.")

@bot.message_handler(func=lambda message: message.text == 'Нет')
def handle_no(message):
    bot.send_message(message.chat.id, "Хорошо, если захочешь пройти тест, напиши '/start'.")

@bot.message_handler(func=lambda message: message.text == 'ДРУГОЕ')
def handle_other(message):
    keyboard = types.ReplyKeyboardMarkup(row_width=3, resize_keyboard=True)
    start_button = types.KeyboardButton(text='Старт')
    stop_button = types.KeyboardButton(text='Создатели')
    website_button = types.KeyboardButton(text='Сайт')
    keyboard.add(start_button, stop_button, website_button)
    bot.send_message(message.chat.id, 'Выбери действие:', reply_markup=keyboard)

@bot.message_handler(func=lambda message: message.text == 'Старт')
def handle_start(message):
    # Вызываем команду /start
    bot.send_message(message.chat.id, "/start")

@bot.message_handler(func=lambda message: message.text == 'Создатели')
def handle_creators(message):
    bot.send_message(message.chat.id, "Бот создан командой Jarvis.15( #mause & @robxwork )")

@bot.message_handler(func=lambda message: message.text == 'Сайт')
def start(message):
    keyboard = types.InlineKeyboardMarkup()
    website_button = types.InlineKeyboardButton(text='Перейти', url='https://www.lifecell.ua/uk/mobilnij-zvyazok/taryfy/vilniy-life-2021/')
    keyboard.add(website_button)
    bot.send_message(message.chat.id, ' Если вы жилаете перейти на сайт Lifecell нажмите кнопку ниже' , reply_markup=keyboard)

bot.polling()
