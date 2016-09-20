FROM debian:jessie

# add our user and group first to make sure their IDs get assigned consistently
RUN groupadd -r kibana && useradd -r -m -g kibana kibana

RUN apt-get update && apt-get install -y \
		ca-certificates \
		wget \
	--no-install-recommends && rm -rf /var/lib/apt/lists/*

# https://www.elastic.co/guide/en/kibana/4.4/setup.html#kibana-apt
# https://packages.elasticsearch.org/GPG-KEY-elasticsearch
#RUN apt-key adv --keyserver ha.pool.sks-keyservers.net --recv-keys 46095ACC8548582C1A2699A9D27D666CD88E42B4
#RUN echo "deb http://packages.elastic.co/kibana/5.0.0-alpha5/debian stable main" >/etc/apt/sources.list.d/kibana.list


COPY src/kibana-5.0.0-alpha5-linux-x86_64.tar.gz /opt
RUN tar -zxvf /opt/kibana-5.0.0-alpha5-linux-x86_64.tar.gz -C /opt
RUN mv /opt/kibana-5.0.0-alpha5-linux-x86_64 /opt/kibana
RUN set -x \
	&& apt-get update \
	&& chown -R kibana:kibana /opt/kibana \
	&& rm -rf /var/lib/apt/lists/*

ENV PATH /opt/kibana/bin:$PATH

COPY docker-entrypoint.sh /
RUN /opt/kibana/bin/kibana-plugin install x-pack
COPY conf/kibana.yml /opt/kibana/conf/kibana.yml

EXPOSE 5601

RUN chmod -R og+w /opt/kibana
USER 1000

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["kibana"]
