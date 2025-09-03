FROM node:22-trixie-slim

ENV DEBIAN_FRONTEND=noninteractive

ADD ./entrypoint.sh /root/entrypoint.sh
RUN yarn global add pm2 hydrooj @hydrooj/ui-default
RUN chmod +x /root/entrypoint.sh && \
    mkdir -p /root/.hydro

ENTRYPOINT ["/root/entrypoint.sh"]