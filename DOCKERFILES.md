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