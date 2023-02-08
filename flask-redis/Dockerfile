FROM python:3.6-alpine as bigimage

# install the C compiler
RUN apk add --no-cache linux-headers g++

# instead of installing, create a wheel
RUN pip wheel --wheel-dir=/root/wheels uwsgi

# Image WITHOUT C compiler but WITH uWSGI
FROM python:3.6-alpine

WORKDIR /app

COPY --from=bigimage /root/wheels /root/wheels

# Ignore the Python package index
# and look for archives in
# /root/wheels directory
RUN pip install \
      --no-index \
      --find-links=/root/wheels \
      uwsgi

COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

COPY . .

EXPOSE 8080
CMD ["uwsgi", "--ini", "conf.ini"]
