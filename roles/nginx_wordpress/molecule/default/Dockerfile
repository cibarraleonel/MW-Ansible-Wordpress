FROM ubuntu:22.04

# Evitar prompts interactivos durante la instalación
ENV DEBIAN_FRONTEND=noninteractive

# Actualizar e instalar Python3 y certificados SSL
RUN apt-get update && \
    apt-get install -y \
    python3 \
    python3-pip \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Crear enlace simbólico para python
RUN ln -sf /usr/bin/python3 /usr/bin/python

# Comando por defecto
CMD ["sleep", "infinity"] 