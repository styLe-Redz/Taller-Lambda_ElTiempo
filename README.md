# Taller-Lambda_ElTiempo

import json
import urllib.request
import boto3
import datetime

def lambda_handler(event, context):
    
    url = "https://www.eltiempo.com/"

    try:
        response = urllib.request.urlopen(url)
        html_content = response.read().decode('utf-8')
        print(html_content)  # Imprime el contenido HTML de la página
    
    except urllib.error.URLError as e:
        print("Error al hacer la solicitud:", e)
        
    
    s3_client = boto3.client('s3')
    
    html = html_content
    
    current_date = datetime.datetime.now().strftime('%Y-%m-%d')

    s3_key = f'{current_date}.html'
    s3_client.put_object(Bucket='pc1.2', Key=s3_key, Body=html_content)
    
    #s3_client.put_object(Body=html, Bucket='pc1.2', Key='elTiempo.html', ContentType='text/html')
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda!')
    }

