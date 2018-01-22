FROM local/odoo11c_base
MAINTAINER Viktor Vorobjov

USER root

#Odoo user
RUN adduser --system --quiet --shell=/bin/bash --home=/opt/odoo --gecos 'odoo' --group odoo
RUN chown -R odoo:odoo /opt

#Odoo source
RUN git clone --depth 1 --single-branch --branch 11.0 https://www.github.com/odoo/odoo /opt/odoo/app/odoo

# Copy odoo server config file
COPY odoo.conf /opt/odoo/app/config/
RUN chown odoo:odoo /opt/odoo/app/config/odoo.conf

#Odoo Addons
RUN mkdir /opt/odoo/app/addons_ext/
RUN mkdir /opt/odoo/addons_git/
RUN git clone --depth 1 --single-branch --branch 11.0 https://github.com/Openworx/backend_theme.git /opt/odoo/addons_git/backend_theme
RUN git clone --depth 1 --single-branch --branch 11.0 https://github.com/OCA/web.git /opt/odoo/addons_git/web

RUN ln -s /opt/odoo/addons_git/backend_theme/backend_theme_v11 /opt/odoo/app/addons_ext/backend_theme_v11
RUN ln -s /opt/odoo/addons_git/web/web_responsive /opt/odoo/app/addons_ext/web_responsive


RUN chown -R odoo:odoo /opt/odoo
RUN chown -R odoo:odoo /opt/odoo/*

ADD run.sh /run.sh
RUN chmod -v +x /run.sh

# Expose Odoo services
EXPOSE 8069 8071

# Set default user when running the container
USER odoo
CMD ["/run.sh"]
