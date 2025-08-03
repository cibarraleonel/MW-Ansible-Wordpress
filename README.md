# MW-Ansible-Wordpress

Automatización del despliegue de una instalación básica de WordPress con NGINX, PHP y MySQL/MariaDB en máquinas virtuales Vagrant, usando Ansible como herramienta de aprovisionamiento.

## Requisitos

- [asdf]
- [Poetry]
- [Docker]

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



## Testing con Molecule

El proyecto incluye testing automatizado usando Molecule con Docker:

### Ejecutar tests

```bash
# Instalar dependencias
poetry install

# Ejecutar todas las pruebas
poetry run molecule test

# Solo crear el contenedor de prueba
poetry run molecule create

# Solo ejecutar el rol
poetry run molecule converge

# Solo verificar que funciona
poetry run molecule verify

# Limpiar contenedores
poetry run molecule destroy
```

### Configuración de testing

- **Plataforma**: Ubuntu 22.04 (Docker)
- **Driver**: Docker
- **Verificaciones**: Nginx, PHP-FPM, WordPress

---

##  Estructura del proyecto

```
MW-Ansible-Wordpress/
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
│   │   ├── handlers/
│   │   │   └── main.yml
│   │   ├── molecule/
│   │   │   ├── default/
│   │   │   │   ├── molecule.yml
│   │   │   │   ├── converge.yml
│   │   │   │   ├── verify.yml
│   │   │   │   ├── requirements.yml
│   │   │   │   └── ansible.cfg
│   │   │   └── README.md
│   │   └── meta/
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

Para acceder a WordPress después de ejecutar el rol, necesitarás configurar el acceso según tu entorno de despliegue.

## 🧪 Testing

El proyecto incluye testing automatizado con Molecule que verifica:

- ✅ Instalación de Nginx
- ✅ Instalación de PHP-FPM
- ✅ Descarga de WordPress
- ✅ Configuración de permisos
- ✅ Verificación de servicios