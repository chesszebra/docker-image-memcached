FROM debian:latest

# The user and group id to assign to the user
ARG USER_ID=1000
ARG GROUP_ID=1000

# Create a custom user to run the container on
RUN useradd memcached \
 && mkdir -p /home/memcached \
 && touch /home/memcached/.profile \
 && chown -R memcached:memcached /home/memcached \
 && mkdir /data \
 && chown -R memcached:memcached /data

# Configure the users
RUN usermod -u $USER_ID memcached \
 && groupmod -g $GROUP_ID memcached

# Update the system
RUN apt-get update \
 && apt-get install -y \
 	gcc \
 	libevent-dev \
 	libsasl2-2 \
 	libsasl2-dev \
 	libsasl2-modules \
 	make \
 	perl \
 	sasl2-bin \
 	wget


# Install memcached
RUN wget http://www.memcached.org/files/memcached-1.4.38.tar.gz \
 && tar -zxvf memcached-1.4.38.tar.gz \
 && cd memcached-1.4.38 \
 && ./configure --enable-sasl \
 && make  \
 && make test \
 && make install

# Run as memcached
USER memcached

# Expose the port
EXPOSE 11212

# Run memcached
CMD ["/usr/local/bin/memcached", "-vvv"]
