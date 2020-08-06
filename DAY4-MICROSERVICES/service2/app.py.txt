import requests
from flask import Flask
app = Flask(__name__)

@app.route('/greting')
def hello_world():
    return 'Hello, World!'

if __name__ == '__main__':
     app.run(debug=True, host='0.0.0.0')