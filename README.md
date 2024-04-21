import sqlite3

from telebot import TeleBot

token = '7068166422:AAFUojft8WH8bkRNqcge9l-yrHt2tCnr6eQ'
bot = TeleBot(token)



in_system = False


class User:
    def __init__(self, id='', login='', password=''):
        self.id = id
        self.login = login
        self.password = password


registered_user = User()


@bot.message_handler(commands=['start'])
def start(message):
    user_id = message.from_user.id
    print(user_id)
    connection = sqlite3.connect('database.db')
    cursor = connection.cursor()
    cursor.execute('CREATE TABLE IF NOT EXISTS users (id INTEGER PRIMARY KEY,'
                   'user_id INTEGER) ')
    cursor.execute('CREATE TABLE IF NOT EXISTS fishes(id INTEGER PRIMARY KEY,name VARCHAR(15),description VARCHAR(150))')
    cursor.execute('''INSERT INTO fishes (name,description) VALUES ('Сом обыкновенный','Сом (или сом обыкновенный) считается самым крупным представителем рыб, обитающим на территории Беларуси. Популяция немногочисленная. Рыба в основном встречается на крупных озерах и водохранилищах, а также средних и крупных реках. Средняя длина тела сома составляет 1 м при весе 8-10 кг. Таких параметров он достигает к 10 годам. В 4-5-летнем возрасте антропометрия практически в два раза меньше. Известны случаи поимки в Беларуси особо крупных представителей до 100 кг. По форме тела чем-то схож с налимом. Однако при детальном сравнении становится очевидным разница более сплюснутой головы, слегка выступающей вперед нижней челюсти и, само собой, более крупными размерами взрослых особей. Окрас может отличаться, зависит от поры года, а также условий обитания. От темно-зеленого до практически черного. В целом более молодые особи светлее. Иногда встречаются альбиносы. Рыба держится особняком. Предпочитает передвигаться по глубинным участкам. Коряги, ямы, омуты и корни деревьев – излюбленные места обитания.Особенности ловли сома в Беларуси</b>\n\nОбыкновенный сом питается в основном другой более мелкой рыбой. Поэтому и наживка должна подбираться соответствующая. Как и в случае с налимом лучше всего ловится весной сразу после нереста. Именно тогда наблюдается наибольший жор у рыбы. В это время рыба активно принимает наживку в любое время суток. Летом рекомендуется выходить на рыбалку в поздневечерние или ночные часы. Отлично, если погода пасмурная или даже ненастная. В качестве снасти можно использовать донную или поплавочную удочку. Для приманки рекомендуется использовать пиявку, мальков, лягушонка (живые или мертвые особи)'),''')
    connection.commit()

    cursor.execute('SELECT * FROM users')
    users = cursor.fetchall()

    for user in users:
        if user[1] == user_id:
            bot.send_message(message.chat.id, 'вы не вошли в систему')
            break
    else:
        cursor.execute(f"INSERT INTO users (user_id)VALUES('{user_id}')" )
        connection.commit()
        bot.send_message(message.chat.id, 'вы вошли в систему')

    cursor.close()
    connection.close()
