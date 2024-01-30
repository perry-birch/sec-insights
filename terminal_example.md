* Ensure the .env file has been updated to include your OPENAI_API_KEY *

Terminal commands:

### First Terminal - Setup the Servers

```sh
cd backend
poetry shell
poetry install
set -a
source .env
docker compose up
```

* This will error on first run *
Shut down docker compose, on mac [Ctrl + C]

```sh
make migrate
make run
```

### Second Terminal - Populate Database

With the first terminal still running proceed in a new one with:

```sh
cd backend
poetry shell
poetry intall
set -a
source .env
make seed_db_local
```

### Third Terminal - Front End

```sh
cd frontend
cp .env.example .env
set -a
source .env
npm install
npm run dev
```

Open browser to "http://127.0.0.1:3000/"

### Fourth Terminal - Interactive Query

```sh
cd backend
poetry shell
make chat
```

You should see a prompt indicator like: (Chat🦙)

```sh
(Chat🦙) create
(Chat🦙) detail
(Chat🦙) message Hi
```
