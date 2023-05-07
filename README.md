# Add ChatGPT to Slack



## Slack App & OpenAI

### Setup Slack App

1. Login into Slack and and go to [Slack API website](https://api.slack.com/apps).

2. Click on Your Apps at top right corner and create a new app. 

   ![image-20230306085411462](./assets/Add%20ChatGPT%20to%20Slack.assets/image-20230306085411462.png)

3. Select **from Scratch**. Give an app name, e.g. `chatgpt` and select the workspace which it will be used.

   ![image-20230312205718982](./assets/Add%20ChatGPT%20to%20Slack.assets/image-20230312205718982.png)

4. From side menu, select **Features > OAuth & Permissions**. In the **Scopes** session, add following **Bot Token Scopes**: `app_mentions:read`, `channels:history`, `channels:read`, `chat:write`.

   ![image-20230306084016112](./assets/Add%20ChatGPT%20to%20Slack.assets/image-20230306084016112.png)

5. From side menu, select **Socket Mode**, check **Enable Socket Mode**, copy the `SLACK_APP_TOKEN`, which starts with `xapp-`

   * xapp-1-A04TX4QH8LR-5010375644498-0609ec96a4c9562255f43d6f84e334bb91303a0de2aa0c5f9a31e10849a7889b

   ![image-20230326152741190](./assets/Add ChatGPT to Slack.assets/image-20230326152741190.png)

   

6. From side menu, select **Features > Event Subscriptions**. Enable the Events. In **Subscribe to bot events**, add `app_mention`.

   ![image-20230306090437885](./assets/Add%20ChatGPT%20to%20Slack.assets/image-20230306090437885.png)

7. From side menu, select **Install App**. Copy the Bot User OAuth Token, which will be used as `slack-app-token` in next session.

   ![image-20230306090035829](./assets/Add%20ChatGPT%20to%20Slack.assets/image-20230306090035829.png)

8. Copy the `SLACK_BOT_TOKEN`, which starts with `xoxb`.

   * xoxb-371556214532-4936418345427-kcTx4vYwjEkCI3Tv8V5BB3YU

   ![image-20230312210430712](./assets/Add%20ChatGPT%20to%20Slack.assets/image-20230312210430712.png)



### Get ChatGPT Token

1. Go to [ChatGPT API site](https://openai.com/blog/openai-api).
2. Click on Sign Up and login with your account.
3. Click on your Account Profile on top right corner and select **View API Keys**.
4. Create a new key, which will be used as `OPENAI_API_KEY` in next session.



## Setup Python Program

1. Setup GitHub authentication using SSH.
2. Clone the GitHub repo. 

```
git clone git@github.com:qinjie/ChatGPT-in-Slack.git
cd ChatGPT-in-Slack
```

3. Create a virtual env and install the required libraries.

```
python -m virutalenv .venv --python=python3.9
source .venv/bin/activate
pip install -r requirements.txt
```

4. Copy `.env.sample` to `.env`. Update `.env` file about the `OPENAI_API_KEY`, `SLACK_APP_TOKEN` and `SLACK_BOT_TOKEN`.

5. Test the python script.

```
python main.py
```



### Run the Python Script upon Reboot

1. Edit the crontab

```
crontab -e
```

2. Add the line to run the script upon reboot.

We need to set the SHELL to `/bin/bash` becasue default shell in crontab is `/bin/sh` which doesn't support `source` command.

```
SHELL=/bin/bash
@reboot cd /home/ubuntu/ChatGPT-in-Slack && source .venv/bin/activate && pwd > /home/ubuntu/logs/chatgpt.log && nohup python main.py >> /home/ubuntu/logs/chatgpt.log  2>&1 &
```



