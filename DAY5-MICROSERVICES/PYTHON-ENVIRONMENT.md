## Python Hello World on Server
### Steps
```
ubuntu@ip-172-31-10-250:~$ mkdir hello
ubuntu@ip-172-31-10-250:~$ cd hello
ubuntu@ip-172-31-10-250:~/hello$ sudo nano app.py
ubuntu@ip-172-31-10-250:~/hello$ cd
ubuntu@ip-172-31-10-250:~$ sudo apt update

ubuntu@ip-172-31-10-250:~/hello$ sudo apt install python3-pip

ubuntu@ip-172-31-10-250:~/hello$ python3 -m pip install --user virtualenv
ubuntu@ip-172-31-10-250:~/hello$ sudo apt-get install python3-venv
ubuntu@ip-172-31-10-250:~/hello$ python3 -m venv env
ubuntu@ip-172-31-10-250:~/hello$ source ./venv/bin/activate
or 
ubuntu@ip-172-31-10-250:~/hello$ source env/bin/activate
(venv) ubuntu@ip-172-31-10-250:~/hello$ sudo nano requirements.txt
(venv) ubuntu@ip-172-31-10-250:~/hello$ pip install -r requirements.txt
```

### References

 - [https://flask.palletsprojects.com/en/1.1.x/installation/](https://flask.palletsprojects.com/en/1.1.x/installation/)
 - [https://flask.palletsprojects.com/en/1.1.x/quickstart/#a-minimal-application](https://flask.palletsprojects.com/en/1.1.x/quickstart/#a-minimal-application)

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI5NjkzODAyMiwtMTE5MTkwMDU1MCwxND
gwMDcxMDExLDEyODQ3NzM5MzddfQ==
-->