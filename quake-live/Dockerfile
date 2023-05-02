FROM cm2network/steamcmd:root

WORKDIR /home/steam

RUN set -x \
    && apt-get update \
    && apt-get -y install python3 python3-dev wget git build-essential \
    && wget https://bootstrap.pypa.io/get-pip.py \
    && python3 get-pip.py \
    && rm get-pip.py

USER steam

RUN set -x \
    && ./steamcmd/steamcmd.sh +login anonymous +app_update 349090 +quit \
    && ln -s "Steam/steamapps/common/Quake Live Dedicated Server/" ql \
    && git clone https://github.com/MinoMino/minqlx.git \
    && cd minqlx \
    && make \
    && cd ~ \
    && cp minqlx/bin/* ql \
    && cd ql \
    && git clone https://github.com/MinoMino/minqlx-plugins.git \
    && python3 -m pip install -r minqlx-plugins/requirements.txt

COPY --chown=steam:steam server.cfg /home/steam/ql/baseq3

CMD \
    export NET_PORT=${NET_PORT:-27960} \
    && export ZMQ_STATS_PORT=${ZMQ_STATS_PORT:-$NET_PORT} \
    && export ZMQ_RCON_PORT=${ZMQ_RCON_PORT:-$(($NET_PORT + 1000))} \
    && ./ql/run_server_x64_minqlx.sh \
    ${NET_PORT:++set net_port ${NET_PORT}} \
    ${ZMQ_RCON_PORT:++set zmq_rcon_port ${ZMQ_RCON_PORT}} \
    ${ZMQ_STATS_PORT:++set zmq_stats_port ${ZMQ_STATS_PORT}} \
    ${SV_HOSTNAME:++set sv_hostname ${SV_HOSTNAME}}