# Testing del Role nginx_wordpress con Molecule

Este documento describe cómo usar Molecule para testear automáticamente el role `nginx_wordpress`.

## Qué es Molecule

Molecule es un framework de testing para roles de Ansible que permite:
- Crear instancias de testing de forma automática
- Ejecutar el role en estas instancias
- Verificar que el resultado es el esperado
- Limpiar los recursos al finalizar

## Prerequisitos

1. **Docker**: Molecule usa Docker como driver por defecto
2. **Python 3.11+**: Para ejecutar Molecule
3. **Entorno virtual**: Para instalar las dependencias

## Instalación

Las dependencias están configuradas en el `pyproject.toml` del proyecto:

```bash
# Crear entorno virtual
python3 -m venv venv
source venv/bin/activate

# Instalar dependencias
pip install molecule molecule-plugins[docker] docker
```

## Estructura de Molecule

```
roles/nginx_wordpress/
├── molecule/
│   └── default/
│       ├── molecule.yml     # Configuración principal
│       ├── converge.yml     # Playbook para ejecutar el role
│       └── verify.yml       # Tests de verificación
```

### molecule.yml

Configuración principal que define:
- **Driver**: Docker
- **Plataforma**: Ubuntu 22.04 con systemd
- **Variables**: Configuración específica para el testing

### converge.yml

Playbook que ejecuta el role bajo test:

```yaml
---
- name: Converge
  hosts: all
  gather_facts: true
  become: true
  roles:
    - role: nginx_wordpress
```

### verify.yml

Tests que verifican que el role funciona correctamente:
- Nginx está instalado y funcionando
- PHP-FPM está instalado y funcionando
- WordPress está correctamente instalado
- Archivos de configuración existen
- Servicios responden correctamente

## Comandos de Molecule

### Listar escenarios disponibles
```bash
molecule list
```

### Crear las instancias de testing
```bash
molecule create
```

### Ejecutar el role en las instancias
```bash
molecule converge
```

### Ejecutar solo los tests de verificación
```bash
molecule verify
```

### Ejecutar el ciclo completo de testing
```bash
molecule test
```

### Ejecutar solo la sintaxis del playbook
```bash
molecule syntax
```

### Limpiar las instancias
```bash
molecule destroy
```

### Conectarse a la instancia para debugging
```bash
molecule login
```

## Flujo de Testing Completo

El comando `molecule test` ejecuta la siguiente secuencia:
1. **Lint**: Verificación de sintaxis
2. **Destroy**: Limpia instancias previas
3. **Create**: Crea nueva instancia
4. **Prepare**: Preparación adicional (si es necesario)
5. **Converge**: Ejecuta el role
6. **Idempotence**: Verifica que el role es idempotente
7. **Verify**: Ejecuta tests de verificación
8. **Destroy**: Limpia las instancias

## Tests Implementados

Los tests verifican:

1. **Instalación de paquetes**:
   - nginx
   - php-fpm y extensiones PHP

2. **Estado de servicios**:
   - nginx ejecutándose
   - php-fpm ejecutándose

3. **Estructura de archivos**:
   - Directorio de WordPress existe
   - wp-config.php configurado
   - Configuración de nginx para WordPress

4. **Funcionalidad**:
   - nginx responde en puerto 80

## Configuración de Variables

Las variables del role están configuradas en `molecule.yml`:

```yaml
inventory:
  group_vars:
    all:
      nginx_package: nginx
      php_packages:
        - php-fpm
        - php-mysql
        - php-curl
        - php-gd
        - php-mbstring
      web_user: www-data
      php_fpm_service: php8.1-fpm
      wordpress_version: "6.4"
```

## Debugging

### Ver logs detallados
```bash
molecule test -v
```

### Mantener instancia para debugging
```bash
molecule converge
molecule login
```

### Ejecutar tests específicos
```bash
molecule create
molecule converge
molecule verify
```

## Troubleshooting

### Error de permisos con Docker
```bash
sudo usermod -aG docker $USER
# Cerrar y reabrir terminal
```

### Error de systemd en container
La configuración incluye parámetros específicos para que systemd funcione en Docker:
- `privileged: true`
- `capabilities: [SYS_ADMIN]`
- `cgroupns_mode: host`

### Timeout en conexión SSH
Aumentar el timeout en molecule.yml:
```yaml
provisioner:
  config_options:
    defaults:
      timeout: 60
```

## Integración Continua

Molecule se puede integrar fácilmente en pipelines de CI/CD:

```bash
# En GitHub Actions, GitLab CI, etc.
molecule test
```

## Extensión de Tests

Para agregar más tests, editar `molecule/default/verify.yml`:

```yaml
- name: Test adicional
  uri:
    url: http://localhost/wp-admin/
    status_code: 200
```

## Recursos Adicionales

- [Documentación oficial de Molecule](https://molecule.readthedocs.io/)
- [Molecule con Docker](https://molecule.readthedocs.io/en/latest/getting-started.html#docker)
- [Testing Ansible roles](https://docs.ansible.com/ansible/latest/dev_guide/testing.html)