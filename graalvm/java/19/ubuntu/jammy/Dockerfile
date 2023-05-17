FROM ubuntu:22.04

ENV JAVA_HOME /graalvm
ENV PATH $JAVA_HOME/bin:$PATH

# Default to UTF-8 file.encoding
ENV LANG='en_US.UTF-8' LANGUAGE='en_US:en' LC_ALL='en_US.UTF-8'

RUN apt-get update && \
    apt-get install -y --no-install-recommends ca-certificates build-essential zlib1g-dev wget && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists

RUN set -eux; \
    ARCH="$(dpkg --print-architecture)"; \
    case "${ARCH}" in \
        aarch64) \
            ESUM="95b0d0b1bf7e586695d8cf595df7a532b25314745397bb3d044cd00c409f6a0d"; \
            REMOTE_URL="https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-22.3.1/graalvm-ce-java19-linux-aarch64-22.3.1.tar.gz"; \
            ;; \
        amd64) \
            ESUM="7cd99d805e7a8b7d4c4576802fb107fb862944e47ce5f2e4f37c0f469a70dd2f"; \
            REMOTE_URL="https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-22.3.1/graalvm-ce-java19-linux-amd64-22.3.1.tar.gz"; \
            ;; \
        *) \
            echo "Unsupported arch: ${ARCH}"; \
            exit 1; \
            ;; \
    esac; \
    wget -O graalvm.tar.gz ${REMOTE_URL}; \
    echo "${ESUM} *graalvm.tar.gz" | sha256sum -c -; \
    mkdir -p "${JAVA_HOME}"; \
    tar --extract \
        --file graalvm.tar.gz \
        --directory "$JAVA_HOME" \
        --strip-components 1; \
  rm -f graalvm.tar.gz;

RUN echo Verifying install ... \
  && fileEncoding="$(echo 'System.out.println(System.getProperty("file.encoding"))' | jshell -s -)"; [ "$fileEncoding" = 'UTF-8' ]; rm -rf ~/.java \
  && echo javac --version && javac --version \
  && echo java --version && java --version \
  && echo Complete.

CMD ["jshell"]
