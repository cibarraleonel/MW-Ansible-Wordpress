# MW-Ansible-Wordpress

Automatización del despliegue de una instalación básica de WordPress con NGINX, PHP y MySQL/MariaDB en máquinas virtuales Vagrant, usando Ansible como herramienta de aprovisionamiento.

## Requisitos

- [asdf]
- [Poetry]
- [Vagrant]
- [VirtualBox]

---

## Instalación de entorno Python con ASDF y Poetry

1. Instalar Python 3.11.9 con ASDF:

   ```bash
   asdf plugin-add python
   asdf install python 3.11.9
   asdf global python 3.11.9
   ```

2. Inicializar proyecto con Poetry:

   ```bash
   poetry install
   ```

   Esto instalará las dependencias definidas en `pyproject.toml`, incluyendo Ansible.

3. Instalar los roles de Ansible Galaxy necesarios:

   ```bash
   poetry run ansible-galaxy install -r requirements.yml -p roles/galaxy
   ```



## Despliegue de máquinas virtuales

El archivo `Vagrantfile` define 3 VMs con boxes diferentes:

| VM       | Box               | IP             | S.O. Base | PHP     |
|----------|-------------------|----------------|-----------|---------|
| Ubuntu   | ubuntu/jammy64    | 192.168.56.101 | Ubuntu 22 | 8.1     |
| Debian   | debian/bookworm64 | 192.168.56.102 | Debian 12 | 8.2     |
| Rocky    | generic/rocky9    | 192.168.56.103 | Rocky 9   | 8.0     |

### Levantar las VMs y ejecutar playbook

```bash
vagrant up
```

> Esto levantará las 3 VMs y ejecutará el playbook directamente desde el `Vagrantfile`.

O, para ejecutar manualmente:

```bash
vagrant up ubuntu
```
> Luego se debe provionar las VMs usando el `inventory.ini`. Para provisionar las 3 VMs:

```bash
poetry run ansible-playbook -i inventory.ini playbook.yml
```
Y si lo queremos hacer con una especifica:

```bash
poetry run ansible-playbook -i inventory.ini playbook.yml --limit ubuntu
```

---

##  Estructura del proyecto

```
MW-Ansible-Wordpress/
├── Vagrantfile
├── inventory.ini
├── playbook.yml
├── group_vars/
│   ├── all.yml
│   ├── ubuntu.yml
│   ├── debian.yml
│   └── rocky.yml
├── roles/
│   ├── nginx_wordpress/
│   │   ├── tasks/
│   │   │   └── main.yml
│   │   ├── templates/
│   │   │   ├── wordpress.conf.j2
│   │   │   └── wp-config.php.j2
│   │   └── handlers/
│   │       └── main.yml
|   └── galaxy/
│       └── geerlingguy.mysql/
├── requirements.yml
├── pyproject.toml
└── README.md
```

---

## ⚙️ ¿Qué hace el playbook?

1. Instala NGINX.
2. Instala PHP y sus extensiones.
3. Instala MySQL/MariaDB usando el rol `geerlingguy.mysql`.
4. Descarga WordPress.
5. Genera `wp-config.php` desde plantilla Jinja2.
6. Configura y levanta servicios (`nginx`, `php-fpm`).
7. Expone el sitio en el puerto 80 para acceso web.

---

## 🌐 Acceso a WordPress

Desde tu navegador:

- [http://192.168.56.101](http://192.168.56.101) – Ubuntu
- [http://192.168.56.102](http://192.168.56.102) – Debian
- [http://192.168.56.103](http://192.168.56.103) – Rocky