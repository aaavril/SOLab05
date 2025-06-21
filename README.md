# Lab 5

## Instalación de MySQL o PostgreSQL - Ubuntu (VM Azure)

### Objetivo General
Explorar e instalar motores de bases de datos populares (MySQL y PostgreSQL) en Ubuntu 22.04 instalada en la nube de Azure, y realizar operaciones básicas para afianzar el conocimiento de CLI y gestión de bases de datos.

---

## Pre-laboratorio (Investigación y Preparación)

### Investigación Breve

#### Bases de datos relacionales y NoSQL

##### Bases de Datos Relacionales (SQL)

- Las bases de datos relacionales (Structured Query Language) son el estándar para almacenar datos estructurados.
- Organizan la información en tablas interconectadas, compuestas por filas (registros) y columnas (atributos). 
- Se establecen relaciones entre estas tablas para combinar datos de diferentes fuentes.
- Utilizan SQL como el lenguaje principal para definir, manipular y consultar los datos.
- Son muy útiles para implementar operaciones transaccionales y garantizar las propiedades ACID y que las transacciones sean fiables y se mantengan en un estado válido.
- Ideales para aplicaciones que requieren integridad de datos y consistencia, como sistemas financieros.


##### Bases de Datos NoSQL (Not Only SQL)

- Las bases de datos NoSQL pueden manejar grandes volúmenes de datos estructurados y no estructurados.
- A diferencia de las relacionales, las NoSQL no requieren una estructura fija como tablas y relaciones. Sus columnas pueden ser dinámicas y se pueden realizar cambios.
- Permiten escalar horizontalmente de forma más sencilla y económica, añadiendo más nodos de almacenamiento al sistema.
- Pueden gestionar un elevado número de fuentes de datos, distintos tipos de datos (estructurados, no estructurados, semiestructurados) y grandes cantidades de datos con alta volatilidad.
- Generalmente, ofrecen consistencia eventual en lugar de las garantías ACID completas de las bases de datos relacionales. Esto significa que los datos pueden tardar un tiempo en propagarse y volverse consistentes en todo el sistema.
- Se utilizan en Big Data y Machine Learning.


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

![image](https://github.com/user-attachments/assets/9eda6474-66cb-418b-aa28-bd8cb82f0c64)
![image](https://github.com/user-attachments/assets/2d1e42f9-25ee-4294-be60-6c58921d38e9)
![image](https://github.com/user-attachments/assets/c52cee90-4422-4e6d-8293-8fb0b2e0fb05)


---

## Desarrollo del laboratorio con retos

### Instalación de MySQL y PostgreSQL

#### MySQL

```bash
sudo apt install mysql-server
sudo mysql_secure_installation
```
![image](https://github.com/user-attachments/assets/182a3806-3d36-4be6-b860-53b924dc0068)
![image](https://github.com/user-attachments/assets/1cc45015-a647-4a6b-93f6-2204d70c386a)
![image](https://github.com/user-attachments/assets/2d50056f-28b8-4aea-9311-6a7dd4a606c1)



#### PostgreSQL

```bash
sudo apt install postgresql postgresql-contrib
```

---
![image](https://github.com/user-attachments/assets/92717376-7228-4ee8-b8ae-e0be582482a3)
![image](https://github.com/user-attachments/assets/9e94ec4c-c5e9-4745-a25c-cefd15bce1c4)
![image](https://github.com/user-attachments/assets/4eb9f268-236d-4559-b2e6-719124126bb2)



### Primeros pasos con MySQL

1. Iniciar sesión como root:

   ```bash
   sudo mysql -u root -p
   ```
![image](https://github.com/user-attachments/assets/fe2b1d2c-f205-4f60-a1ad-2e8c7f22bccb)

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

   ![image](https://github.com/user-attachments/assets/700450a7-31fb-46ab-9522-dbb995e0dfca)

3. Insertar datos:

   ```sql
   INSERT INTO usuarios (id, nombre, email) VALUES
   (1, 'Juan', 'juan@example.com'),
   (2, 'María', 'maria@example.com'),
   (3, 'Pedro', 'pedro@example.com');
   ```
![image](https://github.com/user-attachments/assets/d16d73df-895b-4bee-acfe-ecd23b6ad05e)

4. Consultar datos:

   ```sql
   SELECT * FROM usuarios;
   ```
![image](https://github.com/user-attachments/assets/3cb17cd2-ade0-4551-8ffe-cda3962d07dc)

---

### Retos

- **Challenge 1 (opcional):** Repetir la creación de base y tabla en PostgreSQL usando `psql`.
![image](https://github.com/user-attachments/assets/5380f6d1-2a6b-435a-8cf0-b9f0bc435b38)
![image](https://github.com/user-attachments/assets/9a9079b7-2edf-4ab4-8f11-197348d43b9a)

- **Challenge 2:** Agregar columna `telefono VARCHAR(20)` en `usuarios` (MySQL y PostgreSQL).
![image](https://github.com/user-attachments/assets/bc6d57ad-b44f-42f7-a900-6d993f6a7a08)
![image](https://github.com/user-attachments/assets/d2d023b6-6cfd-4c7b-9e6c-e8dab15163cb)

- **Challenge 3:** Eliminar un usuario y mostrar los datos actualizados.
![image](https://github.com/user-attachments/assets/b68cdadb-52b9-48ab-abc2-b3b6973c6658)

- **Challenge 4:** Investigar y ejecutar un comando de *backup* y *restore* para MySQL o PostgreSQL.
![image](https://github.com/user-attachments/assets/93e56fae-5911-47a0-bd96-bbdbc75aba85)
![image](https://github.com/user-attachments/assets/c98dce30-10a1-4590-b055-0e5a39ee1e2b)

---

### Exploración adicional

- Consultas avanzadas: `WHERE`, `JOIN`, `GROUP BY`
- Funciones agregadas: `SUM`, `AVG`, etc.
- Relaciones entre tablas: *foreign keys*

![image](https://github.com/user-attachments/assets/e45f3cde-529e-4250-957b-2d4eb1428e56)
![image](https://github.com/user-attachments/assets/acfa470b-92fa-420d-8067-79f619a4f3ec)
![image](https://github.com/user-attachments/assets/945d6469-6621-4223-b362-1eec34a53f83)
![image](https://github.com/user-attachments/assets/71d93d16-8c90-4333-97bf-004f7b5fba1b)
![image](https://github.com/user-attachments/assets/1d4cc191-7ee3-4ce0-a1b8-5b3175b2dc4d)


---

## Conclusiones

> Me gustó este laboratorio porque me acerco a muchos conceptos que me habían interesado siempre pero no había puesto en práctica y consideraba más complejos de lo que son (aunque haya sido un acercamiento inicial).

> Las bases de datos relacionales me parecen una tecnología sumamente necesaria de comprender como profesional de software ya que hoy los datos son un recurso fundamental de gran valor. Aprender a crear, consultar y administrar bases de datos me abre las posibilidades a seguir explorando intereses por este lado relacionados a análisis de datos.
