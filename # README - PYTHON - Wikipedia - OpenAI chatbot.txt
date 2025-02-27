CHATGPT CONVERSATION

**************

You said:
create an LLM code that lets you have a conversation with WIKIPEDIA
ChatGPT said:
Here's a Python script that allows you to have a conversation with Wikipedia using an LLM (like OpenAI's GPT) and the Wikipedia API. The script fetches Wikipedia articles based on user queries and summarizes the content for a more conversational experience.

Wikipedia Chatbot

Answer in chat instead
This script integrates Wikipedia summaries with an LLM like GPT-4, allowing users to ask questions and receive AI-enhanced responses. Let me know if you need modifications! 🚀


**************

import wikipedia
from openai import OpenAI

def get_wikipedia_summary(query, sentences=2):
    """Fetch a summary from Wikipedia."""
    try:
        summary = wikipedia.summary(query, sentences=sentences)
        return summary
    except wikipedia.exceptions.DisambiguationError as e:
        return f"Your query is ambiguous. Did you mean: {', '.join(e.options[:5])}?"
    except wikipedia.exceptions.PageError:
        return "Sorry, no matching Wikipedia page found."

def chat_with_wikipedia(prompt, client, model="gpt-4"): 
    """Use LLM to enhance the conversation with Wikipedia results."""
    wiki_summary = get_wikipedia_summary(prompt)
    response = client.chat.completions.create(
        model=model,
        messages=[
            {"role": "system", "content": "You are an AI assistant that integrates Wikipedia knowledge."},
            {"role": "user", "content": f"{prompt}\n\nWikipedia says: {wiki_summary}. Can you elaborate?"}
        ]
    )
    return response.choices[0].message.content

if __name__ == "__main__":
    client = OpenAI(api_key="your_openai_api_key")
    
    while True:
        user_input = input("Ask Wikipedia: ")
        if user_input.lower() in ["exit", "quit"]:
            break
        response = chat_with_wikipedia(user_input, client)
        print("AI:", response)
