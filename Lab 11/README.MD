Parte 1: Diseño de la Infraestructura en la Nube

Para diseñar una infraestructura en la nube escalable usando AWS, utlizamos algunos componentes clave:
1. Servidores (EC2)
Estos son los que ejecutan nuestra aplicación.
    Se configura un grupo de servidores que se ajuste automáticamente según la cantidad de usuarios (Auto Scaling).
    Para mayor seguridad y disponibilidad, los distribuimos en varias zonas de AWS (Availability Zones).
2. Almacenamiento (S3)
Aquí podemos guardar archivos como imágenes, videos o copias de seguridad.
    Configuramos reglas para mover los archivos menos usados a almacenamiento más barato (como Glacier).
    Activamos el versionado para protegernos si alguien borra algo por accidente.
3. Red (VPC)
Se crea una red privada para nuestra aplicación.
- Los servidores están en subredes privadas (inaccesibles desde internet).
- Un balanceador de carga (en una subred pública) se encarga de distribuir el tráfico de los usuarios hacia los servidores.
- Agregamos un "puente" (NAT Gateway) para que los servidores puedan conectarse a internet sin exponerse.

Parte 2: Configuración de IAM (Roles y Permisos)
¿Quién tiene acceso y a qué?

Definimos roles y permisos para proteger nuestros recursos:

- Desarrolladores: Solo pueden ver y probar ciertas partes del sistema (lectura limitada).
- Administradores: Tienen acceso total para gestionar y solucionar problemas.
- Servidores de la aplicación: Solo pueden acceder a lo que necesitan, como leer archivos en S3 o conectarse a la base de datos.

Seguridad Extra
- Se da a cada usuario o servidor solo lo necesario para trabajar (principio de privilegio mínimo).
- Se activa autenticación en dos pasos (MFA) para roles importantes.

Parte 3: Estrategia de Gestión de Recursos

1. Escalabilidad Automática (Auto Scaling)

Configuramos los servidores para que crezcan o se reduzcan según el número de usuarios.

- Si aumenta el tráfico, se activan más servidores.
- Si disminuye, se apagan para ahorrar costos.

2. Balanceo de Carga (ELB)

Este distribuye el tráfico entre los servidores.

- Además, revisa que los servidores estén funcionando bien antes de enviarles tráfico.
- Usamos HTTPS para asegurar las conexiones.

3. Control de Costos

- Establecemos alertas con AWS Budgets para no gastar más de lo planeado.
- Revisamos regularmente los gastos con AWS Cost Explorer.

Parte 4: ¿Cómo funciona todo junto?

El Usuario y el Tráfico Web:
- Cuando alguien entra a la aplicación, su solicitud pasa primero por un balanceador de carga (ELB).
- Este dirige la solicitud al servidor más adecuado en la red privada.

Procesamiento de la Aplicación:

Los servidores procesan las solicitudes y, si es necesario, acceden a la base de datos o recuperan archivos de S3.

- Almacenamiento y Recursos Estáticos:

Los archivos como imágenes o videos se sirven directamente desde S3 para aligerar el trabajo de los servidores.

Seguridad de la Red:
- Todo el tráfico está controlado por reglas de seguridad que bloquean accesos no autorizados.

Parte 5: Discusión y Evaluación
Definicion de  servicios
- EC2 con Auto Scaling asegura que tengamos suficiente capacidad cuando sea necesario, pero sin desperdiciar recursos.
- S3 nos da almacenamiento confiable y barato para los archivos estáticos.
- ELB y VPC ofrecen un entorno seguro y bien distribuido para manejar el tráfico.

Configuración segura
- Cada rol tiene solo los permisos mínimos necesarios, reduciendo riesgos de errores o ataques.
- Con medidas como autenticación en dos pasos y monitoreo constante, mejoramos la seguridad general.

Eficiencia en costos y escalable
- Usamos escalado automático y reglas de almacenamiento para reducir costos durante periodos de baja actividad.
- El monitoreo continuo asegura que no gastemos de más.