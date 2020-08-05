
## Deploying Multiple Python Microservices to Docker
**Step 1:**  create directory 
In my case I created two directories in pymicroservice those are service1 and service2
```
#Create Derectories
ubuntu@ip-172-31-4-174:~$ sudo mkdir pymicroservice
ubuntu@ip-172-31-4-174:~$ cd microservice/
ubuntu@ip-172-31-4-174:~/pymicroservice$ sudo mkdir service1
ubuntu@ip-172-31-4-174:~/pymicroservice$ sudo mkdir service2
```
Step 2 :Create app.py , requirements.txt and Dockerfile  in service1 directory
```
ubuntu@ip-172-31-4-174:~/pymicroservice/service1$ sudo nano app.py
ubuntu@ip-172-31-4-174:~/pymicroservice/service1$ sudo nano Dockerfile
ubuntu@ip-172-31-4-174:~/pymicroservice/service1$ sudo nano requirements.txt  

```
My app.py
```
import requests
from flask import Flask
app = Flask(__name__)
@app.route('/microservice')
def hello_world():
    response=requests.get('http://172.17.0.2:5000/greting')
    if response.status_code == 200:
       #print('...............Success!....................')
       return 'Connection established succesfully'
    elif response.status_code == 404:
        #print('..........Not Found..............')
        return 'Error 404 : Not Found '
    else: return 'Hello, World!'
if __name__ == '__main__':
     app.run(debug=True, host='0.0.0.0')

```
Dockerfile
```

```
#Create Dockerfile in service1 directory
```

## References:

 - [https://realpython.com/python-requests/](https://realpython.com/python-requests/)

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU2ODkxNzQ5MiwtNjkxNTQ0NzU4LDE0Mj
MxNjg1MDAsODA0OTkwMzc1LDIwOTY2NTg0MzYsMTY5MDY0NDY0
NF19
-->