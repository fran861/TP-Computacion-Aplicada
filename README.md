# Trabajo Práctico Integrador Grupal — Computación Aplicada

Universidad de Palermo · 2º cuatrimestre 2026

## Integrantes

- [Nombre y Apellido 1]
- [Nombre y Apellido 2]
- [Nombre y Apellido 3]
- [Nombre y Apellido 4]
- [Nombre y Apellido 5]

## Contenido del repositorio

| Archivo | Contenido |
|---|---|
| `root.tar.gz` | Directorio `/root` (claves SSH, material del TP y scripts usados) |
| `etc.tar.gz` | Directorio `/etc` (hostname, red, fstab, crontab, SSH, Apache) |
| `opt.tar.gz` | Directorio `/opt` (`/opt/scripts/backup_full.sh` y `/opt/particion`) |
| `www_dir.tar.gz` | Partición `/www_dir` (sitio web: `index.php` y `logo.png`) |
| `backup_dir.tar.gz.part_aa`, `backup_dir.tar.gz.part_ab` | Partición `/backup_dir` (backups generados por el script), dividida en 2 partes |
| `var.tar.gz.part_aa` … `var.tar.gz.part_ak` | Directorio `/var` dividido en 11 partes |

### Cómo volver a unir `/var` y `/backup_dir`

Los archivos se dividieron con `split` porque GitHub no acepta archivos grandes subidos desde la web. Para reconstruirlos:

```bash
cat var.tar.gz.part_* > var.tar.gz
cat backup_dir.tar.gz.part_* > backup_dir.tar.gz
tar -xzf var.tar.gz
tar -xzf backup_dir.tar.gz
```

## Resumen de lo realizado

1. **Entorno:** blanqueo de la clave de root (`palermo`) editando GRUB con `init=/bin/bash`, y hostname `TPServer`.
2. **Servicios:** actualización a Debian 12 (bookworm); SSH con acceso de root por clave pública/privada; Apache + PHP 8.2 sirviendo `index.php` y `logo.png`; MariaDB con `db.sql` cargado (base `ingenieria`).
3. **Red:** IP estática `192.168.100.61/24`, gateway `192.168.100.1` en `/etc/network/interfaces`.
4. **Almacenamiento:** disco nuevo de 10 GB con dos particiones tipo 83: `/www_dir` (3 GB) y `/backup_dir` (6 GB), montadas desde `/etc/fstab`. Apache apunta a `/www_dir`. Copia de `/proc/partitions` en `/opt/particion`.
5. **Backup:** script `/opt/scripts/backup_full.sh` (origen, destino, `-help`, validaciones, fecha ANSI) programado en `/etc/crontab`: todos los días 00:00 (`/var/log`) y lunes, miércoles y viernes 23:00 (`/www_dir`).
