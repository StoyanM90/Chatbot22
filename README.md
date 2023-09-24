import operator
import random
import datetime
import pickle

statistics_file = "statistics.pkl"

try:
    with open(statistics_file, "rb") as file:
        statistics = pickle.load(file)
except FileNotFoundError:
    statistics = {
        "People Count": 0,
        "Last User": None,
        "Name Counts": {},
        "Birthdate Counts": {},
        "Birthplace Counts": {},
        "Profession Counts": {},
        "Gender Counts": {"m": 0, "f": 0},
    }

users_data = []

print("HELLO! MY NAME IS MEGABOT3000. NOW I WILL ASK YOU SOME QUESTIONS.")

add_new_user = True

while add_new_user:
    questions = [
        "What is your name?",
        "When is your birthday?",
        "Which city were you born in?",
        "Which city do you currently live in?",
        "What is your profession?",
        "Are you male or female? (m/f)",
    ]

    answers = [input(question + " ") for question in questions]
    name, birthdate, birth_city, current_city, profession, gender = answers

    person = {
        "Name": name,
        "Birthdate": birthdate,
        "Birthplace": birth_city,
        "Current city": current_city,
        "Profession": profession,
        "Gender": gender
    }

    users_data.append(person)

    statistics["People Count"] += 1
    statistics["Last User"] = person["Name"]

    for key in ["Name", "Birthdate", "Birthplace", "Profession"]:
        value = person[key]
        if value in statistics[f"{key} Counts"]:
            statistics[f"{key} Counts"][value] += 1
        else:
            statistics[f"{key} Counts"][value] = 1

    gender = person["Gender"]
    statistics["Gender Counts"][gender] += 1

    print("Thank you for sharing your information!")

    response = input("Do you want to start a new user or exit? (new/exit): ").lower()
    if response != "new":
        add_new_user = False

with open(statistics_file, "wb") as file:
    pickle.dump(statistics, file)

print("\nStatistics:")
print("Number of people who have used the code:", statistics["People Count"])
print("Last user to use the code:", statistics["Last User"])

print("Number of people per name:")
for name, count in statistics["Name Counts"].items():
    print(f"{name}: {count} people")

print("Number of people per birthdate:")
for birthdate, count in statistics["Birthdate Counts"].items():
    print(f"{birthdate}: {count} people")

print("Number of people per birthplace:")
for birthplace, count in statistics["Birthplace Counts"].items():
    print(f"{birthplace}: {count} people")

print("Number of people per profession:")
for profession, count in statistics["Profession Counts"].items():
    print(f"{profession}: {count} people")

print("Number of people per gender:")
for gender, count in statistics["Gender Counts"].items():
    print(f"{gender}: {count} people")
