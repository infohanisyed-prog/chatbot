from groq import Groq, AuthenticationError, RateLimitError, APIError
import pyttsx3

API_KEY = "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"

client = Groq(api_key=API_KEY)

engine = pyttsx3.init()



messages = [
    {"role": "system", "content": "You are a hani's helpful assistant."}
]

while True:
    user_message = input("You: ")

    if user_message.lower() == "exit":
        print("Chatbot: Goodbye!")
        break
    if user_message.lower() == "stop":
      engine.stop()


    messages.append({"role": "user", "content": user_message})

    try:
        response = client.chat.completions.create(
            model="openai/gpt-oss-120b",
            messages=messages,
        )

        reply = response.choices[0].message.content
        print("AI:", reply)
        engine.say(reply)
        engine.runAndWait()
        messages.append({"role": "assistant", "content": reply})

    except AuthenticationError:
        print("Chatbot: Your Groq API key is invalid.")

    except RateLimitError:
        print("Chatbot: Rate limit hit. Try again in a moment.")

    except APIError as e:
        print("Chatbot: API error:", e)

    except Exception as e:
        print("Chatbot: Something went wrong.")
        print("Error:", e)
        
