FROM redis:3.2.11
MAINTAINER yangzhaoa

EXPOSE 26379

ADD sentinel.conf /etc/redis/sentinel.conf

RUN chown redis:redis /etc/redis/sentinel.conf

ENV MASTER_ALIAS redis-master
ENV SENTINEL_QUORUM 2
ENV SENTINEL_DOWN_AFTER 5000
ENV SENTINEL_FAILOVER 5000



