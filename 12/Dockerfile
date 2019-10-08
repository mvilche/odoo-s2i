FROM alpine:3.10
ENV PYTHON_VERSION=python3 ODOO_VERSION=12.0 ODOO_REPO=https://github.com/odoo/odoo.git
LABEL autor="Martin Vilche <mfvilche@gmail.com>" \
      io.k8s.description="Odoo s2i images" \
      io.k8s.display-name="Odoo python" \
      io.openshift.tags="builder,python,s2i,odoo" \
      io.openshift.expose-services="8070" \
      org.jboss.deployments-dir="/opt/odoo/custom_addons" \
      io.openshift.s2i.scripts-url="image:///usr/libexec/s2i"
RUN apk add --no-cache --update $PYTHON_VERSION alpine-sdk bash shadow curl nodejs npm wkhtmltopdf tzdata postgresql-dev git gcc libxml2 libxslt-dev py3-lxml libxml2-dev libc-dev python3-dev jpeg-dev py3-psutil linux-headers openldap-dev tiff-dev && \
pip3 install --upgrade pip
RUN mkdir -p /opt && cd /opt && git clone --depth=1 -b $ODOO_VERSION $ODOO_REPO && cd /opt/odoo && pip3 install -r requirements.txt && npm install -g less
COPY s2i/bin/ /usr/libexec/s2i
RUN mkdir -p /opt/odoo/server-config /opt/odoo/custom_addons && rm -rf /etc/localtime  && touch /etc/timezone /etc/localtime && \
adduser -D -u 1001 -h /opt/odoo odoo && \
usermod -aG 0 odoo && \
chown 1001 -R /opt /etc/timezone /etc/localtime /usr/libexec/s2i /usr/lib/python*  /usr/include/python* /usr/bin  && \
chgrp -R 0 /opt /etc/timezone /etc/localtime /usr/libexec/s2i /usr/lib/python*  /usr/include/python* /usr/bin && \
chmod g=u -R /opt /etc/timezone /etc/localtime /usr/libexec/s2i /usr/lib/python*  /usr/include/python* /usr/bin && \
chmod +x /opt/odoo/odoo-bin /usr/libexec/s2i/* && \
chmod g+w /etc/passwd && \
rm -rf /var/cache/apk/*
WORKDIR /opt/odoo
EXPOSE 8070
ENV HOME /opt/odoo
USER 1001   
CMD ["/usr/libexec/s2i/usage"]
