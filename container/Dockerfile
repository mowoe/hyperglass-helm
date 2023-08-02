FROM python:3.7-buster

RUN curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
RUN apt update && \
    apt install -y nodejs yarn && \
    pip3 install --no-cache-dir hyperglass
RUN hyperglass setup -d    