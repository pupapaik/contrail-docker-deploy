FROM ubuntu:14.04.3
MAINTAINER Pedro Marques <roque@juniper.net>
RUN apt-get update
RUN apt-get install -y software-properties-common curl patch
RUN add-apt-repository cloud-archive:kilo
RUN apt-add-repository -y ppa:opencontrail
RUN apt-add-repository -y ppa:opencontrail/r2.20
RUN apt-get update
RUN apt-get install -y contrail-config-openstack

# TMP Apply patch from review-id https://review.opencontrail.org/#/c/12863/1/src/config/api-server/utils.py
#RUN curl -s https://review.opencontrail.org/cat/12863%2C1%2Csrc/config/api-server/utils.py%5E1 | gunzip > /tmp/utils.py.orig
#RUN curl -s https://review.opencontrail.org/cat/12863%2C1%2Csrc/config/api-server/utils.py%5E0 | gunzip >/tmp/utils.py
#RUN diff -up /tmp/utils.py.orig /tmp/utils.py > /tmp/utils.py.diff || true
#RUN patch /usr/lib/python2.7/dist-packages/vnc_cfg_api_server/utils.py /tmp/utils.py.diff
#RUN rm -rf /tmp/utils.py*

RUN sed -i 's/parser.add_argument("--disc_server_ip", help="IP address of discovery server")//' /usr/lib/python2.7/dist-packages/vnc_cfg_api_server/utils.py

# TMP Apply ifmap client.py patch https://review.opencontrail.org/#/c/12816/2
RUN sed -i 's/ : PROTOCOL_SSLv23/ : PROTOCOL_TLSv1/' /usr/lib/python2.7/dist-packages/cfgm_common/ifmap/client.py
RUN sed -i 's/sys.version_info/False and sys.version_info/' /usr/lib/python2.7/dist-packages/cfgm_common/ifmap/client.py

# TMP Fix discovery server startup issue
RUN sed -i 's/\(.*update.*DEFAULTS.*\)/#\ \1/' /usr/lib/python2.7/dist-packages/discovery/disc_server.py

ENTRYPOINT /usr/bin/contrail-svc-monitor
