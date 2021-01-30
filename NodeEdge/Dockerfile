FROM selenium/node-base:3.141.59-20210128
LABEL authors=SeleniumEdge

USER root

# Edge browser
RUN wget -q -O - https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \ 
&& echo "deb [arch=amd64] https://packages.microsoft.com/repos/edge stable main" >> /etc/apt/sources.list.d/microsoft-edge-dev.list \
&& apt-get update -qqy \
&& apt-get -qqy install microsoft-edge-dev \
&& rm /etc/apt/sources.list.d/microsoft-edge-dev.list \
&& rm -rf /var/lib/apt/lists/* /var/cache/apt/*

## Edge Wrapper
COPY wrap_edge_binary /opt/bin/wrap_edge_binary
RUN sudo chmod +x /opt/bin/wrap_edge_binary \
&& /opt/bin/wrap_edge_binary

USER 1200

# Edge driver
ARG EDGE_VERSION
RUN if [ -z "$EDGE_VERSION"]; \
then EDGE_VERSION=$(microsoft-edge -version | awk '{ print $3 }'); \ 
fi \
&& echo "Using mgsedgedriver version: "$EDGE_VERSION \
&& wget --no-verbose -O /tmp/edgedriver_linux64.zip https://msedgewebdriverstorage.blob.core.windows.net/edgewebdriver/$EDGE_VERSION/edgedriver_linux64.zip \
&& rm -rf /opt/selenium/msedgedriver \
&& unzip /tmp/edgedriver_linux64.zip -d /opt/selenium \
&& rm /tmp/edgedriver_linux64.zip \
&& mv /opt/selenium/msedgedriver /opt/selenium/msedgedriver-$EDGE_VERSION \
&& chmod 755 /opt/selenium/msedgedriver-$EDGE_VERSION \
&& sudo ln -fs /opt/selenium/msedgedriver-$EDGE_VERSION /usr/bin/msedgedriver

# Edge default config
COPY generate_config /opt/bin/generate_config
RUN sudo chmod +x /opt/bin/generate_config \
&& /opt/bin/generate_config > /opt/selenium/config.json

# Setting location to edge web driver.
ENV JAVA_OPTS="-Dwebdriver.edge.driver=/usr/bin/msedgedriver"

