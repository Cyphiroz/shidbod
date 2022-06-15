import os.path
import requests
from bs4 import BeautifulSoup
from datetime import datetime
import random
import wikipedia
import pywhatkit as kit
import subprocess as sp
import webbrowser

# Prints all the commands available to use
def help():
    print("    help: Prints all commands which can be used.")
    print("    weather: Finds the weather in any location.")
    print("    time: Finds the time.")
    print("    exit: Exits the program.")
    print("    password: Generates a password.")
    print("    hangman: Play hangman.")
    print("    cal: Opens calculator.")
    print("    hl: Guess a number between 1 and 100.")
    print("    ip: Finds your IP address.")
    print("    wiki: Finds information on any topic.")
    print("    yt: Finds the latest video from any youtuber or a specfic youtube video.")
    print("    joke: Finds a joke.")
    print("    advice: Finds an advice.")
    print("    web: Opens any website.")
    print("    quote: Finds a quote.")
    print("    date: Finds the date of any day.")
    print("    rps: Plays rock paper scissors with shidbod.")
    print("    google: Searches google for anything.")
    print("    8ball: Answers a question.")


# Gets the weather data needed
def weather(location):
    try:
        url = f'https://www.google.com/search?q=weather+{location.replace(" ", "")}'
        session = requests.Session()
        session.headers['User-Agent'] = "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/100.0.4896.127 Safari/537.36"
        html = session.get(url)

        soup = BeautifulSoup(html.text, 'html.parser')

        name = soup.find('div', attrs = {'id': 'wob_loc'}).text
        weather = soup.find('span', attrs = {'id': 'wob_dc'}).text
        temp = soup.find('span', attrs = {'id': 'wob_tm'}).text
        precip = soup.find('span', attrs = {'id': 'wob_pp'}).text

        print(f"    Weather in {name}: {weather}  Temperature: {temp} C  Precipitation: {precip}")
    except:
        print(">>> Invalid Location.")

# Finds the time 
def _time():
    now = datetime.now()
    current_time = now.strftime("%H:%M:%S")
    print("    Current Time is :", current_time)

# Generates a password
def password():
    length = random.randint(8, 30)

    word = []

    for i in range(length):
        let = "abcdefghijklmnopqrstuvwxyz1234567890!@#$%^&*()[]-?_+"
        rand_let = random.choice(let)
        if rand_let.isnumeric() == False:
            uper_lower = random.randint(1,2)
            if uper_lower == 1:
                rand_let = rand_let.upper()
        word.append(rand_let)

    string = ''.join(word)
    print("    "+string)

# Hangman
def hangman():
    if os.path.isfile("words.txt") == False:
        print(">>> words.txt not found in directory.")
    else:
        with open("words.txt", "r") as file:
            line = file.read()
            wordlist = line.split()

        word = wordlist[random.randint(0, len(wordlist)-1)]
        word = word.lower()
        secret = []

        for i in range(len(word)):
            secret.append("_")

        print("    "+" ".join(secret))

        word_true = False
        tries = 0

        while word_true != True:
            letter = str(input("    Guess a letter: "))
            if letter in word:
                for i in range(len(word)):
                    if word[i] == letter:
                        secret[i] = letter
                print("    "+" ".join(secret))

                if "_" not in secret:
                    word_true = True
                    print(">>> You win!")
            elif letter.isalpha() != True or len(letter) != 1:
                print(">>> Please enter a single letter.")
            else:
                tries += 1
                print("    Incorrect. You have", 6 - tries, "tries left.")
                if tries == 6:
                    print(f">>> You lose. The word was {word}.")
                    break

# Calculator
def calculator():
    sp.Popen("C:\\Windows\\System32\\calc.exe")

# Highlow game
def highlow():
    secret_num = random.randint(1, 100)

    tries = 0

    guess_true = False
    while (guess_true != True) and (tries != 6):
        guess = input("    I am thinking of a random number. Between 1 and 100. What is it? ")
        if guess.isnumeric() != True:
            print(">>> Please enter a number.")
        else:
            guess = int(guess)
            if guess < 1 or guess > 100:
                print(">>> Please enter a number between 1 and 100.")
            elif guess < secret_num:
                tries += 1
                print("    Too low. You have", 6 - tries, "tries left.")
            elif guess > secret_num:
                tries += 1
                print("    Too high. You have", 6 - tries, "tries left.")
            elif guess == secret_num:
                print(">>> Correct!")
                guess_true == True
    if tries == 6:
        print(f">>> You lose. The number was {secret_num}.")

# Finds your IP address
def ip():
    url = "https://api.ipify.org"
    session = requests.Session()
    session.headers['User-Agent'] = "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/100.0.4896.127 Safari/537.36"
    html = session.get(url)

    soup = BeautifulSoup(html.text, 'html.parser')

    ip = soup.text

    print(f"    Your IP address is: {ip}")

# Finds information on any topic
def wiki(topic):
    try:
        results = wikipedia.summary(topic, sentences=2)
        print(f"    {results}")
    except:
        print(">>> Invalid Topic. Please try rephrasing the topic you would like searched.")

# Finds a video on YouTube
def yt(video):
    try:
        kit.playonyt(video)
    except:
        print(">>> Invalid Video.")

# Finds a joke
def joke():
    headers = {
        'Accept': 'application/json'
    }
    res = requests.get("https://icanhazdadjoke.com/", headers=headers).json()
    print(f'    {res["joke"]}')

# Finds an advice
def advice():
    res = requests.get("https://api.adviceslip.com/advice").json()
    print(f"    {res['slip']['advice']}")

# Opens a website
def web(site):
    try:
        webbrowser.open(site)
    except:
        print(">>> Invalid Website.")

# Finds a quote
def quote():
    res = requests.get("https://api.quotable.io/random").json()
    print(f"    {res['content']} - {res['author']}")

# Validation Module - Checks if the date is possible
def validDate(date, month, year):
    if (month > 12) or month < 1:
        return 'Invalid'
    elif ((month == 1 or 3 or 5 or 7 or 8 or 10 or 12) and ((date > 32) or (date < 1))) or ((month == 4 or 6 or 9 or 11) and ((date > 31) or (date < 1))):
        return 'Invalid'
    elif ((month == 2) and (year % 4 != 0) and (date > 28)):
        return 'Invalid'
    elif ((month == 2) and (date > 29)):
        if ((year % 400 == 0) or ((year % 4 == 0) and (year % 100 != 0)) and (date == 29)):
            return 'Valid'
        else:
            return 'Invalid'
    else:
        return 'Valid'

# Finds the day of any date
def _date():
    print(">>> Please make the date, month and year in the dd/mm/yyyy format.")

    d = str(input('    What is the date? '))
    m = str(input('    What is the month? Please enter in the number form. '))
    y = str(input('    What year does this date take place? '))

    if ((len(d) != 2) or (len(m) != 2) or (len(y) != 4)) or ((d.isnumeric() == False) or (m.isnumeric() == False) or (y.isnumeric() == False)):
        print(">>> ERROR: Invalid Notation entered. Please make date in DD/MM/YYYY format.")
    # What happens if the date is valid or not
    else:   
        date = int(d)
        month = int(m)
        year = int(y)
        if validDate(date, month, year) == 'Invalid':
            # If not valid
            print(">>> ERROR: Invalid date entered.")

        elif validDate(date, month, year) == 'Valid':
            # Arrays for the month codes and the days
            months = [ 0, 3, 2, 5, 0, 3, 5, 1, 4, 6, 2, 4 ]
        days = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']

        if (month < 3) :
            year = year - 1

        year_code = year // 4
        cent_code = year // 100
        leap_year = year // 400
        month_code = months[month - 1]

        # Final formula and return of the day
        form = (year + year_code - cent_code + leap_year + month_code + date) % 7
        day = days[form - 1]
        print(f">>> {day}")

# Rock Paper Scissors
def rps():
    print(">>> Rock Paper Scissors")
    print(">>> Rock = 1, Paper = 2, Scissors = 3")
    print(">>> Please enter the number of the option you would like to choose.")
    choice = int(input("    1, 2, or 3? "))
    if choice == 1:
        print("    You chose Rock.")
    elif choice == 2:
        print("    You chose Paper.")
    elif choice == 3:
        print("    You chose Scissors.")
    else:
        print(">>> Invalid response.")
    
    comp = random.randint(1, 3)
    if comp == 1:
        print("    Computer chose Rock.")
    elif comp == 2:
        print("    Computer chose Paper.")
    elif comp == 3:
        print("    Computer chose Scissors.")
    
    if choice == comp:
        print("    It's a tie!")
    elif choice == 1 and comp == 2:
        print("    You lose!")
    elif choice == 1 and comp == 3:
        print("    You win!")
    elif choice == 2 and comp == 1:
        print("    You win!")
    elif choice == 2 and comp == 3:
        print("    You lose!")
    elif choice == 3 and comp == 1:
        print("    You lose!")
    elif choice == 3 and comp == 2:
        print("    You win!")

# 8ball
def eightball():
    question = input("    Question: ")
    answers = ["It is certain.", "It is decidedly so.", "Without a doubt.", "Yes - definitely.", "You may rely on it.", "As I see it, yes.", "Most likely.", "Outlook good.", "Yes.", "Signs point to yes.", "Reply hazy, try again.", "Ask again later.", "Better not tell you now.", "Cannot predict now.", "Concentrate and ask again.", "Don't count on it.", "My reply is no.", "My sources say no.", "Outlook not so good.", "Very doubtful."]
    print(f"    {random.choice(answers)}")

# Google anything
def google():   
    search = input("    What would you like to search for? ")
    webbrowser.open(f"https://www.google.com/search?q={search}")

print(">>> Welcome to shidbod")
print(">>> Type help for a list of commands.")

exit = False

while exit == False:
    command = input(">>> ")
    if command == "help":
        help()
    elif command == "weather":
        location = input("    Location: ")
        weather(location)
    elif command == "time":
        _time()
    elif command == "exit":
        sure = input("    Are you sure you want to exit? Y/N: ")
        if sure == "Y" or sure == "y":
            print(">>> Goodbye. shidbod exitting...")
            exit = True
    elif command == "password":
        password()
    elif command == "hangman":
        hangman()
    elif command == "cal":
        calculator()
    elif command == "hl":
        highlow()
    elif command == "ip":
        ip()
    elif command == "wiki":
        topic = input("    Topic: ")
        wiki(topic)
    elif command == "yt":
        video = input("    Youtube: ")
        yt(video)
    elif command == "joke":
        joke()
    elif command == "advice":
        advice()
    elif command == "web":
        site = input("    Website: ")
        web(site)
    elif command == "quote":
        quote()
    elif command == "date":
        _date()
    elif command == "rps":
        rps()
    elif command == "8ball":
        eightball()
    elif command == "google":
        google()
    else:
        print(">>> Invalid Command.")

quit()