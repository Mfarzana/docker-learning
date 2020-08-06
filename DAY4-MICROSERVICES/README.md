
## Deploying Multiple Python Microservices to Docker
### Step 1:  Create directory 
In my case, I created two directories in pymicroservice those are service1 and service2
```
#Create Derectories
ubuntu@ip-172-31-4-174:~$ sudo mkdir pymicroservice
ubuntu@ip-172-31-4-174:~$ cd microservice/
ubuntu@ip-172-31-4-174:~/pymicroservice$ sudo mkdir service1
ubuntu@ip-172-31-4-174:~/pymicroservice$ sudo mkdir service2
```
### Step 2 : Create app.py, requirements.txt & Dockerfile  in service1 directory
```
ubuntu@ip-172-31-4-174:~/pymicroservice/service1$ sudo nano app.py
ubuntu@ip-172-31-4-174:~/pymicroservice/service1$ sudo nano Dockerfile
ubuntu@ip-172-31-4-174:~/pymicroservice/service1$ sudo nano requirements.txt  
```
**My app.py**
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
**My Dockerfile**
```
FROM python:3
COPY . /app
WORKDIR /app
RUN pip install requests
RUN pip install -r requirements.txt
CMD ["python","app.py"]
```
**My requirements.txt file** 
```
flask
```

### Step 3 : Create app.py, requirements.txt & Dockerfile  in service2 directory
```
ubuntu@ip-172-31-4-174:~/pymicroservice/service2$ sudo nano app.py
ubuntu@ip-172-31-4-174:~/pymicroservice/service2$ sudo nano Dockerfile
ubuntu@ip-172-31-4-174:~/pymicroservice/service2$ sudo nano requirements.txt
```
> NB: Or Use command for copy paste service1 files to service2 
>>  ubuntu@ip-172-31-4-174:~/pymicroservice/service1$ sudo cp * -r ../service2

**app.py** 
```                                                                     app.py
from flask import Flask
app = Flask(__name__)

@app.route('/greting')
def hello_world():
    return 'Hello, World!'

if __name__ == '__main__':
     app.run(debug=True, host='0.0.0.0')
  
 ```
**Dockefile**
```
FROM python:3
COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt
CMD ["python","app.py"]
```
 **requirements.txt file**
 ```
 flask 
 ```
### Step 4: Build  image from Dockerfile
```
ubuntu@ip-172-31-4-174:~/pymicroservice/service1$ sudo docker build -t mfarzana/microservice:0.0.1 .
ubuntu@ip-172-31-4-174:~/pymicroservice/service2$ sudo docker build -t mfarzana/microservice:0.0.2 .
# docker images list
ubuntu@ip-172-31-4-174:~$ sudo docker images
REPOSITORY              TAG                 IMAGE ID            CREATED              SIZE
mfarzana/microservice   0.0.2               03c9959a29b1        32 seconds ago       944MB
mfarzana/microservice   0.0.1               29fefc4131d5        About a minute ago   947MB
```
### Step 5: Run image in docker container
```
ubuntu@ip-172-31-4-174:~$ sudo docker container run --name pymicro-service2 -d -p 8085:5000 mfarzana/microservice:0.0.2

# Running container list
ubuntu@ip-172-31-4-174:~$ sudo docker ps
CONTAINER ID        IMAGE                         COMMAND             CREATED             STATUS              PORTS                    NAMES
dba424a24ee1        mfarzana/microservice:0.0.1   "python app.py"     17 minutes ago      Up 17 minutes       0.0.0.0:80->5000/tcp     xenodochial_liskov
0c6913d70e0c        mfarzana/microservice:0.0.2   "python app.py"     22 minutes ago      Up 22 minutes       0.0.0.0:8085->5000/tcp   pymicro-service2

# Get details about container:ip address 
ubuntu@ip-172-31-4-174:~$ sudo docker inspect 0c6913d70e0c

ubuntu@ip-172-31-4-174:~$ sudo docker run -p 80:5000 mfarzana/microservice:0.0.1
```
From Browser view 
![enter image description here](https://github.com/Mfarzana/docker-learning/blob/master/images/microservice-python.jpg)

## References:

 - [https://realpython.com/python-requests/](https://realpython.com/python-requests/)
 - [https://flask.palletsprojects.com/en/1.1.x/](https://flask.palletsprojects.com/en/1.1.x/)

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTMxMTE2Mjc3MywxMTYzNzg0Mzg3LC0xNj
A2MDYzNDA1LDQ3NjIzNzYzLC05ODg4ODAwNyw2NDgwODM4NTMs
MTQ3OTM2NTY1NSwyMTI1NTk3NjM1LDEyMDc0MDc3MzIsOTg1Nj
MxODM2LC0zNjA5OTMwMDYsLTY5MTU0NDc1OCwxNDIzMTY4NTAw
LDgwNDk5MDM3NSwyMDk2NjU4NDM2LDE2OTA2NDQ2NDRdfQ==
-->