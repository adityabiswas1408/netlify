import json
import pandas as pd
from http.server import BaseHTTPRequestHandler

# Load the Excel data
data = pd.read_excel("VOTE.xlsx")

def handler(event, context):
    try:
        # Parse query
        body = json.loads(event['body'])
        user_input = body.get('query', '').upper()

        # Convert columns to string for comparison
        data['HRMS'] = data['HRMS'].astype(str)
        data['IPAS'] = data['IPAS'].astype(str)

        # Search for data in either HRMS or IPAS
        result = data[(data['HRMS'] == user_input) | (data['IPAS'] == user_input)]

        if result.empty:
            return {
                'statusCode': 404,
                'body': json.dumps({"message": "No data found"})
            }

        return {
            'statusCode': 200,
            'body': json.dumps(result.to_dict(orient='records'))
        }

    except Exception as e:
        return {
            'statusCode': 500,
            'body': json.dumps({"message": f"Error: {str(e)}"})
        }
