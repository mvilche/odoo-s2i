FROM alpine:3.9
ENV PYTHON_VERSION=python2 ODOO_VERSION=8.0 ODOO_REPO=https://github.com/odoo/odoo.git AEROOLIB_REPO=https://github.com/aeroo/aeroolib.git AEROOLIB_BRANCH=py2.x
RUN apk add --no-cache --update $PYTHON_VERSION py2-pip shadow su-exec nodejs npm wkhtmltopdf tzdata postgresql-dev git zlib zlib-dev libjpeg gcc libxml2 libxslt-dev py2-lxml libxml2-dev libc-dev python2-dev jpeg-dev linux-headers openldap-dev tiff-dev && \
pip install --upgrade pip && npm install -g less && pip install pillow -U
WORKDIR /opt
RUN git clone --depth=1 -b $ODOO_VERSION $ODOO_REPO && cd /opt/odoo && pip install -r requirements.txt && pip install pillow -U &&  pip install suds && \
pip install python-ldap -U && pip install suds-jurko && pip install PyQRCode
RUN git clone --depth=1 -b $AEROOLIB_BRANCH $AEROOLIB_REPO /tmp/aeroolib && cd /tmp/aeroolib && python setup.py install && \
rm -rf /tmp/aeroolib /tmp/odoo /var/cache/apk/*
COPY run.sh /usr/bin/run.sh
RUN mkdir -p /opt/odoo/server-config /opt/odoo/custom_addons && rm -rf /etc/localtime  && touch /etc/timezone /etc/localtime && \
adduser -D -u 1001 -h /opt/odoo odoo && \
usermod -aG 0 odoo && \
chown 1001 -R /opt /usr/bin/run.sh /etc/timezone /etc/localtime  && \
chgrp -R 0 /opt /usr/bin/run.sh /etc/timezone /etc/localtime && \
chmod g=u -R /opt /usr/bin/run.sh /etc/timezone /etc/localtime && \
chmod +x /usr/bin/run.sh && \
chmod g+w /etc/passwd && \
rm -rf /var/cache/apk/*
WORKDIR /opt/odoo
EXPOSE 8070
ENV HOME /opt/odoo
USER 1001
ENTRYPOINT ["/usr/bin/run.sh"]
CMD ["-c", "/opt/odoo/server-config/odoo.cfg"]
