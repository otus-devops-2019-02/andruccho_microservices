FROM python:3.6.0-alpine

WORKDIR /app
ADD . /app

RUN apk --update add python py-pip openssl ca-certificates py-openssl wget
RUN apk --update add --virtual build-dependencies libffi-dev openssl-dev python-dev py-pip build-base \
  && pip install --upgrade pip \
  && pip install -r /app/requirements.txt \
  && apk del build-dependencies

#RUN pip install -r /app/requirements.txt

ENV POST_DATABASE_HOST post_db
ENV POST_DATABASE posts

ENTRYPOINT ["python3", "post_app.py"]
