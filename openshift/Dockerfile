FROM docker.elastic.co/elasticsearch/elasticsearch:5.6.3

ENV ELASTICSEARCH_MAJOR 5.6
ENV ELASTICSEARCH_VERSION 5.6.3
#ENV xpack.security.enabled=false
ENV PATH /usr/share/elasticsearch/bin:$PATH
ENV ES_GROUP 0
USER root

WORKDIR /usr/share/elasticsearch
COPY config ./config
COPY docker-entrypoint.sh /
COPY jvm.options /usr/share/elasticsearch/config/

EXPOSE 9200 9300

RUN chgrp -R 0 /usr/share/elasticsearch/ && \
    chmod -R g=u /usr/share/elasticsearch/

RUN /usr/share/elasticsearch/bin/elasticsearch-plugin install -b -s ingest-attachment && \
    /usr/share/elasticsearch/bin/elasticsearch-plugin install -b -s mapper-attachments

RUN echo "* - nofile 66000" >> /etc/security/limits.conf && \
    echo "* - memlock unlimited" >> /etc/security/limits.conf && \
    echo vm.max_map_count=262144 > /etc/sysctl.d/max_map_count.conf && \
    echo vm.swappiness=0 > /etc/sysctl.d/swappiness.conf

#RUN chmod -R og+w /usr/share/elasticsearch && \
#    chmod 755 /docker-entrypoint.sh
#RUN sysctl -w vm.max_map_count=261244
USER 1000

#ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["/usr/share/elasticsearch/bin/elasticsearch"]
