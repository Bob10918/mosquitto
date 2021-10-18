FROM openssl-oqs:latest


# get sources
WORKDIR /
RUN git clone https://github.com/Bob10918/mosquitto.git
RUN git clone https://github.com/DaveGamble/cJSON.git

# install mosquitto
RUN apt-get install -y g++
WORKDIR /cJSON
RUN mkdir build && cd build && cmake .. -DENABLE_CJSON_UTILS=On -DENABLE_CJSON_TEST=Off -DCMAKE_INSTALL_PREFIX=/usr && make DESTDIR=$pkgdir install
WORKDIR /mosquitto
RUN make -j 8 binary

# Set up the entry point script and default command
ENV LD_PRELOAD="/openssl/libcrypto.so.1.1 /openssl/libssl.so.1.1"
COPY mosquitto.conf passwd.txt /
COPY /certs /certs
EXPOSE 8883
WORKDIR /mosquitto/src
CMD ["./mosquitto", "-c", "/mosquitto.conf"]
