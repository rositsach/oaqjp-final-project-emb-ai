import requests
import json

def emotion_detector(text_to_analyze):
    # URL and Headers for the Watson NLP service
    url = 'https://sn-watson-emotion.labs.skills.network/v1/watson.runtime.nlp.v1/NlpService/EmotionPredict'
    headers = {"grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"}
    
    # Input JSON format
    myobj = { "raw_document": { "text": text_to_analyze } }
    
    # Sending the POST request
    response = requests.post(url, json=myobj, headers=headers)
    
    # Converting the response text into a dictionary
    formatted_response = json.loads(response.text)
    
    # Extracting the required emotions and their scores
    # The structure from Watson is: ['emotionPredictions'][0]['emotion']
    emotions = formatted_response['emotionPredictions'][0]['emotion']
    anger_score = emotions['anger']
    disgust_score = emotions['disgust']
    fear_score = emotions['fear']
    joy_score = emotions['joy']
    sadness_score = emotions['sadness']
    
    # Logic to find the dominant emotion
    emotion_list = [anger_score, disgust_score, fear_score, joy_score, sadness_score]
    emotion_names = ['anger', 'disgust', 'fear', 'joy', 'sadness']
    dominant_emotion = emotion_names[emotion_list.index(max(emotion_list))]
    
    # Final output format
    result = {
        'anger': anger_score,
        'disgust': disgust_score,
        'fear': fear_score,
        'joy': joy_score,
        'sadness': sadness_score,
        'dominant_emotion': dominant_emotion
    }
    
    return result