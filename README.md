# Lab 5

## Instalación de MySQL o PostgreSQL - Ubuntu (VM Azure)

### Objetivo General
Explorar e instalar motores de bases de datos populares (MySQL y PostgreSQL) en Ubuntu 22.04 instalada en la nube de Azure, y realizar operaciones básicas para afianzar el conocimiento de CLI y gestión de bases de datos.

---

## Pre-laboratorio (Investigación y Preparación)

### Investigación Breve

#### Bases de datos relacionales y NoSQL

##### Bases de Datos Relacionales (SQL)

- Son el estándar para almacenar datos estructurados.
- Organizan la información en tablas interconectadas (filas y columnas).
- Establecen relaciones entre tablas.
- Utilizan SQL como lenguaje principal.
- Soportan propiedades **ACID** para transacciones fiables.
- Ideales para aplicaciones que requieren **integridad** y **consistencia**.

##### Bases de Datos NoSQL (Not Only SQL)

- Manejan grandes volúmenes de datos estructurados y no estructurados.
- No requieren una estructura fija.
- Escalan horizontalmente más fácilmente.
- Admiten múltiples tipos de datos y fuentes.
- Ofrecen **consistencia eventual** (no ACID).
- Usadas en **Big Data** y **Machine Learning**.

---

### Comparativa: Relacionales vs. NoSQL

| Característica      | Bases de Datos Relacionales (SQL)                                          | Bases de Datos NoSQL                                                                 |
|---------------------|----------------------------------------------------------------------------|---------------------------------------------------------------------------------------|
| Modelo de Datos     | Estructura rígida en tablas, filas y columnas. Requiere un esquema fijo.   | Estructura flexible o sin esquema. No requiere un esquema fijo.                      |
| Relaciones          | Establecen relaciones explícitas entre tablas.                             | Generalmente, no existen relaciones explícitas entre colecciones.                    |
| Escalabilidad       | Escalan principalmente de forma vertical (más recursos en un servidor).    | Escalan horizontalmente (distribuyen la carga entre servidores, gestionado por el proveedor). |
| Consistencia        | Ofrecen consistencia fuerte y cumplen con las propiedades ACID.            | Generalmente, ofrecen consistencia eventual.                                          |
| Consultas           | Utilizan SQL para consultas complejas y uniones (JOINs).                   | Consultas sencillas y rápidas; uniones limitadas o no soportadas.                    |
| Tipos de Datos      | Predominan los datos estructurados.                                        | Manejan datos estructurados, semiestructurados y no estructurados.                   |


---

### Motores de bases de datos más usados en 2025

| Base de Datos | Tipo / Modelo                         | Ventajas                                                                                                                                  | Desventajas                                                                                                         | Casos de Uso                                                                                   |
|---------------|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|
| MySQL         | Relacional (RDBMS) / Cliente-servidor | - Fácil de usar e instalar<br>- Buen rendimiento para apps web<br>- Comunidad amplia<br>- Gratuito (GPL)                                 | - Menos potente en escrituras intensivas<br>- Limitado frente a PostgreSQL en funciones avanzadas                  | Webs con alto tráfico de lectura (WordPress / Joomla / e-commerce / LAMP stack)                |
| MariaDB       | Relacional (RDBMS) / Fork de MySQL    | - 100% open source (GPL)<br>- Mejoras de rendimiento<br>- Motores de almacenamiento extra<br>- Migración sencilla desde MySQL            | - Menor base de usuarios que MySQL<br>- Soporte empresarial limitado frente a Oracle                               | Proyectos de alto rendimiento (Google, Wikipedia, Alibaba)                                     |
| PostgreSQL    | Objeto-relacional (ORDBMS)            | - Muy robusto y estable<br>- Extensible<br>- Compatible con SQL<br>- Soporta JSON / XML / GIS (PostGIS)<br>- Licencia permisiva          | - Más complejo para principiantes<br>- Menor presencia en hostings simples<br>- Menos ágil para operaciones simples | Apps con consultas complejas / análisis de datos / apps geoespaciales / data warehouse         |
| SQLite        | Relacional embebido (motor ligero)    | - Súper liviano y portable<br>- Multiplataforma<br>- Sin configuración de servidor<br>- Ideal para apps pequeñas o móviles               | - No escalable<br>- Sin control de usuarios<br>- Monousuario<br>- Limitado en tipos de datos                        | Apps móviles / Sitios simples / Apps locales / Páginas estáticas                               |
| MongoDB       | NoSQL / Orientado a documentos        | - Muy flexible<br>- Escalable horizontalmente<br>- Rápido para datos semiestructurados<br>- Ideal para cambios frecuentes                | - No garantiza ACID completo<br>- No tiene JOINs clásicos<br>- Requiere perfil técnico más avanzado                | Big Data / Web 2.0 / redes sociales / sistemas documentales / apps dinámicas                   |
| Oracle DB     | Relacional (RDBMS) / Multimodelo      | - Altamente escalable y seguro<br>- Soporte para múltiples tipos de datos<br>- Alta disponibilidad (RAC)<br>- Soporte técnico sólido     | - Licencia muy costosa<br>- Configuración compleja<br>- Requiere personal especializado                             | Grandes corporaciones / OLTP / transacciones críticas / bancos y gobiernos                     |
| SQL Server    | Relacional (RDBMS) / Microsoft        | - Integración total con .NET, Azure y Windows<br>- Herramientas BI (SSAS, SSIS, SSRS)<br>- Seguridad avanzada<br>- SSMS amigable         | - Licencia cara (Standard / Enterprise)<br>- Históricamente enfocado a Windows (aunque ya soporta Linux)           | Empresas con infraestructura Microsoft / apps .NET / BI / data warehousing                     |


---

## Requisitos previos del sistema

- Tener Ubuntu 22.04 y espacio suficiente.
- Conexión a internet estable.
- Actualizar sistema:

  ```bash
  sudo apt update && sudo apt upgrade
  ```

- En Windows, descargar:
  - [MySQL Workbench](https://dev.mysql.com/downloads/workbench/)
  - [pgAdmin](https://www.pgadmin.org/download/)

- Abrir puerto 3306 (MySQL) en el firewall del grupo de seguridad de Azure.

---

## Desarrollo del laboratorio con retos

### Instalación de MySQL y PostgreSQL

#### MySQL

```bash
sudo apt install mysql-server
sudo mysql_secure_installation
```

#### PostgreSQL

```bash
sudo apt install postgresql postgresql-contrib
```

---

### Primeros pasos con MySQL

1. Iniciar sesión como root:

   ```bash
   sudo mysql -u root -p
   ```

2. Crear base de datos y tabla:

   ```sql
   CREATE DATABASE ejemplo;
   USE ejemplo;
   CREATE TABLE usuarios (
       id INT PRIMARY KEY,
       nombre VARCHAR(50),
       email VARCHAR(100)
   );
   ```

3. Insertar datos:

   ```sql
   INSERT INTO usuarios (id, nombre, email) VALUES
   (1, 'Juan', 'juan@example.com'),
   (2, 'María', 'maria@example.com'),
   (3, 'Pedro', 'pedro@example.com');
   ```

4. Consultar datos:

   ```sql
   SELECT * FROM usuarios;
   ```

---

### Retos

- **Challenge 1 (opcional):** Repetir la creación de base y tabla en PostgreSQL usando `psql`.
- **Challenge 2:** Agregar columna `telefono VARCHAR(20)` en `usuarios` (MySQL y PostgreSQL).
- **Challenge 3:** Eliminar un usuario y mostrar los datos actualizados.
- **Challenge 4:** Investigar y ejecutar un comando de *backup* y *restore* para MySQL o PostgreSQL.

---

### Exploración adicional

- Consultas avanzadas: `WHERE`, `JOIN`, `GROUP BY`
- Funciones agregadas: `SUM`, `AVG`, etc.
- Relaciones entre tablas: *foreign keys*

---

## Conclusiones

> Me gustó este laboratorio porque me acerco a muchos conceptos que me habían interesado siempre pero no había puesto en práctica y consideraba más complejos de lo que son (aunque haya sido un acercamiento inicial).

> Las bases de datos relacionales me parecen una tecnología sumamente necesaria de comprender como profesional de software ya que hoy los datos son un recurso fundamental de gran valor. Aprender a crear, consultar y administrar bases de datos me abre las posibilidades a seguir explorando intereses por este lado relacionados a análisis de datos.
