# 1. Buid images
    Dir:\0.images\debian_odoo_11:  docker build -t local/odoo11c_base .

#Odoo 11:
**Project: odoo11c_source**

    docker-compose -f odoo11c-source.yml -p odoo11c_source build
    docker-compose -f odoo11c-source.yml -p odoo11c_source up -d
    docker-compose -f odoo11c-source.yml -p odoo11c_source down
    
    clean valume too
    docker-compose -f odoo11c-source.yml -p odoo11c_source down -v


    docker run --rm -ti --volumes-from odoo11c odoo11csource_odoo11c_source bash
