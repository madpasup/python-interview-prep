🔑 Workflow 

Created a Webex Bot Token

Generated the bot token in Webex Developer Portal.

This token is used for authentication with Webex APIs.

Backend Setup

A Flask backend (app.py) was created.

It listens for incoming Webex webhooks (chat messages).

Redis is used to maintain session states.

Webex → Backend Communication

User sends a query in Webex.

Webex forwards the message (via webhook) → Flask backend.

Hookbuster: The backend webhook receives it at the configured webhook IP.

The backend then fetches the relevant data from the data source.

Finally, the response is sent back to the same Webex client.

Backend → AI Processing

Flask app reads contextual knowledge from .txt / .csv.

Passes user query + context → Azure OpenAI.

AI generates the best possible response.

Backend → Webex Response

Bot first replies with “⏳ Thinking...” placeholder.

Then updates with AI-generated response.

Logs query to chat_history.csv and system logs to infrabot.log.

Code Repository (GitHub)

Your complete bot code is versioned in GitHub.

(You can share the repo link like: https://github.com/<org>/<repo-name>)

📂fp-monitor-bot Layout
fp-monitor-bot/
├── app.py                   # Flask server & Webex webhook handler
├── app_watchdog.py          # Watchdog logic for monitoring log/files
├── config.py                # Configuration (Webex token, Redis, Azure OpenAI)
├── hookbuster.py            # Utility to test Webex webhooks locally
├── infrabot.log             # Application log file (auto-rotated)
├── requirements.txt         # Python dependencies

🌐 Overview

Infra Bot is a Webex-integrated assistant that automates infrastructure-related query handling.
It listens to Webex chat messages, retrieves contextual data from text/CSV sources, and leverages Azure OpenAI for smart responses.

🔑 Key Features

Webex Integration → Send and update messages via Webex APIs.

Azure OpenAI → Intelligent responses based on knowledge base + user input.

Redis Sessions → Tracks user conversations with automatic expiry.

File-based Knowledge Base → Reads .txt and .csv files as reference data.

Logging → RotatingFileHandler ensures structured logs with rotation.

Chat History → Saves queries + metadata to chat_history.csv for audit.

Watchdog → Monitors log files and triggers actions when anomalies occur.

📝 Example Flow

User sends a message in Webex →
"What are you monitoring?"

Webex webhook triggers → Flask (app.py) processes the payload.

Session checked in Redis → loads history or starts fresh.

The data/ folder is the central location where all alert logs (Prometheus, UptimeKUMA, etc.) will be stored.

Raventh has to schedule a job to Fetch Prometheus alerts data and UptimeKUMA alerts data and Place them  inside the data/ folder.

Query + context passed to Azure OpenAI → generates response.

Bot replies in two stages:

Sends “⏳ Thinking...” placeholder.

Updates with the final AI response.

Interaction logged:

User queries → chat_history.csv  we are using it for further quering

System logs → infrabot.log

⚙️ Deployment
1. Install dependencies
pip install -r requirements.txt

2. Run Flask App
python app.py


(or via Gunicorn/Docker for production).

3. Webex Webhook

Expose port 5005 to the internet.

Register webhook in Webex Developer Portal pointing to:

https://<your-domain>:5005/

4. Environment / Config

Set required variables in config.py or environment:

REDIS_HOST → Redis server hostname/IP.

BOT_TOKEN → Webex bot token.

BOT_ID → Webex bot ID.

API_KEY, ENDPOINT, DEPLOYMENT_NAME, API_VERSION → Azure OpenAI settings.

DATA_FILE_PATH → Directory containing knowledge base files.

Flowchart









Steps 
Cloning into 'devx-production'...




Step 1: Clone repository

Step 2: Setup virtual environment

Step 3: Install dependencies

Step 4: Configure systemd services

Step 5: Manage services

git clone git@github.com:<org-or-username>/devx-production-code.git
cd devx-production-code/tools/infrabot

2. Create & Activate Virtual Environment
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt


(Optional: Test manually)

python3 app.py

3. Configure systemd Services

Copy service files:

sudo cp infrabot.service /etc/systemd/system/
sudo cp infrabot_hookbuster.service /etc/systemd/system/
sudo cp infrabot_watchdog.service /etc/systemd/system/


Reload systemd:

sudo systemctl daemon-reload

4. Enable Services (start on boot)
sudo systemctl enable infrabot
sudo systemctl enable infrabot_hookbuster
sudo systemctl enable infrabot_watchdog

5. Start Services
sudo systemctl start infrabot
sudo systemctl start infrabot_hookbuster
sudo systemctl start infrabot_watchdog

🔧 Service Management

Check status

systemctl status infrabot
systemctl status infrabot_hookbuster
systemctl status infrabot_watchdog


Restart

sudo systemctl restart infrabot infrabot_hookbuster infrabot_watchdog


Stop

sudo systemctl stop infrabot infrabot_hookbuster infrabot_watchdog


View logs

journalctl -u infrabot -f
journalctl -u infrabot_hookbuster -f
journalctl -u infrabot_watchdog -f


👉 With this format, any developer (even new joiners) can follow without second-guessing.

Do you want me to make this into a README.md file so you can drop it directly inside tools/infrabot/?
