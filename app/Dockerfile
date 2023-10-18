#FROM python:3.6.8-alpine3.9
FROM python:3.10-alpine

LABEL MAINTAINER="Hari Kishore <hkishore42@gmail.com>"

WORKDIR /app

COPY app_crud.py /app/
COPY requirements.txt /app/

RUN pip install -r requirements.txt
RUN apk upgrade
RUN apk --no-cache add curl

EXPOSE 5000
CMD python app_crud.py