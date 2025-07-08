FROM        --platform=$TARGETOS/$TARGETARCH python:3.13-slim-bookworm

LABEL       author="Jack O'Neill" maintainer="168836329+jackoneill77@users.noreply.github.com"

RUN apt-get update \
    && apt-get install -y --no-install-recommends git curl ca-certificates dnsutils iproute2 procps openssh-client tini file mysql-client \
    && rm -rf /var/lib/apt/lists/*

ENV POETRY_HOME="/opt/poetry" POETRY_VERSION="2.1.3"
RUN curl -sSL https://install.python-poetry.org | python3 - --version "${POETRY_VERSION}" \
    && ln -s "${POETRY_HOME}/bin/poetry" /usr/local/bin/poetry

RUN groupadd -g 999 container \
    && useradd -u 999 -g 999 -m -d /home/container -s /bin/bash container

USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

COPY --chown=container:container ./entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

STOPSIGNAL SIGINT
ENTRYPOINT ["/usr/bin/tini", "-g", "--"]
CMD ["/entrypoint.sh"]

