FROM centos

# update and install some usefull apps
RUN yum -y update && yum -y install vim wget mpg123 jackd kernel-devel

# install locale
RUN localedef --quiet -c -i ru_RU -f UTF-8 ru_RU.UTF-8

# obtain sources
RUN wget -P /root/ https://downloads.asterisk.org/pub/telephony/asterisk/asterisk-16-current.tar.gz
RUN wget -P /root/ https://downloads.asterisk.org/pub/telephony/libpri/libpri-current.tar.gz
RUN wget -P /root/ https://downloads.asterisk.org/pub/telephony/dahdi-linux-complete/dahdi-linux-complete-current.tar.gz
RUN tar xf /root/asterisk-16-current.tar.gz -C /root/
RUN tar xf /root/libpri-current.tar.gz -C /root/
RUN tar xf /root/dahdi-linux-complete-current.tar.gz -C /root/

# preinstall iksemel if you need Jabber:
#RUN wget -P /root/ http://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/e/epel-release-7-11.noarch.rpm
#RUN rpm -Uvh /root/epel-release*rpm
#RUN yum -y install iksemel-devel

# pre install and compile
ENV ASTERISK_VERSION=16.0.1
RUN cd /root/asterisk-${ASTERISK_VERSION}/;echo "Y" | /root/asterisk-${ASTERISK_VERSION}/contrib/scripts/install_prereq install
RUN cd /root/asterisk-${ASTERISK_VERSION}/;echo "Y" | /root/asterisk-${ASTERISK_VERSION}/contrib/scripts/install_prereq install-unpackaged

# install DAHDI and libpri
#RUN cd /root/dahdi-linux-complete-*/;make
#RUN cd /root/dahdi-linux-complete-*/;make install
#RUN cd /root/dahdi-linux-complete-*/;make config
#RUN cd /root/libpri-*/;make
#RUN cd /root/libpri-*/;make install

RUN cd /root/asterisk-${ASTERISK_VERSION}/;./contrib/scripts/get_mp3_source.sh
RUN cd /root/asterisk-${ASTERISK_VERSION}/;make distclean
RUN cd /root/asterisk-${ASTERISK_VERSION}/;./configure --with-jansson-bundled
ADD menuselect.makeopts /root/asterisk-${ASTERISK_VERSION}/
RUN cd /root/asterisk-${ASTERISK_VERSION}/;make
RUN cd /root/asterisk-${ASTERISK_VERSION}/;make install
RUN cd /root/asterisk-${ASTERISK_VERSION}/;make samples
RUN cd /root/asterisk-${ASTERISK_VERSION}/;make config
RUN cd /root/asterisk-${ASTERISK_VERSION}/;make install-logrotate

# cleanup
RUN rm -Rf /root/asterisk-${ASTERISK_VERSION}
RUN rm /root/asterisk-16-current.tar.gz

# add g729 codec
ADD codec_g729-ast150-gcc4-glibc-x86_64-core2-sse4.so /usr/lib/asterisk/modules/

EXPOSE 1720
EXPOSE 4569/UDP
EXPOSE 5060/UDP
EXPOSE 10000-20000/UDP

CMD    ["/usr/sbin/asterisk", "-f"]

