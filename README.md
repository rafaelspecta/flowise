# Experimenting with flowise

## Instructions

### Donwload the repository

```
git clone git@github.com:rafaelspecta/flowise.git
```

### Install Docker

Install Docker according to your OS and run it     s

### Run Docker Compose

Docker Compose will initiate the Postgresql Database and Flowise

Note: Run inside the `flowise` directory created when you checked out the project

```
docker-compose up -d
```

### Access from the browser

```
http://localhost:8459/
```

* User: admin
* Pass: 123

### Stop the services (WHEN NOT USING)

```
docker-compose down
```
