FROM ubuntu:14.04

LABEL io.k8s.description="nginx" \
      io.k8s.display-name="nginx" \
      io.openshift.expose-services="80:http" \
      io.openshift.tags="logstash,elasticsearchi,nginx"

RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 1484120AC4E9F8A1A577AEEE97A80C63C9D8B80B

RUN echo "deb http://osquery-packages.s3.amazonaws.com/trusty trusty main" >> /etc/apt/sources.list

RUN set -x \
  	&& apt-get update \
	&& apt-get install -y --no-install-recommends osquery nginx curl supervisor
ADD conf/supervisord.conf /etc/supervisord.conf
ADD conf/osquery.conf /etc/osquery/osquery.conf
COPY src/filebeat-5.0.0-alpha5-amd64.deb .
COPY src/metricbeat-5.0.0-alpha5-amd64.deb .
RUN dpkg -i filebeat-5.0.0-alpha5-amd64.deb
RUN dpkg -i metricbeat-5.0.0-alpha5-amd64.deb
ADD conf/filebeat.yml /etc/filebeat/filebeat.yml
ADD conf/metricbeat.yml /etc/metricbeat/metricbeat.yml

EXPOSE 80
CMD ["supervisord","-n","-c", "/etc/supervisord.conf"]
