---
date: "2020-07-12T00:00:00Z"
subtitle: Example of CD for a Django Project
tags:
- deployment
title: Automate deployment on an EC2 instance using Github Webhooks
---

# Credits
This article have been mainly inspired by the project -[Midgard](https://github.com/iit-technology-ambit/Midgard). Thanks to [Pankaj Kumar](https://github.com/Shankusu993) with whom I setup this Continuous deployment. I am a newbie in this field of deployment. If any mistakes, please do provide a feedback. Also thanks to [Shubham Mishra](https://github.com/grapheo12) for helping us in the process.

# Problem Statement
There is a repository on github and you have an EC2 instance(or any instance provided by an IaaS). On every push to certain branch, you want the code in the instance to be updated.

# Approach
For every repo, Github provides something called Webhooks. On occurance of an event(pull request or push or issue or comment, there is a long list you can find in settings)  every time Github sends a POST request with payload to the server, whose link you provide in the Repo settings.
We build a Flask server and deploy it on the EC2 instance. And provide its link in the github webhooks section. So on every push when a payload is sent to the Flask Server, it runs a Bash Script which pulls from the Github the updated code and restarts the necessary things(like systemd service).

# Action:
### Script:-
For this example, I am considering deployment of a Django project([ref](https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-18-04)).  Suppose the name of the project is `myproject`, and is present at the location `/home/username/myproject` in the instance. Assuming the systemd service file is written in a file name   `myproject.service`. Lets make Bash Script file - `deploy.sh`
```
#!/bin/bash

cd /home/username/myproject 

/usr/bin/git fetch
sleep 5

/usr/bin/git reset --hard origin/master
sleep 5

source env/bin/activate
sleep 5

pip3 install -r requirements.txt
sleep 5

python3 manage.py migrate
sleep 5

sudo systemctl restart myproject.service

```
From the above  code, it is clear that these following steps are done in order
- Moving to project directory
- "Force pulling" from repo's master branch. - Using fetch and reset --hard
-  Activating the virtual environment
- Installing requirements 
- Migrating DB
- Restarting the service

I would like to emphasize few things here 
- Usage of /usr/bin/git instead of  just git . Using just `git` doesn't help
- Instead of doing `git pull`, we are doing `fetch` and `reset  --hard`. Reason sometimes we force push to certain branches after squashing commits or rebasing in that case using just git pull won't update
- The command sudo systemctl restart myproject.service is executed without password? This could be done because we added a line in /etc/sudoers ([ref1](https://unix.stackexchange.com/questions/192706/how-could-we-allow-non-root-users-to-control-a-systemd-service)), ([ref2](https://www.digitalocean.com/community/tutorials/how-to-edit-the-sudoers-file))
  ```
  %username ALL=(ALL) NOPASSWD: /bin/systemctl restart myproject.service
	```
- I have added sleep in middle so that execution of commands continuously doesn't overload the instance CPU usage.

### Flask Application:-
Refer [this](https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uswgi-and-nginx-on-ubuntu-18-04) to deploy a Flask application. And the code in `"app.py"` (the file which is run to start the server) like this.
```
from flask import Flask, request
from subprocess import call
import os
import json
app = Flask(__name__)

@app.route("/", methods=['GET', 'POST']) 
def deploy():
    if json.loads(request.get_data().decode("utf-8"))["ref"] == 'refs/heads/master':
        call(["./deploy.sh"], shell=True) 
        return "Success", 200
    return "Success - not updated", 200

if __name__ == "__main__":
    app.run(host='0.0.0.0')
```

When the Github Webhook is going to hit a POST request with payload at `/`  route
we are going to check from the payload that push is made to master branch or not. If so, then we run the script that we made above and return a string with status 200. If not then we simply return another string with  status 200.
Lets say this flask application deployed with domain name - `deploy.example.com`

### Configuring Github Webhooks:-
Go to Settings  of your repository. On the vertical Navbar present at the left click on the Webhooks
Click on Add webhooks. And select the options as per the below image. Regarding the secret, better keep a randomly generated long string.
![Github Webhook Settings](/img/github_webhooks.png.PNG)

So, now as we planned on every push Github will send a payload to the Flask server, which contains a lot of details. On receiving the details, the Flask server will  check if the push has be done to the particular branch or not(here 'master'). If yes, it "pulls" the code from github and executes the necessary commands put in the Bash Script.

# Still Facing Issues?
- Sometimes it takes time for changes to be reflected especially in the case of DNS changes. So if things don't work out immediately check if this is the issue.
- In case you made changes to the Flask Server, don't forget to restart the service.
- Script didn't run? Make sure that you have given the execute permissions to the script using `chmod`
- For debugging, you can redeliver the payload from the Github itself below  Webhook settings

# Important things I missed
Now anyone can send requests to the Flask server and do malicious things. It is always better to check if the request you got the POST request  from Github or not. That was the pupose of the Secret in Webhook settings. You can check [here](https://developer.github.com/webhooks/securing/) and [here](https://github.com/iit-technology-ambit/Midgard/blob/master/hookListen.py)

What if the repo is not public? In that case, what we can do is to create dummy Github User and give it the necessary permissions(adding collaborator or adding to the Github Organization). And execute this command in the repo directory
```
git config remote.origin.url https://{USERNAME}:{PASSWORD}@github.com/{USERNAME}/{REPONAME}.git
```
[Ref: Answer by Sergio Morstabilini](https://superuser.com/questions/199507/how-do-i-ensure-git-doesnt-ask-me-for-my-github-username-and-password)
# Conclusion
I know I have assumed too many things. But I feel there are better articles to explain such things - Deploying Flask, deploying Django and I have tried putting references at places. Another interesting problem would be what if there was React Application? We need to generate its build also right? Sometimes the instance dies while building the code, so a better option would be to use Travis or Github Workflows to build a new branch which consists of build of the latest code([ref](https://youtu.be/BFpSD2eoXUk)) and then pull the build from that branch.
