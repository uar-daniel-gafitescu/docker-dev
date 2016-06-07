FROM redis:latest
COPY redis.conf /usr/local/etc/redis/redis.conf
VOLUME /var/lib/redis

CMD [ "redis-server", "/usr/local/etc/redis/redis.conf" ]

EXPOSE 6379