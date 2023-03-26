from flask import Flask, request, jsonify

app = Flask(__name__)

LOGIN_DETAILS = {}
ART_FORM_OPTIONS = {}

def login():
    while True:
        username = input("Enter your username: ")
        password = input("Enter your password: ")

        if username == LOGIN_DETAILS["username"] and password == LOGIN_DETAILS["password"]:
            print("Login successful!")
            return True
        else:
            print("Invalid username or password. Please try again.")

def choose_art_form():
    print("\nSelect an art form:")
    print("1. Music")
    print("2. Dance")
    print("3. Spoken word")
    print("4. Regional theatricals")

    art_form = get_valid_input("Enter a number: ", int, lambda x: 1 <= x <= 4)

    if art_form == 1:
        return view_or_buy("music")
    elif art_form == 2:
        return view_or_buy("dance")
    elif art_form == 3:
        return view_or_buy("spoken word")
    elif art_form == 4:
        return view_or_buy("regional theatricals")


def view_or_buy(art_form_name):
    print(f"\nSelect an option for {art_form_name}:")
    print("1. View")
    print("2. Buy")
    print("3. Add new view or buy option")

    option = get_valid_input("Enter a number: ", int, lambda x: 1 <= x <= 3)

    if option == 1:
        return view_art_form(art_form_name)
    elif option == 2:
        return buy_art_form(art_form_name)
    elif option == 3:
        return add_option(art_form_name)

def add_option(art_form_name):
    option_name = input("Enter option name: ")
    option_price = get_valid_input("Enter option price: ", int)

    ART_FORM_OPTIONS[art_form_name]['view'].append(option_name)
    ART_FORM_OPTIONS[art_form_name]['buy'][option_name] = option_price

    print(f"Successfully added {option_name} to {art_form_name} options.")

    return go_back_option(art_form_name)

def view_art_form(art_form_name):
    view_options = ART_FORM_OPTIONS[art_form_name]["view"]
    if len(view_options) == 0:
        print(f"No options available for {art_form_name}.")
    else:
        print(f"View options for {art_form_name}:")
        for option in view_options:
            print("-", option)

    return go_back_option(art_form_name)

def buy_art_form(art_form_name):
    buy_options = ART_FORM_OPTIONS[art_form_name]["buy"]
    if len(buy_options) == 0:
        print(f"No options available for {art_form_name}.")
    else:
        print(f"Buy options for {art_form_name}:")
        for option in buy_options:
            print("-", option)

        option = input("Enter name of option to see buy price: ")
        if option in buy_options:
            assert buy_options[option] == ART_FORM_OPTIONS[art_form_name]["buy"][option], "Inconsistent value."
            print(f"Buy price for {option} is {buy_options[option]}.")
        else:
            print("Invalid input. Please try again.")
            return buy_art_form(art_form_name)




'''data to pass to http://localhost:5000/data using postman:

{"login":{
    "username": "my_username",
    "password": "my_password"
},"art":{
    "music": {
        "view": ["rock", "jazz", "blues"],
        "buy": {
            "rock": 10,
            "jazz": 15,
            "blues": 20
        }
    },
    "dance": {
        "view": ["ballet", "tap", "salsa"],
        "buy": {
            "ballet": 25,
            "tap": 20,
            "salsa": 30
        }
    },
    "spoken word": {
        "view": ["poetry", "storytelling"],
        "buy": {
            "poetry": 5,
            "storytelling": 8
        }
    },
    "regional theatricals": {
        "view": ["nautanki", "jatra"],
        "buy": {
            "nautanki": 12,
            "jatra": 18
        }
    }
}}

'''

