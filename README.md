# Proyecto-Base-de-datos
### Sergio Alejandro Gonzalez Silva
### GRUPO R4
#### MODELO ENTIDAD RELACION
![modelo_entidad_relacion](Modelo_entidad_relacion_transporte.png)
***
#### MODELO RELACIONAL
![Modelo Relacional](Modelo_relacional_transporte.png)
***
#### Creacion base de datos:
~~~sql
    CREATE DATABASE transporte;
~~~
***
#### Ingreso a la base de datos:
~~~sql
    use transporte;
~~~
***
#### Borrar bases de datos:
~~~sql
    DROP TABLE IF EXISTS Buses, Zona, Parada, ParadaXruta, Conductor, Ruta, Recorridos;
~~~
***
#### Creacion de las tablas:
~~~sql
CREATE TABLE `Buses`(
    `id_bus` INT UNSIGNED NOT NULL PRIMARY KEY,
    `placa` VARCHAR(50)
); 
CREATE TABLE `Zona`(
    `id_zona` INT UNSIGNED NOT NULL PRIMARY KEY,
    `nom_zona` VARCHAR(100) NOT NULL
);

CREATE TABLE `Parada`(
    `id_parada` INT UNSIGNED NOT NULL PRIMARY KEY,
    `nom_parada` VARCHAR(100) NOT NULL
);

CREATE TABLE `ParadaXruta`(
    `id_ruta` INT UNSIGNED NOT NULL,
    `id_parada` INT UNSIGNED NOT NULL,
    FOREIGN KEY(`id_parada`) REFERENCES `Parada`(`id_parada`)
);

CREATE TABLE `Conductor`(
    `id_conductor` INT UNSIGNED NOT NULL PRIMARY KEY,
    `nom_conductor` VARCHAR(100)  NOT NULL
);

CREATE TABLE `Ruta`(
    `id_ruta` INT UNSIGNED NOT NULL PRIMARY KEY,
    `nom_ruta` VARCHAR(100) NOT NULL,
    `tiempo_ruta` TIME NOT NULL,
    `id_zona` INT UNSIGNED,
    FOREIGN KEY(`id_zona`) REFERENCES `Zona`(`id_zona`)
);

CREATE TABLE `Recorridos`(
    `id_bus` INT UNSIGNED NOT NULL,
    `id_conductor` INT UNSIGNED NOT NULL,
    `dia` VARCHAR(100) NOT NULL,
    `id_ruta` INT UNSIGNED NOT NULL,
    FOREIGN KEY(`id_conductor`) REFERENCES `Conductor`(`id_conductor`),
    FOREIGN KEY(`id_bus`) REFERENCES `Buses`(`id_bus`),
    FOREIGN KEY(`id_ruta`) REFERENCES `Ruta`(`id_ruta`)
);
~~~
#### Poblar la base de datos:
~~~sql
INSERT INTO `Zona` 
VALUES (1,"Norte"), (2,"Sur"), (3,"Oriente"), (4,"Occidente"), (5,"Floridablanca"), (6,"Girón"), (7,"Piedecuesta");

INSERT INTO `Parada`
VALUES (1,"Colseguros"), (2,"Clínica Chicamocha"), (3,"Plaza Guarín"), (4,"Mega Mall"), (5,"UIS"), (6,"UDI"), (7,"Santo Tomás"), (8,"Boulevard Santander"),
 (9,"Búcaros"), (10,"Rosita"), (11,"Puerta del Sol"), (12,"Cacique"), (13,"Plaza Satélite"), (14,"La Sirena"), (15,"Provenza"), (16,"Fontana"), 
 (17,"Gibraltar"), (18,"Terminal"), (19,"Mutis"), (20,"Plaza Real");

INSERT INTO `Buses`
VALUES
    (1,"XVH345"),(2,"XDL965"),(3,"XFG847"),(4,"XRJ452"),(5,"XDF459"),(6,"XET554"),(7,"XKL688"),(8,"XXL757");

INSERT INTO `Conductor`
VALUES
    (1,"Andrés Manuel López Obrador"),(2,"Nicolás Maduro Moros"),(3,"Alberto Fernández"),(4,"Luiz Inácio Lula da Silva"),
    (5,"Gabriel Boric"),(6,"Miguel Díaz-Canel"),(7,"Daniel Ortega"),(8,"Gustavo Petro Urrego"),(9,"Luis Arce"),(10,"Xiomara Castro");

INSERT INTO `Ruta`
VALUES (1,"Universidades", '2:00:00',1), (2,"Café Madrid", '2:15:00',1), (3,"Cacique", '1:45:00',NULL), (4,"Diamantes", '1:50:00',4),
(5,"Terminal", '2:00:00',4), (6,"Prado", '1:30:00',NULL), (7,"Cabecera", '2:00:00',NULL), (8,"Ciudadela", '2:00:00',NULL), 
(9,"Punta Estrella", '2:30:00',NULL),(10,"Niza", '2:45:00',5), (11,"Autopista", '2:15:00',5), (12,"Lagos", '2:15:00',5), (13,"Centro Florida", '2:30:00',NULL);

INSERT INTO `Recorridos`(id_conductor, id_bus, dia, id_ruta)
VALUES 
(5, 1, "Lunes", 1),(5, 1, "Martes", 1),(5, 3, "Miércoles", 1),(5, 3, "Jueves", 1),(5, 5, "Viernes", 2),(5, 5, "Sábado", 2),(5, 5, "Domingo", 2),
(3, 5, "Lunes", 4),(3, 6, "Martes", 4),(3, 1, "Miércoles", 4),(3, 1, "Jueves", 5),(3, 3, "Viernes", 5),(3, 3, "Sábado", 5),(3, 3, "Domingo", 5),
(10, 3, "Lunes", 10),(10, 3, "Martes", 10),(10, 5, "Miércoles", 10),(10, 5, "Jueves", 10),(10, 4, "Viernes", 10),(10, 7, "Sábado", 11),(10, 7, "Domingo", 11),
(7, 7, "Lunes", 11),(7, 7, "Martes", 11),(6, 7, "Miércoles", 12),(6, 7, "Jueves", 12),(6, 7, "Viernes", 12),(6, 6, "Sábado", 12),(6, 6, "Domingo", 12);

INSERT INTO `ParadaXruta` (id_ruta, id_parada)
VALUES
(1,1),(1,2),(1,3),(1,4),(1,5),(1,6),(1,7),
(3,8),(3,9),(3,10),(3,11),(3,12),
(4,13),(4,14),(4,15),
(5,16),(5,17),
(8,18),(8,19),(8,20);
~~~
***
### CONSULTAS
***
#### 1.Cantidad de Paradas por Ruta
~~~sql
SELECT R.nom_ruta, COUNT(PXR.id_parada) AS cantidad_paradas
FROM `Ruta` R
INNER JOIN `ParadaXruta` PXR ON R.id_ruta = PXR.id_ruta
GROUP BY R.nom_ruta;
~~~
#### 2.Nombre de las Paradas de la Ruta Universidades
~~~sql
SELECT P.nom_parada AS paradas_universidades
FROM `Parada` P
INNER JOIN `ParadaXruta` PXR ON PXR.id_parada = P.id_parada 
INNER JOIN `Ruta` R ON R.id_ruta = PXR.id_ruta
WHERE R.nom_ruta = 'Universidades';
~~~
#### 3.Nombres de las Rutas No Programadas
~~~sql
SELECT R.nom_ruta AS ruta_no_programada
FROM `Ruta` R
LEFT JOIN `Recorridos` RE ON RE.id_ruta = R.id_ruta
WHERE RE.id_ruta IS NULL;
~~~
#### 4.Rutas Programadas sin Conductor Asignado
~~~sql
SELECT R.nom_ruta AS ruta_sin_conductor
FROM `Ruta` R
LEFT JOIN `Recorridos` RE ON RE.id_ruta = R.id_ruta
WHERE RE.id_conductor IS NULL;
~~~
#### 5.Conductores No Asignados a la Programación, que no estan planeados para salir en una ruta
~~~sql
SELECT C.nom_conductor AS Conductores_No_Asignados
FROM `Conductor` C
LEFT JOIN `Recorridos` R ON R.id_conductor = C.id_conductor
WHERE R.id_conductor IS NULL;
~~~
#### 6.Buses No asignados a la Programación
~~~sql
SELECT B.id_bus, B.placa
FROM `Buses` B
LEFT JOIN `Recorridos` R ON R.id_bus = B.id_bus
WHERE R.id_bus IS NULL;
~~~
#### 7.Zonas NO Programadas
~~~sql
SELECT Z.id_zona, Z.nom_zona AS zona_no_programada
FROM `Zona` Z
LEFT JOIN `Ruta` R ON R.id_zona = Z.id_zona
WHERE R.id_zona IS NULL
~~~
#### 8.Programación asignada a cada conductor (Conductor, Ruta y Día)
~~~sql
SELECT C.nom_conductor, R.dia, RU.nom_ruta
FROM `Conductor` C
INNER JOIN `Recorridos` R ON R.id_conductor = C.id_conductor
INNER JOIN `Ruta` RU ON RU.id_ruta = R.id_ruta
~~~
#### 9.Programación asignada a conductores que hacen rutas de más de dos horas
~~~sql
SELECT C.nom_conductor, R.dia, RU.nom_ruta
FROM `Conductor` C
INNER JOIN `Recorridos` R ON R.id_conductor = C.id_conductor
INNER JOIN `Ruta` RU ON RU.id_ruta = R.id_ruta
WHERE TIME_TO_SEC(RU.tiempo_ruta) > TIME_TO_SEC('02:00:00');
~~~
#### 10.Nombres de Zonas y cantidad de rutas que tienen programadas (Contar)
~~~sql
SELECT Z.nom_zona, COUNT(R.id_ruta) AS cantidad_rutas
FROM `Zona` Z
LEFT JOIN `Ruta` R ON R.id_zona = Z.id_zona
LEFT JOIN `Recorridos` RE ON RE.id_ruta = R.id_ruta
GROUP BY Z.nom_zona;
~~~
***
