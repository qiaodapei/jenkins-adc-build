FROM centos:centos7.6.1810
# 镜像的作者
LABEL maintainer="<371583076@qq.com>" \
      name="adc build" \
      author="xiao.li" \
      vendor="qiaodapei" \
      version="2.176.2" \
      systemver="centos" \
      desc="centos or adc build " \
      build-date="20190812"
COPY Resource /tmp/Resource
### SET ENVIRONNEMENT
ENV LANG=zh_CN.UTF-8 \
    LANGUAGE=zh_CN.UTF-8 \
    TIME_ZONE=Asia/Shanghai
# Install language pack
RUN localedef -v -c -i en_US -f UTF-8 zh_CN.UTF-8 >/dev/null 2>&1 &&\
    ln -nfs  /usr/share/zoneinfo/Asia/Shanghai /etc/localtime &&\
    grep -q 'zh_CN.utf8' /etc/locale.conf || sed -i -E 's/^LANG=.*/LANG="zh_CN.UTF-8"/' /etc/locale.conf &&\
    yum -y install http://rpms.famillecollet.com/enterprise/remi-release-7.rpm &&\
    yum install -y ntp yum-plugin-fastestmirror vim-enhanced ntp wget bash-completion elinks lrzsz unix2dos dos2unix git unzip net-tools cronie libcurl-devel libxml2-devel glibc.i686 libstdc++-devel.i686 centos-release-scl&&\
    yum-config-manager --add-repo https://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo
    yum install -y sudo gcc-c++ make libssh2-devel mysql-community-client python python-devel python-pip java-1.8.0-openjdk openssl openssl-devel libstdc++-devel rpmdevtools \
    mock  boost-devel uuid-c++-devel libuuid-devel devtoolset-7 apache-maven glog-devel gflags-devel glog-devel lmdb-devel libatomic atomic pugixml pugixml-devel&&\
    yum localinstall -y /tmp/Resource/rpm/* && \
    mkdir -p /home/occi/rdbms && ln -s /usr/include/oracle/11.2/client64/ /home/occi/rdbms/public && ln -s /usr/lib/oracle/11.2/client64/lib/ /home/occi/lib &&\
    echo '/usr/lib/oracle/11.2/client64/lib' >/etc/ld.so.conf.d/occi.conf &&\
    echo '/usr/local/lib64/' > /etc/ld.so.conf.d/librdkafka.conf &&\
    cd /usr/lib64/ && ln -s libatomic.so.1.0.0 /usr/lib64/libatomic.so &&\
    echo 'source /opt/rh/devtoolset-7/enable' >>/etc/profile &&\
    echo 'ulimit -c unlimited' >>/etc/profile  &&\
    echo 'export ORACLE_HOME=/home/occi' >>/etc/profile &&\
    cp -f /usr/include/librdkafka/* /usr/include/ &&\
    cp -r /tmp/Resource/agent.jar /usr/bin/agent.jar && \
    cp -r /tmp/Resource/jenkins.sh /usr/local/bin/jenkins.sh && \
    chmod +x /usr/bin/agent.jar && chmod +x /usr/local/bin/jenkins.sh && \
    mkdir -p /data/jenkins_home && yum clean all && rm -fr /tmp/Resources


WORKDIR /data/jenkins_home


ENTRYPOINT ["/usr/local/bin/jenkins.sh"]
