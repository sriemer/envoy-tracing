# Original from envoyproject/envoy:examples/front-proxy/Dockerfile-frontenvoy
FROM envoyproxy/envoy:ff857396dc6e8c9a94c547bf080ecf6eaa38e333

RUN set -x \
  && apt-get update \
  && apt-get install --no-install-recommends --no-install-suggests -y \
              curl apt-transport-https iproute2 \
  && apt-get install --no-install-recommends --no-install-suggests -y lynx

ARG agent_key

ENV INSTANA_AGENT_KEY=${agent_key}

# Download extension from Artifactory
RUN sensor_version=$(lynx -auth _:${INSTANA_AGENT_KEY} -dump -listonly https://artifact-public.instana.io/artifactory/shared/com/instana/libinstana_sensor/ | grep -o 'https:.*/[0-9]\+\.[0-9]\+\.[0-9]\+/' | rev | cut -d '/' -f 2 | rev | sort -V | tail -n1); \
    curl --user _:${INSTANA_AGENT_KEY} --silent --output /usr/local/lib/libinstana_sensor.so https://artifact-public.instana.io/artifactory/shared/com/instana/libinstana_sensor/${sensor_version}/linux-amd64-libinstana_sensor.so

RUN chmod 777 /usr/local/lib/libinstana_sensor.so 

ADD ./start-gateway.sh /usr/local/bin/start-gateway.sh
RUN chmod u+x /usr/local/bin/start-gateway.sh

ENTRYPOINT /usr/local/bin/start-gateway.sh
