version: "3.8"

services:
  odoo:
    image: odoo:18
    ports:
      - "8069:8069"         # Odoo

    volumes:
      - /root/jupyter/filestore:/var/lib/odoo
      - /root/jupyter/addons:/mnt/extra-addons
      - /root/jupyter/conf:/etc/odoo
    environment:
      - HOST=db
      - USER=odoo
      - PASSWORD=odoo
    depends_on:
      - db

  db:
    image: postgres:13
    environment:
      POSTGRES_DB: odoo
      POSTGRES_USER: odoo
      POSTGRES_PASSWORD: odoo
    volumes:
      - db_data:/var/lib/postgresql/data


  jupyter:
    image: jupyter/base-notebook
    container_name: jupyter-notebook
    ports:
      - "8888:8888"
    volumes:
      - /root/jupyter/:/home/jovyan/work
    command: start-notebook.sh --NotebookApp.token=''

volumes:
  db_data:
