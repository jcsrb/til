# Install Only the Redis CLI

if you are on Ubuntu you can use the `redis-tools` package
```
sudo apt-get install redis-tools
```

if you are on any other distro 

```
wget http://download.redis.io/redis-stable.tar.gz
tar xvzf redis-stable.tar.gz
cd redis-stable
make
sudo cp src/redis-cli /usr/local/bin/
```

or if you have docker, get the docker image and run redis-cli from there

```
docker run -it --link some-redis:redis --rm redis redis-cli -h redis -p 6379
```

or use `nc`

```
nc -v redis.mydomain.com 6379
```

or even telnet 

```
telnet redis.mydomain.com 6379
```
