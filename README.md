# docker-redis-cluster
version: "3"
# https://yq.aliyun.com/articles/57953
services:
  redis-master:
    image: redis:3
    deploy:
      restart_policy:
        condition: on-failure
    command: redis-server --masterauth kaishui --requirepass kaishui
    ports:
      - 6379:6379

  redis-slave:
    image: redis:3
    command: redis-server --masterauth kaishui --requirepass kaishui --slaveof redis-master 6379
    deploy:
      replicas: 2
      restart_policy:
        condition: on-failure
  sentinel:
    image: registry.aliyuncs.com/acs-sample/redis-sentinel:3
    environment:
      - SENTINEL_DOWN_AFTER=5000
      - SENTINEL_FAILOVER=5000    
    deploy:
      replicas: 3
      restart_policy:
        condition: on-failure

