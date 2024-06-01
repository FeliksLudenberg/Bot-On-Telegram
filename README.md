#logic
from random import randint
import requests

class Pokemon:
    pokemons = {}
    # Инициализация объекта (конструктор)
    def __init__(self, pokemon_trainer):

        self.pokemon_trainer = pokemon_trainer   

        self.pokemon_number = randint(1,1000)
        self.img = self.get_img()
        self.name = self.get_name()
        self.hp = randint(50, 55)
        self.power = randint(10, 15)

        Pokemon.pokemons[pokemon_trainer] = self

    # Метод для получения картинки покемона через API
    def get_img(self):
        pass
    
    def attack(self, enemy):
        if enemy.hp > self.power:
            enemy.hp -= self.power
            return f"Сражение @{self.pokemon_trainer} с @{enemy.pokemon_trainer}"
        else:
            enemy.hp = 0
            return f"Победа @{self.pokemon_trainer} над @{enemy.pokemon_trainer}! "


    # Метод для получения имени покемона через API
    def get_name(self):
        url = f'https://pokeapi.co/api/v2/pokemon/{self.pokemon_number}'
        response = requests.get(url)
        if response.status_code == 200:
            data = response.json()
            return (data['forms'][0]['name'])
        else:
            return "Pikachu"


    # Метод класса для получения информации
    def info(self):
        return f'''Имя твоего покеомона: {self.name}
hp: {self.hp}
power: {self.power}'''

    # Метод класса для получения картинки покемона
    def show_img(self):
        url = f'https://pokeapi.co/api/v2/pokemon/{self.pokemon_number}'
        response = requests.get(url)
        if response.status_code == 200:
            data = response.json()
            return (data['sprites']['other']['official-artwork']['front_default'])
        else:
            return "errors"
        
    def show_back(self):
        url = f'https://pokeapi.co/api/v2/pokemon/{self.pokemon_number}'
        response = requests.get(url)
        if response.status_code == 200:
            data = response.json()
            return (data['sprites']['back_default'])
        else:
            return "errors" 
        
class Fighter(Pokemon):
    def attack(self, enemy):
        super_power = randint(0,150000000000000000)
        self.power += super_power
        result = super_power().attack(enemy)
        self.power -= super_power
        return result + f"\
    Боец применил супер-атаку силой:{super_power} "

class Hiller(Pokemon):
    def attack(self, enemy):
        super_doctor = randint(10, 20)
        self.hp += super_doctor
        result = super_doctor().hp
        return result + f"\
    Боец исцилил своё здаровье на:{super_doctor}"

class Sniper(Pokemon):
    def attack(self, enemy):
        sniper_attak = randint(1,2)
        if sniper_attak = 2:
            self.power += 30
            result = sniper_attak().attack(enemy)
        else:
            self.power - 15
            result = sniper_attak().attack(enemy)
            return result +f"\
        Снайпер сделал свой выстрел"

class Sage(Pokemon):
    def attack(self, enemy):

        return super().attack(enemy)
class Stalker(Pokemon):
    def attack(self, enemy):
        
        return super().attack(enemy)
        #main
        import telebot 
from config import token

from logic import Pokemon

bot = telebot.TeleBot(token) 

@bot.message_handler(commands=['go'])
def go(message):
    if message.from_user.username not in Pokemon.pokemons.keys():
        pokemon = Pokemon(message.from_user.username)
        bot.send_message(message.chat.id, pokemon.info())
        bot.send_photo(message.chat.id, pokemon.show_img())
    else:
        bot.reply_to(message, "Ты уже создал себе покемона")

@bot.message_handler(commands=['fight'])
def fight(message):
    if message.reply_to_message:
        pokemon_1 = Pokemon.pokemons[message.from_user.username]
        pokemon_2 = Pokemon.pokemons[message.reply_to_message.from_user.username]
        result = pokemon_1.attack(pokemon_2)        
        bot.send_photo(message.chat.id, pokemon.show_img())
    else:
        bot.reply_to(message, "Ответь на сообщение другого пользователя!")

@bot.message_handler(commands=['info'])
def info(message):
    if message.from_user.username in Pokemon.pokemons.keys():
        pok = Pokemon.pokemons[message.from_user.username]
        bot.send_message(message.chat.id, pok.info())
    else:
        bot.reply_to(message, "У тебя нет покемоши")

   @bot.message_handler(commands=['feed'])
def feed(message):
    if message.from_user.username in Pokemon.pokemons.keys():
        pok = Pokemon.pokemons[message.from_user.username]
        bot.send_message(message.chat.id, pokemon_1.feed())
    else:
        bot.reply_to(message, "У тебя нет покемоши")

bot.infinity_polling(none_stop=True)
