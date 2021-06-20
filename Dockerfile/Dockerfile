FROM alpine
LABEL maintainer="poisonw" \
           VERSION=0.0.1
ARG PW_URL=https://github.com/PoisonWxyz/O_Scripts
ARG PW_BRANCH=main
ADD ./ /home
#ARG KEY="-----BEGIN OPENSSH PRIVATE KEY-----\n补位\n-----END OPENSSH PRIVATE KEY-----"
ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin \
    LANG=zh_CN.UTF-8 \
    SHELL=/bin/bash \
    PW_DIR=/home \
    PS1="\u@\h:\w \$ " \
    DEFAULT_LIST_FILE=crontab_list.sh \
        CUSTOM_LIST_MERGE_TYPE=append\
WORKDIR=${PW_DIR}/Scripts
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories \
    && apk update \
    && apk upgrade \
    && apk add --no-cache -f bash \
                             npm\
                             jq\
                             nodejs\
                             coreutils \
                             moreutils \
                             git \
                             wget \
                             curl \
                             nano \
                             tzdata \
                             perl \
                             openssl \
                             nginx \
                             python3 \
                             ln -sf python3 /usr/bin/python \
                             make \
                             g++ \
                             yarn \
    && rm -rf /var/cache/apk/* \
    && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone \
    && mkdir -p /root/.ssh \
    && echo -e $KEY > /root/.ssh/id_rsa \
    && chmod 600 /root/.ssh/id_rsa \
    && ssh-keyscan github.com > /root/.ssh/known_hosts \
    && git clone -b ${PW_URL} ${PW_BRANCH} ${PW_DIR} \
    && cd ${PW_DIR} \
    && mkdir -p env logs Scripts/jd Shell Scripts/oth Scripts/python \
    && cd /scripts \
    && yarn install \
    && yarn build \
    && yarn build-back \
    && yarn install --production \
    && npm config set registry https://registry.npm.taobao.org \
    && npm install \
    && cp /Shell/docker_entrypoint.sh /usr/local/bin \
    && chmod +x /usr/local/bin/docker_entrypoint.sh \
    && cp /Shell/* ${PW_DIR}/Shell
    
RUN cd ${PW_DIR}/python \
    && apk add --update --no-cache python3 \
    && ln -sf python3 /usr/bin/python \
    & wget https://bootstrap.pypa.io/get-pip.py&python get-pip.py \
    & wget https://raw.githubusercontent.com/自用/requirements.txt&pip install -r ./requirements.txt

COPY /home/Scripts/jd/* ${PW_DIR}/Scripts/jd

RUN rm -rf ${PW_DIR}/env/* ${PW_DIR}/logs/* ${PW_DIR}/Scripts/*

ENTRYPOINT ["docker_entrypoint.sh"]

CMD ["pm2-runtime"]
