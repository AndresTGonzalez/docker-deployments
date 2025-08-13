# Docker Deployments

Un repositorio centralizado para archivos Docker Compose que facilitan el despliegue rápido de servicios comunes en múltiples servidores.

## 🎯 Objetivo

Este proyecto centraliza configuraciones Docker Compose para servicios de infraestructura comunes, permitiendo un despliegue rápido y consistente en diferentes entornos de desarrollo, staging y producción.

## 🚀 Servicios Disponibles

### PostgreSQL
- **Versión**: PostgreSQL 16
- **Puerto**: Configurable via variable de entorno
- **Persistencia**: Datos almacenados en `/opt/postgres-data`
- **Variables de entorno requeridas**:
  - `POSTGRES_USER`: Usuario de la base de datos
  - `POSTGRES_PASSWORD`: Contraseña de la base de datos
  - `POSTGRES_DB`: Nombre de la base de datos
  - `POSTGRES_PORT`: Puerto de exposición

### Redis
- **Versión**: Redis 7.2
- **Puerto**: Configurable via variable de entorno
- **Persistencia**: Datos almacenados en `/opt/redis-data`
- **Características**: AOF (Append Only File) habilitado para persistencia
- **Variables de entorno requeridas**:
  - `REDIS_PORT`: Puerto de exposición

## 📁 Estructura del Proyecto

```
docker-deployments/
├── postgresql/
│   └── docker-compose.yml
├── redis/
│   └── docker-compose.yml
├── .gitignore
└── README.md
```

## 🛠️ Instalación y Uso

### Prerrequisitos
- Docker instalado en el servidor
- Docker Compose instalado
- Permisos de administrador para crear directorios de datos

### Despliegue de un Servicio

1. **Navegar al directorio del servicio**:
   ```bash
   cd postgresql/
   # o
   cd redis/
   ```

2. **Crear archivo de variables de entorno**:
   ```bash
   cp .env.example .env
   # Editar .env con los valores apropiados
   ```

3. **Crear directorios de datos** (si no existen):
   ```bash
   sudo mkdir -p /opt/postgres-data
   sudo mkdir -p /opt/redis-data
   sudo chown 999:999 /opt/postgres-data  # Para PostgreSQL
   sudo chown 1001:1001 /opt/redis-data   # Para Redis
   ```

4. **Iniciar el servicio**:
   ```bash
   docker-compose up -d
   ```

5. **Verificar el estado**:
   ```bash
   docker-compose ps
   docker-compose logs
   ```

### Despliegue de Múltiples Servicios

Para desplegar múltiples servicios en el mismo servidor:

```bash
# Desde el directorio raíz
cd postgresql && docker-compose up -d
cd ../redis && docker-compose up -d
```

## 🔧 Configuración

### Variables de Entorno

Cada servicio requiere un archivo `.env` con las variables apropiadas. Ejemplo para PostgreSQL:

```env
POSTGRES_USER=myuser
POSTGRES_PASSWORD=mypassword
POSTGRES_DB=mydatabase
POSTGRES_PORT=5432
```

Ejemplo para Redis:

```env
REDIS_PORT=6379
```

### Puertos por Defecto

- **PostgreSQL**: 5432
- **Redis**: 6379

## 📊 Monitoreo y Mantenimiento

### Comandos Útiles

```bash
# Ver logs en tiempo real
docker-compose logs -f

# Reiniciar un servicio
docker-compose restart

# Detener un servicio
docker-compose down

# Ver estadísticas de uso
docker stats

# Limpiar recursos no utilizados
docker system prune
```

### Backup y Restauración

#### PostgreSQL
```bash
# Backup
docker exec postgres_db pg_dump -U $POSTGRES_USER $POSTGRES_DB > backup.sql

# Restauración
docker exec -i postgres_db psql -U $POSTGRES_USER $POSTGRES_DB < backup.sql
```

#### Redis
```bash
# Los datos se persisten automáticamente en /opt/redis-data
# Para backup manual, copiar el directorio de datos
sudo cp -r /opt/redis-data /backup/redis-$(date +%Y%m%d)
```

## 🔒 Seguridad

- **No incluir archivos `.env` en el repositorio** (ya está en .gitignore)
- **Usar contraseñas fuertes** para bases de datos
- **Limitar acceso de red** configurando firewalls apropiados
- **Revisar permisos** de directorios de datos

## 🚀 Próximos Servicios

Servicios planificados para futuras versiones:
- [ ] MongoDB
- [ ] MySQL/MariaDB
- [ ] Elasticsearch
- [ ] RabbitMQ
- [ ] Nginx
- [ ] Traefik (reverse proxy)

## 🤝 Contribuciones

1. Fork el proyecto
2. Crea una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

## 📝 Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo `LICENSE` para más detalles.

## 🆘 Soporte

Si encuentras algún problema o tienes preguntas:

1. Revisa los logs del servicio: `docker-compose logs`
2. Verifica la configuración de variables de entorno
3. Asegúrate de que los puertos no estén en uso
4. Abre un issue en el repositorio

---

**Nota**: Este proyecto está diseñado para facilitar el despliegue rápido de servicios de infraestructura. Para entornos de producción, considera revisar y ajustar las configuraciones según tus necesidades específicas de seguridad y rendimiento.
