## Manual installation
 - Install nodejs locally ()
 - Clone the repo
 - Install dependencies (npm install)
 - Start the DB locally
    - docker run -e POSTGRES_PASSWORD=mysecretpassword -d -p 5432:5432 postgres
    - Go to neon.tech and get yourself a new DB
 - Change the .env file and update your DB credentials
 - npx prisma migrate
 - npx prisma generate
 - npm run build
 - npm run start

 ## Docker installation
 - Install docker
 - Create a network - docker network create user_project
 - Start postgres
    -  docker run --network user_project --name postgres -e POSTGRES_PASSWORD=mysecretpassword -d -p 5432:5432 postgres
 - Build the image - `docker build --network=host -t user-project .`
 - Start the image - `docker run -e DATABASE_URL=postgresql://postgres:mysecretpassword@postgres:5432/postgres --network user_project -p 3000:3000 user-project`

 ## Docker Compose installation steps
 - Install docker, docker-compose
 - Run `docker-compose up`



//Random commit(More structured with volumes and other things)
version: '3.8'

services:
  app:
    build: .
    container_name: node_app
    environment:
      - DATABASE_URL=postgresql://postgres:mysecretpassword@db:5432/postgres
    depends_on:
      - db
    ports:
      - "3000:3000"
    volumes:
      - .:/app
      - /app/node_modules
    command: ["npm", "start"]

  db:
    image: postgres:15
    container_name: postgres_db
    restart: always
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: mysecretpassword
      POSTGRES_DB: postgres
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata: