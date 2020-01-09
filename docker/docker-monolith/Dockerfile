FROM ubuntu:16.04

RUN apt-get update -qq && \
    apt-get install -y --no-install-recommends ruby-full=1:2.3.0+1 \
                                               ruby-dev=1:2.3.0+1 \
                                               mongodb-server=1:2.6.10-0ubuntu1 \
                                               git=1:2.7.4-0ubuntu1.7 \
                                               build-essential=12.1ubuntu2 && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    gem install bundler:2.1.3 --no-ri --no-rdoc
RUN git clone -b monolith https://github.com/express42/reddit.git

COPY mongod.conf /etc/mongod.conf
COPY db_config /reddit/db_config
COPY start.sh /start.sh

WORKDIR /reddit
RUN bundle install
RUN chmod 0777 /start.sh

CMD ["/start.sh"]
