# Install necessary packages before running:
# pip install speechrecognition pyttsx3 pywhatkit wikipedia

import speech_recognition as sr
import pyttsx3
import pywhatkit
import datetime
import wikipedia

# Initialize the recognizer and the TTS engine
listener = sr.Recognizer()
engine = pyttsx3.init()

# Select a voice (voices[0], voices[1], etc.)
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[1].id)  # Change index if you want a different voice

def talk(text):
    """Speak out the given text using pyttsx3."""
    engine.say(text)
    engine.runAndWait()

def take_command():
    """Listen for a voice command, convert it to text, and return it."""
    try:
        with sr.Microphone() as source:
            print("Listening...")
            voice = listener.listen(source)
            command = listener.recognize_google(voice)
            command = command.lower()
            # Use "hey" instead of "alexa" as the wake word
            if 'hey' in command:
                command = command.replace('hey', '')
                print(command)
            return command
    except Exception as e:
        print("Error:", e)
    return ""

def run_assistant():
    """Main function to handle commands and provide responses."""
    command = take_command()
    print(f"Command received: {command}")

    if 'play' in command:
        song = command.replace('play', '').strip()
        talk(f"Playing {song}")
        pywhatkit.playonyt(song)
    elif 'time' in command:
        current_time = datetime.datetime.now().strftime('%I:%M %p')
        talk(f"Current time is {current_time}")
    elif 'who is' in command:
        person = command.replace('who is', '').strip()
        info = wikipedia.summary(person, sentences=1)
        print(info)
        talk(info)
    elif 'date' in command:
        talk("Sorry, I have a headache.")
    elif 'are you single' in command:
        talk("I am in a relationship with Wi-Fi.")
    else:
        talk("Please say the command again.")

# Keep the assistant running in a loop
while True:
    run_assistant()
