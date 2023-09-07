FROM python:3.7-buster

RUN curl -sL https://deb.nodesource.com/setup_14.x | bash -
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt update && \
    apt install -y nodejs yarn && \
    pip3 install --no-cache-dir hyperglass
RUN useradd -ms /bin/bash hyperglass
USER hyperglass
WORKDIR /home/hyperglass
RUN hyperglass setup -d    