# Ejemplo de Ejecución de Molecule

Este archivo muestra ejemplos de cómo se vería la ejecución de Molecule en un entorno con Docker funcionando.

## Estado Actual de la Configuración

✅ Molecule está correctamente configurado para el role `nginx_wordpress`  
✅ Archivos de configuración creados:
- `molecule/default/molecule.yml`
- `molecule/default/converge.yml`  
- `molecule/default/verify.yml`

❌ Docker daemon no disponible en este entorno

## Ejemplo de Ejecución Exitosa

### 1. Listar escenarios

```bash
$ molecule list
WARNING  Driver docker does not provide a schema.
INFO     Running default > list
              ╷             ╷             ╷              ╷         ╷
  Instance    │             │ Provisioner │ Scenario     │         │
  Name        │ Driver Name │ Name        │ Name         │ Created │ Converged
╶─────────────┼─────────────┼─────────────┼──────────────┼─────────┼───────────╴
  instance    │ docker      │ ansible     │ default      │ false   │ false
              ╵             ╵             ╵              ╵         ╵
```

### 2. Verificación de sintaxis (requiere Docker)

```bash
$ molecule syntax
INFO     default scenario test matrix: syntax
INFO     Performing prerun...
INFO     Running default > syntax
INFO     Passed syntax validation
```

### 3. Crear instancia de testing

```bash
$ molecule create
INFO     default scenario test matrix: create
INFO     Performing prerun...
INFO     Running default > create
INFO     Starting instances...
INFO     Provisioning instances...
INFO     Creating image...
INFO     Instance(s) created.
```

### 4. Ejecutar el role

```bash
$ molecule converge
INFO     default scenario test matrix: converge
INFO     Performing prerun...
INFO     Running default > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [instance]

TASK [nginx_wordpress : Mostrar distribución y familia del sistema operativo] *
ok: [instance] => {
    "msg": "Instalando en Ubuntu (Debian)"
}

TASK [nginx_wordpress : Actualizar caché de apt] ******************************
changed: [instance]

TASK [nginx_wordpress : Instalar nginx] ***************************************
changed: [instance]

TASK [nginx_wordpress : Instalar PHP y extensiones necesarias] ***************
changed: [instance]

[... más tareas ...]

PLAY RECAP *********************************************************************
instance                   : ok=15   changed=12   unreachable=0    failed=0
```

### 5. Ejecutar tests de verificación

```bash
$ molecule verify
INFO     default scenario test matrix: verify
INFO     Performing prerun...
INFO     Running default > verify

PLAY [Verify] ******************************************************************

TASK [Verificar que nginx está instalado] *************************************
ok: [instance]

TASK [Confirmar que nginx está instalado] *************************************
ok: [instance] => {
    "msg": "nginx está correctamente instalado"
}

TASK [Verificar que php-fpm está instalado] ***********************************
ok: [instance] => {
    "msg": "php-fpm está correctamente instalado"
}

[... más verificaciones ...]

PLAY RECAP *********************************************************************
instance                   : ok=15   changed=0    unreachable=0    failed=0
```

### 6. Test completo

```bash
$ molecule test
INFO     default scenario test matrix: destroy, create, converge, verify, destroy
INFO     Performing prerun...
INFO     Running default > destroy
INFO     Running default > create
INFO     Running default > converge
INFO     Running default > verify
INFO     Running default > destroy
INFO     Verifying...
Molecule test completed successfully.
```

## Verificaciones Implementadas

Los tests de `verify.yml` verifican:

### ✅ Instalación de Paquetes
- nginx package instalado
- php-fpm package instalado
- Extensiones PHP necesarias

### ✅ Estado de Servicios
- nginx service running
- php-fpm service running

### ✅ Estructura de Archivos
- `/var/www/html/wordpress/` existe
- `/var/www/html/wordpress/wp-config.php` existe
- `/etc/nginx/conf.d/wordpress.conf` existe

### ✅ Funcionalidad Web
- nginx responde en puerto 80
- Códigos de estado HTTP válidos (200, 301, 302, 403)

## Para Ejecutar en un Entorno con Docker

1. **Instalar Docker**:
   ```bash
   # Ubuntu/Debian
   sudo apt install docker.io
   sudo systemctl start docker
   sudo usermod -aG docker $USER
   ```

2. **Activar entorno virtual**:
   ```bash
   source venv/bin/activate
   ```

3. **Ejecutar tests**:
   ```bash
   cd roles/nginx_wordpress
   molecule test
   ```

## Beneficios del Testing con Molecule

1. **Automatización**: Tests automáticos en cada cambio
2. **Consistencia**: Mismo entorno de testing siempre
3. **Verificación**: Confirma que el role hace lo que debe hacer
4. **Idempotencia**: Verifica que el role se puede ejecutar múltiples veces
5. **CI/CD**: Integración fácil en pipelines de deployment

## Archivos de Configuración Creados

```
roles/nginx_wordpress/
├── molecule/
│   └── default/
│       ├── molecule.yml        # Configuración principal
│       ├── converge.yml        # Playbook de testing
│       └── verify.yml          # Tests de verificación
├── MOLECULE_TESTING.md         # Documentación completa
└── EJEMPLO_EJECUCION.md        # Este archivo
```

## Próximos Pasos

1. Ejecutar en entorno con Docker disponible
2. Extender tests para verificar configuración de WordPress
3. Agregar tests de performance
4. Configurar CI/CD pipeline
5. Agregar tests de seguridad