FROM eclipse-temurin:8-jre

ARG kyuubi_uid=10009

USER root

ENV KYUUBI_HOME /opt/kyuubi
ENV KYUUBI_LOG_DIR ${KYUUBI_HOME}/logs
ENV KYUUBI_PID_DIR ${KYUUBI_HOME}/pid
ENV KYUUBI_WORK_DIR_ROOT ${KYUUBI_HOME}/work

RUN set -ex; \
   sed -i 's/http:\/\/deb.\(.*\)/https:\/\/deb.\1/g' /etc/apt/sources.list; \
   apt-get update && \
   apt install -y wget bash tini libc6 libpam-modules krb5-user libnss3 procps; \
   mkdir -p ${KYUUBI_HOME} ${KYUUBI_LOG_DIR} ${KYUUBI_PID_DIR} ${KYUUBI_WORK_DIR_ROOT}

COPY dist ${KYUUBI_HOME}

# setup kyuubi
RUN set -ex && \
   useradd -u ${kyuubi_uid} -g root kyuubi && \
   chmod ug+rw -R ${KYUUBI_HOME} && \
   chmod a+rwx -R ${KYUUBI_WORK_DIR_ROOT} && \
   rm -rf /var/cache/apt/*

WORKDIR ${KYUUBI_HOME}

CMD [ "./bin/kyuubi", "run" ]

USER ${kyuubi_uid}
