FROM rayproject/ray:2.9.1

SHELL ["/bin/bash", "-ec"]
USER root

ENV TZ=America/New_York
ENV DEBIAN_FRONTEND=noninteractive

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone \
    && date

RUN apt-get update \
    && apt-get install --assume-yes \
      curl \
      ncat

# shhhh... Secrets
RUN sudo echo "VERY_SECRET_FILE" >> /etc/secret_file
ENV TEST_SECRET=VERY_SECRET

USER ray

CMD ["ray", "start", "--head", "--block"]
