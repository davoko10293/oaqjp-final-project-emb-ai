import requests
import json

def emotion_detector(text_to_analyze):
    url = 'https://sn-watson-emotion.labs.skills.network/v1/watson.runtime.nlp.v1/NlpService/EmotionPredict'
    headers = {"grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"}
    myobj = { "raw_document": { "text": text_to_analyze } }
    
    response = requests.post(url, json=myobj, headers=headers)
    
    # Преобразуем текст ответа в словарь Python
    formatted_response = json.loads(response.text)
    
    # Извлекаем словарь с эмоциями из структуры ответа Watson
    # Структура: ['emotionPredictions'][0]['emotion']
    emotions = formatted_response['emotionPredictions'][0]['emotion']
    
    # Извлекаем оценки для каждой эмоции
    anger_score = emotions['anger']
    disgust_score = emotions['disgust']
    fear_score = emotions['fear']
    joy_score = emotions['joy']
    sadness_score = emotions['sadness']
    
    # Находим доминирующую эмоцию
    # Функция max() найдет ключ с максимальным числовым значением
    dominant_emotion = max(emotions, key=emotions.get)
    
    # Формируем итоговый словарь согласно заданию
    result = {
        'anger': anger_score,
        'disgust': disgust_score,
        'fear': fear_score,
        'joy': joy_score,
        'sadness': sadness_score,
        'dominant_emotion': dominant_emotion
    }
    
    return result