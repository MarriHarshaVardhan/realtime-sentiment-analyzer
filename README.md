# realtime-sentiment-analyzer
Real-Time Sentiment Analysis Web App 🧐
Project Title
Real-Time Sentiment Analysis Web App

Description
This project features a simple, yet effective, web application built with Flask that performs real-time sentiment analysis on user-provided text. Leveraging the power of Hugging Face Transformers, the app can instantly classify text as positive, negative, or neutral, along with a confidence score. It's designed to run seamlessly in Google Colab, making it easy to set up and demonstrate.

Technologies Used
Flask: A lightweight Python web framework used for building the web application's front-end and back-end.

Hugging Face Transformers: A powerful library providing pre-trained models for Natural Language Processing (NLP), specifically used here for sentiment analysis.

Google Colab: A free cloud-based Jupyter notebook environment, perfect for developing and running Python projects with access to GPUs.

ngrok: A tunneling service used to expose the Flask application running in Colab to the public internet, enabling a live demo.

How to Run
Follow these steps to get the Real-Time Sentiment Analysis Web App up and running in your Google Colab environment:

Open Google Colab: Go to https://colab.research.google.com/ and create a new notebook.

Install Dependencies: In the first cell, run the following commands to install all required libraries:

Bash

!pip install flask
!pip install transformers
!pip install flask-ngrok
!pip install pyngrok


!ngrok authtoken 2tn9hLRFrAqqE1ZCUddIa4teUZ7_7CsDvidB6bJYhJDQC4tQG

Create app.py: In a new Colab cell

%%writefile app.py
from flask import Flask, render_template_string, request, jsonify
from transformers import pipeline

app = Flask(__name__)

sentiment_pipeline = pipeline("sentiment-analysis")

HTML_TEMPLATE = """
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Real-time Sentiment Analyzer</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        .container { max-width: 600px; margin: auto; padding: 20px; border: 1px solid #ccc; border-radius: 8px; }
        textarea { width: 100%; height: 150px; margin-bottom: 10px; padding: 10px; border: 1px solid #ddd; border-radius: 4px; }
        button { padding: 10px 15px; background-color: #007bff; color: white; border: none; border-radius: 4px; cursor: pointer; }
        button:hover { background-color: #0056b3; }
        #result { margin-top: 20px; font-weight: bold; }
        .positive { color: green; }
        .negative { color: red; }
        .neutral { color: gray; }
    </style>
</head>
<body>
    <div class="container">
        <h1>Real-time Sentiment Analyzer</h1>
        <p>Type some text below and get its sentiment analyzed instantly!</p>
        <textarea id="textInput" placeholder="Enter your text here..."></textarea>
        <button onclick="analyzeSentiment()">Analyze Sentiment</button>
        <div id="result"></div>
    </div>

    <script>
        async function analyzeSentiment() {
            const text = document.getElementById('textInput').value;
            if (!text.trim()) {
                document.getElementById('result').innerHTML = '';
                return;
            }

            const response = await fetch('/analyze', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ text: text })
            });
            const data = await response.json();
            const sentiment = data.sentiment[0];
            const label = sentiment.label;
            const score = sentiment.score;

            let resultDiv = document.getElementById('result');
            resultDiv.innerHTML = `Sentiment: <span class="${label.toLowerCase()}">${label}</span> (Score: ${score.toFixed(4)})`;
        }
        document.getElementById('textInput').addEventListener('keyup', analyzeSentiment);
    </script>
</body>
</html>
"""

@app.route('/')
def home():
    return render_template_string(HTML_TEMPLATE)

@app.route('/analyze', methods=['POST'])
def analyze():
    data = request.get_json()
    text = data.get('text', '')
    if text:
        sentiment = sentiment_pipeline(text)
        return jsonify({"sentiment": sentiment})
    return jsonify({"sentiment": []}), 400

if __name__ == '__main__':
    app.run()


import threading
import subprocess
import time
from pyngrok import ngrok # Use pyngrok for direct tunneling

def run_flask_app():
    subprocess.run(["python", "app.py"])

flask_thread = threading.Thread(target=run_flask_app)
flask_thread.daemon = True
flask_thread.start()

time.sleep(5) # Give the Flask app a moment to start

# Expose the Flask port (default is 5000)
public_url = ngrok.connect(5000)
print(f"Flask App Live at: {public_url}")
print("Please copy this URL and open it in your browser.")
Access the App: A public URL will be printed in the output of the last cell. Copy this URL and paste it into your web browser to access your live sentiment analysis application!

RESULT

Authtoken saved to configuration file: /root/.config/ngrok/ngrok.yml
Flask App Live at: NgrokTunnel: "https://fa3da339a925.ngrok-free.app" -> "http://localhost:5000"
Please copy this URL and open in your browser.

OPEN THE URL: https://fa3da339a925.ngrok-free.app And click on open.

<img width="1920" height="1080" alt="Screenshot 2025-07-14 110225" src="https://github.com/user-attachments/assets/9aa8324b-30ff-4439-a856-765e2fa402ae" />
this is our frontend web application.
