## Development and Deployment of a 'Chat with LLM' Application Using the Gradio Blocks Framework

### AIM:
To design and deploy a "Chat with LLM" application by leveraging the Gradio Blocks UI framework to create an interactive interface for seamless user interaction with a large language model.

### PROBLEM STATEMENT:
Develop a user-friendly chatbot application powered by a large language model (LLM) that can assist users by generating natural, conversational responses. The system should support real-time interaction, maintain chat history, and allow smooth user input within a responsive Gradio Blocks interface.

### DESIGN STEPS:

### STEP 1:
Set up the working environment by installing required Python libraries such as gradio, dotenv, and the LLM client. Load the API key using environment variables to establish a secure connection with the Large Language Model.

### STEP 2:
Develop the chat response function that formats the user input and chat history, sends it to the LLM, and retrieves the generated response. This function serves as the core logic of the chatbot.

### STEP 3:
Design the Gradio Blocks interface by creating components such as a Chatbot window, a Textbox for user queries, and a Submit button. Connect these components with the chatbot function to enable real-time interaction.

### STEP 4:
Deploy the application using Gradio's launch function. Test the chatbot with various inputs to verify smooth communication, response accuracy, and proper UI functionality.

### PROGRAM:
```
import os
import gradio as gr
from dotenv import load_dotenv
from text_generation import Client

load_dotenv()

HF_API_KEY = os.getenv("HF_API_KEY")
HF_API_FALCON_BASE = os.getenv("HF_API_FALCOM_BASE")

if HF_API_KEY is None or HF_API_FALCON_BASE is None:
    raise ValueError("❗ Set HF_API_KEY and HF_API_FALCOM_BASE in your .env file")

client = Client(
    HF_API_FALCON_BASE,
    headers={"Authorization": f"Basic {HF_API_KEY}"},
    timeout=120
)

def format_prompt(message, history):
    prompt = ""
    for user, bot in history:
        prompt += f"User: {user}\nAssistant: {bot}\n"
    prompt += f"User: {message}\nAssistant:"
    return prompt

def respond(message, history):
    prompt = format_prompt(message, history)

    bot_reply = client.generate(
        prompt,
        max_new_tokens=200,
        stop_sequences=["User:"]
    ).generated_text

    history.append((message, bot_reply))
    return "", history


with gr.Blocks() as demo:
    gr.Markdown("## 🤖 Chat with Falcon LLM")

    chatbot = gr.Chatbot(height=300)
    msg = gr.Textbox(label="Type your message...")
    btn = gr.Button("Send")
    clear = gr.ClearButton([msg, chatbot])

    btn.click(respond, inputs=[msg, chatbot], outputs=[msg, chatbot])
    msg.submit(respond, inputs=[msg, chatbot], outputs=[msg, chatbot])

demo.launch(share=False)
```
### OUTPUT:

<img width="1032" height="560" alt="EXP 8" src="https://github.com/user-attachments/assets/0a3a632c-8cbd-4f97-9b9e-063ceab639ef" />

<img width="1032" height="561" alt="EXP 8 2" src="https://github.com/user-attachments/assets/033a345a-c1a9-4c05-9a9f-9a49b0429d86" />

### RESULT:

The "Chat with LLM" application was successfully developed and deployed using the Gradio Blocks framework. It allows users to interact seamlessly with a large language model through a clean, responsive chat interface. The system effectively maintains chat history, provides real-time responses, and ensures a smooth conversational experience.
