FROM python:3.6.5-stretch
MAINTAINER Alexis Bellido <a@zinibu.com>

COPY requirements.txt /root/requirements.txt
COPY docker-entrypoint.sh /usr/local/bin/docker-entrypoint.sh
RUN chmod +x /usr/local/bin/docker-entrypoint.sh
WORKDIR /root

ENV PYMSSQL_BUILD_WITH_BUNDLED_FREETDS 1

# Install requisites for building Python packages.
# I need to avoid error messages from pip freeze.
RUN set -x \
  && apt-get -y update \
  && apt-get install -y unixodbc-dev

RUN /bin/bash -c "pip install --upgrade pip && pip install --no-cache-dir -r requirements.txt"

ENTRYPOINT ["/usr/local/bin/docker-entrypoint.sh"]

# TODO CMD part
#CMD ["building"]
