# image based on fluentd v1.14-1
FROM fluentd:v1.16-1

# Use root account to use apk
USER root

# below RUN includes plugin as examples elasticsearch is not required# you may customize including plugins as you wish
RUN apk add --no-cache --update --virtual .build-deps \
        sudo build-base ruby-dev \
&& gem uninstall -I elasticsearch \
&& gem install elasticsearch -v 7.17.0 \
&& sudo gem install fluent-plugin-elasticsearch \
&& sudo gem install fluent-plugin-secure-forward \
&& sudo gem sources --clear-all \
&& apk del .build-deps \
&& rm -rf /tmp/* /var/tmp/* /usr/lib/ruby/gems/*/cache/*.gem

# copy binary start file
COPY entrypoint.sh /bin/

RUN chmod +x /bin/entrypoint.sh

USER fluent
