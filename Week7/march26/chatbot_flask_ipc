from flask import Flask, request, jsonify
from flask_cors import CORS
import json
from fuzzywuzzy import process
import random

app = Flask(__name__)
CORS(app)

# Load JSON data
with open('law_data.json', 'r', encoding='utf-8') as file:
    data = json.load(file)

# Extract questions and answers
questions = list(data.keys())

def get_best_match(user_input):
    best_match, score = process.extractOne(user_input, questions)
    if score >= 60:
        return data[best_match]
    else:
        return "I'm sorry, I couldn't find an answer for that. Please try rephrasing your question."

# API to handle chat requests
@app.route('/chat', methods=['POST'])
def chat():
    user_message = request.json.get('message')
    if not user_message:
        return jsonify({'response': "Please enter a valid message."})
    
    response_text = get_best_match(user_message)
    return jsonify({'response': response_text})

# API to fetch 5 random questions from law_data.json
@app.route('/get_suggestions', methods=['GET'])
def get_suggestions():
    random_suggestions = random.sample(list(data.keys()), 5)
    return jsonify({"suggestions": random_suggestions})

# API to provide follow-up questions based on user input
@app.route('/get_followups', methods=['POST'])
def get_followups():
    user_input = request.json.get('userInput', '').lower()
    followups = [q for q in data.keys() if user_input in q.lower()]
    
    # If fewer than 3 relevant follow-ups are found, add random ones
    if len(followups) < 3:
        additional_followups = [q for q in data.keys() if q not in followups]
        followups.extend(random.sample(additional_followups, min(3 - len(followups), len(additional_followups))))
    
    return jsonify({"followups": followups[:3]})

if __name__ == '__main__':
    app.run(debug=True)
