FROM ubuntu:22.04

SHELL ["/bin/bash", "-ec"]

ENV TZ=America/New_York
ENV DEBIAN_FRONTEND=noninteractive

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone \
    && date

COPY requirements.txt /poc/requirements.txt
WORKDIR /poc

RUN apt-get update \
    && apt-get install --assume-yes \
      python3 \
      python3-cryptography \
      python3-ipython \
      python3-pyx \
      python3-pip \
      unzip \
      curl \
      wget \
      ca-certificates \
      iproute2 \
      inetutils-ping \
      net-tools \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && pip install -r requirements.txt

# shhhh... Secrets
RUN echo "VERY_SECRET_FILE" >> /etc/secret_file
ENV TEST_SECRET=VERY_SECRET

EXPOSE 5000

CMD ["ray", "start", "--head"]
