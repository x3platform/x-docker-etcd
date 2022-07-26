# 将自定义 x3platform/alpine 运行时用作父镜像
FROM x3platform/alpine
# 维护该镜像的用户信息
LABEL maintainer="ruanyu <ruanyu@x3platform.com>"

# 设置目标平台参数
ARG TARGETPLATFORM
ARG ETCD_VERSION=v3.4.15

ENV BASE_DIR="/opt/etcd"

# 设置容器公开的端口
EXPOSE 2379 2380

WORKDIR $BASE_DIR

# Github https://github.com/etcd-io/etcd/releases/download/${ETCD_VERSION}/etcd-${ETCD_VERSION}-${OS_NAME}-${OS_ARCH}.tar.gz
# 3.4.15
# https://github.com/etcd-io/etcd/releases/download/${ETCD_VERSION}/etcd-${ETCD_VERSION}.tar.gz -P /tmp
# DOWNLOAD http://dl.x3platform.com/vendors/etcd
RUN if [ "$TARGETPLATFORM" = "linux/amd64" ] ; then \
        export OS_NAME="linux" \ 
        && export OS_ARCH="amd64" ; \
    elif [ "$TARGETPLATFORM" = "linux/arm64" ] ; then \
        export OS_NAME="linux" \ 
        && export OS_ARCH="arm64" \ 
        && export ETCD_UNSUPPORTED_ARCH=arm64; \
    else \
        export OS_NAME="linux" \
        && export OS_ARCH="amd64"; \
    fi; \
    echo Download http://dl.x3platform.com/vendors/etcd/${ETCD_VERSION}/etcd-${ETCD_VERSION}-${OS_NAME}-${OS_ARCH}.tar.gz \
    && cd /tmp && curl -O http://dl.x3platform.com/vendors/etcd/${ETCD_VERSION}/etcd-${ETCD_VERSION}-${OS_NAME}-${OS_ARCH}.tar.gz \
    && tar -zxvf /tmp/etcd-${ETCD_VERSION}-${OS_NAME}-${OS_ARCH}.tar.gz -C /opt/ \
    && cd /opt/etcd-${ETCD_VERSION}-${OS_NAME}-${OS_ARCH} \
    && mkdir -p /opt/etcd/bin\
    && cp -p etcd etcdctl /opt/etcd/bin/ \
    && ln -s /opt/etcd/bin/etcd /usr/local/bin/etcd \
    && ln -s /opt/etcd/bin/etcdctl /usr/local/bin/etcdctl \
    && rm -rf /opt/etcd-${ETCD_VERSION}-${OS_NAME}-${OS_ARCH} \
    && rm -f /tmp/etcd-${ETCD_VERSION}-${OS_NAME}-${OS_ARCH}.tar.gz
    
ADD ./docker-entrypoint.sh /opt/etcd/

# 设置启动脚本
ENTRYPOINT ["sh", "docker-entrypoint.sh"]