# Dockerfiles

## Python
```
FROM python:3.9

ENV OPENAI_API_KEY=placeholder

COPY ./project/ ./app/

COPY requirements.txt ./app/

WORKDIR /app/

EXPOSE 5000

RUN pip install -r requirements.txt

CMD ["gunicorn"  , "-b", "0.0.0.0:5000", "app:app"]
```

## Node
```
FROM node:18-alpine

ENV NODE_ENV production

COPY package.json ./app/

WORKDIR /app/

EXPOSE 3000

RUN npm install

COPY . .

RUN npm run build
```

## Go
```
FROM golang:1.20

WORKDIR /app

COPY go.mod go.sum ./

RUN go mod download

COPY *.go ./

RUN CGO_ENABLED=0 GOOS=linux go build -o /web-app

EXPOSE 8080

CMD ["/web-app"]
```

## Node (yarn)
```
FROM node:18.8-alpine

ENV NODE_ENV=production

RUN apk add bash

WORKDIR /home/node/app/

COPY package*.json ./

COPY . .

RUN npm install copyfiles -g

ENTRYPOINT ["./entrypoint.sh"]

CMD ["yarn", "serve"]
```
