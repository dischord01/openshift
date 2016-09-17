FROM java:8-jre

LABEL io.k8s.description="Logstash" \
      io.k8s.display-name="logstash" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="logstash,elasticsearch"

RUN apt-key adv --keyserver ha.pool.sks-keyservers.net --recv-keys 46095ACC8548582C1A2699A9D27D666CD88E42B4

RUN echo "deb http://packages.elastic.co/logstash/5.0/debian stable main" >> /etc/apt/sources.list

RUN set -x \
  	&& apt-get update \
	&& apt-get install -y --no-install-recommends logstash 


EXPOSE 5044
COPY conf/logstash.conf /etc/logstash/logstash.conf
COPY conf/logstash.yml /etc/logstash/logstash.yml
ENV PATH /usr/share/logstash/bin:$PATH
WORKDIR /usr/share/logstash
COPY docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["logstash","-f", "/etc/logstash/logstash.conf", "--path.settings", "/etc/logstash/"]
