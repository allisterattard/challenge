FROM python:3.9.7-slim-buster

WORKDIR /app

COPY requirements.txt ./

COPY app.py ./

RUN pip install -r requirements.txt

EXPOSE 5000

CMD [ "python", "app.py" ]
