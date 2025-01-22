## CTF_ASSESSMENT

I decided to build my own application which is a CTF as I wanted to test my skills to use the streamlit library and considering that we had the use of chatgpt, I wanted to see if I could built somethig much more advanced than the topics provided. Although I initillay built this application with a flask library which I found the experience to be much smoother I later adaptef to streamlit in order to satisfy teh certain assignment requirements. Through the process I learnt how streamlit handles state, user instructions, and front-end layout as its not the most realistic outputs that you would get from just say using a flask library. I did have some trouble transferring fucntionliaties from flask to streamlit but I managed to get it working and I still believe it demonstarted the core of a penetration testing workflow. 

## Structure 

CTF_ASSESSMENT/
├── CTF.py
├── requirements.txt
├── README.md
├── static/
│   ├── style.css
│   └── script.js
└── templates/
    ├── index.html
    ├── login.html
    └── dashboard.html

## How to run the application

To run the application, you will need to install the required dependencies. You can do this by running the following command:

```
pip install -r requirements.txt
```

Once you have installed the required dependencies, you can run the application by running the following command:

```
streamlit run CTF.py
```

## Requirements

- Flask==3.0.0
- python-dotenv==1.0.0  
- flask-cors==4.0.0    

## Writeup

Synopsis:
In this CTF you will need to login to the trainee account which will then show some suspicious files and directories which you as the hacker will need to exploit. You will then find credentials to further privilege escalate. From the devops user account, you will get need to enumerate the system to find information. Once you have logged in as the devops user, you will again need to enumerate the system to find furtehr credentials to then privilege escalate yourself to root. Once you have logged in as the root user, you will find the flag. 

Skills needed:
- Linux commands
- SSH and key managment

Skills learnt:
- SSH usage 
- Password and credential discovery 
- Puzzle-solving workflow
- Privilege escalation

Writeup:
To begin this CTF, I first logged into the trainee user with the provided credentials, I then begin enumerating with ls which reavealed a documents directory, within this directory I found multiple files including server_notes.txt, maintenance_notes.txt, and a todo.txt. Within the server_notes.txt file I found that they needed to review hidden directories so this gives us a clue that we might need to use the ls -la command to find what they might be talking about. Moving on, I then looked at maintenance_notes.txt which reveeals thatthey moved sensitve files to a .config directory. This is a big giveaway. Additionally, I then tried to cat the todo.txt file but it prompted for a password. 

So I run the command ls -la just to make sure the maintenance_logs.txt is lying to me and it wasn't. I found the .config directory which I then cd into and then run ls which I found a .env file. This migtht reveal some credentials in which we migth be able to use to login to someones account.

I then cat the .env file and found the credentials for the TODO=devops and his credentials TODO_PASS=S3cr3tP@ss. So now we know that these are the rigth credenitals which we can use in the todo.txt file, we will then cd back into the documents directoy in which we will then cat the todo.txt file and use these credentials to verify access which worked.

Looking at the todo.txt file we see that they need to change the SSH password to DevOps2024! so we will use this password to login to the devops user account which works. (the user_flag.txt can be found in the home directory)

We will then need to escalate our privileges to root. so looking at output of ls in home directory there is only one directory and thats it so that means there is probably hidden directories and files within it. We will run the comamnd ls -la in which we find a .ssh directory which is amazing. We will then cd into the .ssh directory and run ls which we find a id_rsa file. We will cat this which reveals a private key. We will submit this private key and we get an output of we can now ssh into the root user account.

To ssh into the account we will run the command ssh -i id_rsa root@192.168.1.100 which works and now we have succesfully privileges escalted and finsihed this CTF. (the root flag is found within the home directory)
