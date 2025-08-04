# Testing con Molecule

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

# Ansible Lint - Guía Básica

`ansible-lint` es una herramienta que ayuda a detectar y corregir problemas comunes en playbooks, roles y tareas de Ansible.

## 🎯 Finalidad

- Mejorar la calidad del código Ansible.
- Detectar errores de sintaxis, estilo y convenciones.
- Aplicar buenas prácticas recomendadas.

## 🛠️ Instalación

Si usás `poetry`, asegurate de tenerlo en el `pyproject.toml`:

```bash
# ayadirlo si no se hace por dependencia
poetry add --group=dev ansible-lint

# correr el linter
poetry run ansible-lint roles/nginx_wordpress/

# corregir errores basicos automaticamente
poetry run ansible-lint --fix roles/nginx_wordpress/
```