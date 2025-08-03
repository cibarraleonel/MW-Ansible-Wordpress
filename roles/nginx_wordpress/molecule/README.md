# Testing con Molecule

Este directorio contiene las pruebas automatizadas para el rol `nginx_wordpress` usando Docker.

## ¿Qué es Molecule?

Molecule es una herramienta que nos ayuda a probar nuestros roles de Ansible de forma automática. Crea contenedores Docker temporales, ejecuta nuestro rol y verifica que todo funcione correctamente.

## Archivos importantes

- `molecule.yml` - Configuración de los contenedores de prueba
- `converge.yml` - Ejecuta nuestro rol
- `verify.yml` - Verifica que todo funciona
- `requirements.yml` - Dependencias del rol
- `ansible.cfg` - Configuración de Ansible

## Comandos básicos

```bash
# Instalar dependencias de Molecule
poetry install

# Ejecutar todas las pruebas
poetry run molecule test

# Solo crear los contenedores
poetry run molecule create

# Solo ejecutar el rol (sin verificar)
poetry run molecule converge

# Solo verificar que todo funciona
poetry run molecule verify

# Limpiar los contenedores
poetry run molecule destroy
```

## ¿Qué contenedores se prueban?

- **Ubuntu 22.04** (contenedor Docker)

## ¿Qué se verifica?

- ✅ Nginx está instalado y funcionando
- ✅ PHP-FPM está instalado y funcionando
- ✅ WordPress está descargado
- ✅ Los permisos son correctos
- ✅ Los servicios se inician automáticamente

## Requisitos

- Docker instalado y funcionando
- Poetry con las dependencias instaladas 