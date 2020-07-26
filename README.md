# Jarvis_Desktop_Assistant
import pyttsx3  # pip install pyttsx3
import speech_recognition as sr  # pip install speechRecognition
import datetime
import wikipedia  # pip install wikipedia
import webbrowser
import os
import smtplib
from os import system

engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
# print(voices[1].id)
engine.setProperty('voice', voices[0].id)


def speak(audio):
    engine.say(audio)
    engine.runAndWait()


def wishMe():
    hour = int(datetime.datetime.now().hour)
    if hour >= 0 and hour < 12:
        print("Good Morining!")
        speak("Good Morning!")

    elif hour >= 12 and hour < 18:
        print("Good Afternoon!")
        speak("Good Afternoon!")

    else:
        print("Good Evening!")
        speak("Good Evening!")
    print("I am Jarvis Sir. Please tell me how may I help you")
    speak("I am Jarvis Sir. Please tell me how may I help you")


def takeCommand():
    # It takes microphone input from the user and returns string output

    r = sr.Recognizer()
    try:
        with sr.Microphone() as source:
            print("Listening...")
            r.pause_threshold = 1
            audio = r.listen(source)
    except:
        print("Please check that your microphone is inserted properly")
        speak("Please check that your microphone is inserted properly")

    try:
        print("Recognizing...")
        query = r.recognize_google(audio, language='en-in')
        print(f"User said: {query}\n")

    except:
        print("Say that again please...")
        return "None"
    return query


if __name__ == "__main__":
    wishMe()
    while True:
        query = takeCommand().lower()
        if 'Hello' in query.lower():
            speak("Hello Sir how can I help you")
        elif query == None:
            continue
        # Logic for executing tasks based on query
        elif 'wikipedia' in query.lower():
            speak('Searching Wikipedia...')
            query = query.replace("wikipedia", "")
            results = wikipedia.summary(query, sentences=2)
            speak("According to Wikipedia")
            speak(results)
            print(results)

        elif 'play music' in query.lower():
            music_dir = 'Music'
            songs = os.listdir(music_dir)
            print(songs)
            os.startfile(os.path.join(music_dir, songs[0]))

        elif 'the time' in query.lower():
            strTime = datetime.datetime.now().strftime("%H:%M:%S")
            speak(f"Sir, the time is {strTime}")

        elif 'open code' in query.lower():
            codePath = "C:\\Users\\Ankesh\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe"
            os.startfile(codePath)

        else:
            speak(f"opening {query}")
            webbrowser.open(f"{query}.com")
        system('cls')
