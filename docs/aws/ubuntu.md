# Configure Ubuntu

```sh
# update package manager
sudo apt-get update

# install supporting utils
sudo apt-get install -y --no-install-recommends wkhtmltopdf s3fs libpq-dev gcc make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
# ** Accept the prompt

# install poetry
curl -sSL https://install.python-poetry.org | python3 -

# switch to sudo user
sudo su -

# configure poetry path
export PATH="/home/ubuntu/.local/bin:$PATH"

# verify Poetry installation
poetry --version

# download source code
git clone https://github.com/perry-birch/sec-insights

# install docker
snap install docker

# install make
apt-get install make

# install aws cli
apt-get install awscli
# ** Accept the prompt

# install python3 dev
apt-get install python3-dev
```


```sh
# move to the backend dir
cd ~/sec-insights/backend

# configure the backend environment
# !! BE SURE TO REPLACE RELEVANT SECRETS !!
cat > .env << EOL
DATABASE_URL=postgresql://user:password@127.0.0.1:5432/llama_app_db
LOG_LEVEL=debug
RENDER=False
BACKEND_CORS_ORIGINS='["http://localhost", "http://localhost:8000", "http://localhost:3000", "http://127.0.0.1:3000", "https://sec-insight.vizidrix.com", "https://sec-insights.vizidrix.com"]'
SEC_EDGAR_COMPANY_NAME=YourOrgName
SEC_EDGAR_EMAIL=you@example.com
OPENAI_API_KEY=sk-eAN440fdaenDSXGAN1k7OqT3BlbkFJtLPd7s7aGu9OsK
POLYGON_IO_API_KEY=xxx
S3_BUCKET_NAME=llama-sec-insights-001
S3_ASSET_BUCKET_NAME=llama-sec-insights-001
S3_ENDPOINT_URL=https://llama-sec-insights-001.s3.us-east-2.amazonaws.com/
CDN_BASE_URL=https://llama-sec-insights-001.s3.us-east-2.amazonaws.com/
AWS_KEY=AKIFIOPVKEAA8889DD9K
AWS_SECRET=Npa8DI8A903KkakcpewqV76Capc43Fp
EOL

# install poetry dependencies
poetry install

# enter the poetry shell
poetry shell
# Notice Prompt: (llama-app-backend-py3.10) root@ip-172-31-30-55:~/sec-insights/backend# 

# prepare the docker setup
docker compose up
# !! NOTICE THE ERROR MESSAGE !!
```

![Docker Compose Error](./ubuntu/ubuntu_compose_error.png)

```sh
# stop the docker compose process [ctrl + C]

# enter enviornment set mode
set -a

# bind the enviornment variables
source .env

# run the database migration script
make migrate

# run the database service
make run
```

## Open a Second Terminal Session

```sh
# switch to sudo user
sudo su -

# configure poetry path
export PATH="/home/ubuntu/.local/bin:$PATH"

# move to the backend dir
cd ~/sec-insights/backend

# enter enviornment set mode
set -a

# bind the enviornment variables
source .env

# enter the poetry shell
poetry shell

# seed the database
make seed_db
```


## Automate Frontend Service Start

```sh
# Create and run service to launch frontend
cat > /etc/systemd/system/frontend.service << EOL
[Unit]
Description=SecInsights Frontend Service

[Service]
ExecStart=npm run dev 
WorkingDirectory=/root/sec-insights/frontend
Restart=always

[Install]
WantedBy=default.targetsystemctl daemon-reload
EOL

systemctl daemon-reload

systemctl stop frontend

systemctl enable frontend

systemctl start frontend

journalctl -xe -u frontend
```

## Automate Backend Service Start

```sh
# Create and run service to launch backend
cat > /etc/systemd/system/backend.service << EOL
[Unit]
Description=SecInsights Backend Service

[Service]
# ExecStart=set -a && source .env && poetry shell && make run
# ExecStart=/bin/bash -c 'set -a && source .env && poetry shell && make run'
ExecStart=/bin/bash -c 'set -a && source /root/sec-insights/backend/.env && poetry run make run'
WorkingDirectory=/root/sec-insights/backend
Restart=always
Environment=PATH="/root/.local/bin:$PATH"

[Install]
WantedBy=default.target
EOL

systemctl daemon-reload

systemctl enable backend

systemctl start backend

journalctl -xe -u backend
```


```sh
# Setup Cloudflared

mkdir -p --mode=0755 /usr/share/keyrings
curl -fsSL https://pkg.cloudflare.com/cloudflare-main.gpg | tee /usr/share/keyrings/cloudflare-main.gpg >/dev/null
echo 'deb [signed-by=/usr/share/keyrings/cloudflare-main.gpg] https://pkg.cloudflare.com/cloudflared bullseye main' | tee /etc/apt/sources.list.d/cloudflared.list
apt-get update
apt-get install cloudflared

cloudflared service install ##REPLACE THIS WITH A REAL KEY##

# service cloudflared start
service cloudflared start


```