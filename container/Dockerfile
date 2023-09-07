FROM python:3.7-buster

RUN curl -sL https://deb.nodesource.com/setup_14.x | bash -
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt update && \
    apt install -y nodejs yarn && \
    pip3 install --no-cache-dir hyperglass

RUN mkdir -p /etc/hyperglass
# Move hyperglass files to /etc/hyperglass in order to not bind to a user, required in openshift
RUN hyperglass setup -d && cp -r /root/hyperglass/* /etc/hyperglass && rm -rf /root/hyperglass && rm -rf /etc/hyperglass/static/images/hyperglass-opengraph.jpg
# Make hyperglass configuration directory readable in openshift environments
RUN chgrp -R 0 /etc/hyperglass && \
    chmod -R g+rwX /etc/hyperglass
# Make pip directory writable in openshift environments
RUN chgrp -R 0 /usr/local/lib && \
    chmod -R g+rwX /usr/local/lib