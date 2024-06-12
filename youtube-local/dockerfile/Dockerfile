FROM python:alpine
RUN apk add git
WORKDIR /app
RUN git clone https://github.com/user234683/youtube-local.git
WORKDIR /app/youtube-local
RUN pip install -r requirements.txt
COPY ./settings.txt .
EXPOSE 8080
CMD ["python", "./server.py"]
