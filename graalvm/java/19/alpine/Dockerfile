FROM alpine:3.18.0

ENV JAVA_HOME /graalvm
ENV PATH $JAVA_HOME/bin:$PATH

# Default to UTF-8 file.encoding
ENV LANG='en_US.UTF-8' LANGUAGE='en_US:en' LC_ALL='en_US.UTF-8'

RUN apk update && \
  apk add curl gcc zlib gcompat 

RUN curl -L -o graalvm.tar.gz https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-22.3.1/graalvm-ce-java19-linux-amd64-22.3.1.tar.gz && \
  tar -xzf graalvm.tar.gz && \
  rm -rf graalvm.tar.gz && \
  mv graalvm-* /graalvm

