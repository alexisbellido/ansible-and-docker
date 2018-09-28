FROM python:3.6.5-slim-stretch as builder
WORKDIR /root

# For pymssql, see http://pymssql.org/en/stable/intro.html
ENV PYMSSQL_BUILD_WITH_BUNDLED_FREETDS 1

RUN set -e \
  && apt-get update \
  && apt-get install -y --no-install-recommends \
    build-essential \
    default-libmysqlclient-dev \
    git-core \
    python3-dev \
    ssh \
  && rm -rf /var/lib/apt/lists/*

ARG SSH_PRIVATE_KEY
RUN mkdir -p /root/.ssh/ \
  && echo "${SSH_PRIVATE_KEY}" > /root/.ssh/id_rsa \
  && chmod 600 /root/.ssh/id_rsa \
  && echo "StrictHostKeyChecking no " > /root/.ssh/config

# Private repositories can be cloned now so the following will work on the
# intermediate container.
# git clone git@github.com:user/project.git
# pip install -e git://git.example.com/MyProject#egg=MyProject

COPY requirements.txt /root/requirements.txt

RUN python -m venv /env \
  && . /env/bin/activate \
  && pip install --upgrade pip \
  && pip install --no-cache-dir -r requirements.txt

FROM python:3.6.5-slim-stretch
LABEL maintainer="Alexis Bellido <a@zinibu.com>"

# Need to keep this in image for MySQL support in Django
RUN set -e \
  && apt-get update \
  && apt-get install -y --no-install-recommends \
    default-libmysqlclient-dev \
  && rm -rf /var/lib/apt/lists/*

ENV PYTHONUNBUFFERED 1
ENV PROJECT_NAME project
ENV PORT 8000

ENV REDIS_HOST none
ENV REDIS_PORT 6379

WORKDIR /root/$PROJECT_NAME
EXPOSE $PORT

# Copy the Python virtual environment setup. This is the multi-stage build magic.
COPY --from=builder /env /env

# For the minimal setup Django project is right here so we just copy it to the container.
COPY $PROJECT_NAME /root/$PROJECT_NAME

COPY docker-entrypoint.sh /usr/local/bin/docker-entrypoint.sh

SHELL ["/bin/bash", "-c"]
ENTRYPOINT ["/usr/local/bin/docker-entrypoint.sh"]