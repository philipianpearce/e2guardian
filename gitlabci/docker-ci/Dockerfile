FROM debian:buster 

LABEL commit.e2guardian=$COMMIT build_date.e2guardian=$BUILD_DATE
COPY . /tmp/e2guardian
RUN cd /tmp/e2guardian \
    && apt-get update && apt-get install --no-install-recommends --no-install-suggests -y sed libevent-pthreads-* libssl1.1 ca-certificates \
    && adduser --no-create-home --uid 1161 --group --system e2guardian \
    && cp src/e2guardian /usr/sbin/e2guardian \
    && chmod +x /usr/sbin/e2guardian \     
    && cp -Rf configs /etc/e2guardian \
    && mkdir -p /usr/share/e2guardian/languages \
    && mkdir -p /run/e2guardian \
    && mkdir -p /var/log/e2guardian && chown -R e2guardian /var/log/e2guardian && chown -R e2guardian /etc/e2guardian && chown -R e2guardian /run/e2guardian \
    && cp -Rf data/languages /usr/share/e2guardian/ \
    && cp data/*.gif /usr/share/e2guardian/ \
    && cp data/*swf /usr/share/e2guardian/ \
    && sed -i "s/^#dockermode.*$/dockermode\ =\ on/" /etc/e2guardian/e2guardian.conf \
    && sed -i "s/^#pidfilename.*$/pidfilename\ =\ \/run\/e2guardian\/e2.pid/" /etc/e2guardian/e2guardian.conf \
    && sed -i "s/^#icapport.*$/icapport\ =\ 1344/" /etc/e2guardian/e2guardian.conf \
    && apt-get autoremove -y \
    && rm -rf /var/lib/apt/lists/* && rm -Rf /tmp/* 

COPY --chown=e2guardian:root run.sh /
RUN chmod +x /run.sh
HEALTHCHECK CMD curl localhost:8080 || exit 1
USER e2guardian
EXPOSE 8080
ENTRYPOINT ["/run.sh"]
