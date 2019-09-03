# Amazon Web Services (AWS)

## Índice

- [Bases de datos](#bases-de-datos)

<a name="bases-de-datos" />

## Bases de datos

### Almacenamiento

- General purpose (SSD): Para una carga de lectura y escritura muy constante.
- Provisioned (SSD): Es para uso intensivo de operaciones de entrada y salida.

### Seguridad

Se puede habilitar o deshabilitar la encriptación de la base de datos. Además de la configuración de los grupos de seguridad en donde podremos especificar los canales de acceso y usuarios permitidos.

### Actualizaciones

Es un servicio autoadministrado y Amazon se encarga de las actualizaciones del motor, pero nosotros podemos especificar el tiempo en el cuál se pueden hacer esas actualizaciones.

### Integración IAM

Soporta de 10 a 20 conexiones por segundo.

### Monitoreo en tiempo real (Enhanced monitoring)

No está disponible para instancias small, y hay que tener en cuenta que el tráfico aumenta.

### Precio

Depende del almacenamiento y el tamaño de la instancia.

### RDS (Relational Database Service)

- Backups automáticos

Se tiene un periodo de retención de 1 a 35 días en el que se puede recuperar la información con exactitud en cualquier segundo de ese periodo. Por defecto se encuentra configurado en 7 días.

También está la opción de realizar backups manuales, tomando un snapshot manual en el momento que se decida.

Al eliminar una base de datos por defecto toma un snapshot de la base de datos en el momento de ser eliminada. También se le puede indicar que no tome ese backup si así se desea.

### Despliegue de base de dato RDS

En servicios elegimos RDS y damos click en el botón **Create database**. Esto nos abrirá una lista de motores de bases de datos que al seleccionar cada uno mostrará las características principales que ofrece.


