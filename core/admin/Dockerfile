FROM alpine:3.8
# python3 shared with most images
RUN apk add --no-cache \
    python3 py3-pip \
  && pip3 install --upgrade pip
# Image specific layers under this line
RUN mkdir -p /app
WORKDIR /app

COPY requirements-prod.txt requirements.txt
RUN apk add --no-cache openssl curl \
 && apk add --no-cache --virtual build-dep openssl-dev libffi-dev python3-dev build-base \
 && pip3 install -r requirements.txt \
 && apk del --no-cache build-dep

COPY mailu ./mailu
COPY migrations ./migrations
COPY manage.py .
COPY start.py /start.py

RUN pybabel compile -d mailu/translations

EXPOSE 80/tcp
VOLUME ["/data"]

CMD /start.py

HEALTHCHECK CMD curl -f -L http://localhost/ui || exit 1
