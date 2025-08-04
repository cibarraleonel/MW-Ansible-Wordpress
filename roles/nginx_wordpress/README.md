# Ansible Role: nginx_wordpress

Este rol instala y configura WordPress utilizando Nginx y PHP-FPM en sistemas Linux compatibles. Está diseñado para funcionar tanto con bases de datos locales como remotas (por ejemplo, AWS RDS).

---

## ¿Qué hace este rol?

- Instala y configura Nginx.
- Instala PHP y las extensiones necesarias.
- Descarga y despliega WordPress.
- Crea el archivo `wp-config.php` con la configuración de base de datos.
- Prepara todo para que WordPress quede funcional con PHP-FPM.

---

## Requisitos del entorno

- Ansible ≥ 11.0
- Python ≥ 3.11 en el host de control
- Docker (solo si usás Molecule para testing)
- Acceso root (`become: true`)
- Compatible con: **Ubuntu**, **Debian** y **Rocky Linux**

---

## Variables requeridas

Estas variables deben definirse en tu inventario, `group_vars/all.yml`, o directamente en el playbook:

```yaml
mysql_databases:
  - name: wordpress
    encoding: utf8mb4
    collation: utf8mb4_general_ci

mysql_users:
  - name: wpuser
    host: localhost
    password: secret123
    priv: "wordpress.*:ALL"

mysql_root_password: rootpass
