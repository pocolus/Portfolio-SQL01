# Portfolio-SQL01

**NOTA:**
Basicamente este proyecto lo iniciamos desde cero, ya que la idea es implementar varias funciones de SQL como views, funciones, procedimientos, triggers, DCL y TCL. Para asi poder ver todo lo que nos puede ofrecer SQL, en cualquier proyecto de datos que tengamos.

**•	TITULO: TIENDA DE ELECTRONICOS Y HOGAR.**

**•	DESCRIPCION DEL PROYECTO:** En la siguiente base de datos, encontramos la información de una tienda comercial que vende diferentes tipos de productos, especialmente de tecnología y hogar. Esta tienda está ubicada en varias ciudades a nivel mundial. Acá podemos evidenciar sus ventas del mes de marzo de 2024, en cada una de sus sucursales.

**•	OBJETIVOS:**
El objetivo primordial seria desarrollar una base de datos, que nos permita hacer un seguimiento apropiado, del funcionamiento de la compañía. Para ello queremos saber cuáles son los productos más vendidos, las sucursales con más ventas, los empleados más dinámicos con respecto a las ventas, los clientes más activos y a su vez, que sucursales no generar tanto movimiento etc.

**•	HERRAMIENTAS O LENGUAJES:** Básicamente para este proyecto utilizamos la herramienta MySQL, en donde hicimos creacion de tablas, insercion de datos, views, funciones, procedimientos,
triggers, DCL y TCL.

**•	CONJUNTO DE DATOS:** Basicamente la base de datos trata de como han sido las ventas de la empresa. Tenemos tablas de ventas, clientes, vendedores, localizacion, sucursales, productos,
proveedores, catalogo-proveedor, tipo de pago y detalle-venta, toda esta coleccion de datos nos permite poder analizar y a su vez desarrollar estrategias que nos permitan tener mejores resultados en la empresa.

**•	DESARROLLO-EJECUCION:**

![DIAGRAMA-ENTIDAD-RELACION](https://github.com/pocolus/Portfolio-SQL01/blob/main/Imagen1.png)

**1. CREAMOS LA BASE DE DATOS**
```sql
DROP schema if exists Proyecto_Tienda_Electronicos;
CREATE schema if not exists Proyecto_Tienda_Electronicos;
USE Proyecto_Tienda_Electronicos;
```

**2. CREACION DE TABLAS**
```sql
-- 2.1 CREAMOS LA TABLA CLIENTE
DROP TABLE if exists CLIENTES;
CREATE table IF not exists CLIENTES
(ID_CLIENTE int not null auto_increment,
DOCUMENTO int not null,
NOMBRE varchar (50) not null,
APELLIDO varchar (50) not null,
EDAD int not null,
GENERO enum ('M','F') NOT null,
TIPO_CLIENTE enum ('Normal', 'Miembro') not null,
TELEFONO varchar (20) not null,
CORREO varchar (50) not null,
primary key (ID_CLIENTE)
);

-- 2.2 CREAMOS LA TABLA PROVEEDOR
DROP TABLE if exists PROVEEDOR;
CREATE TABLE if not exists PROVEEDOR
(ID_PROVEEDOR int not null auto_increment,
NOMBRE varchar (50) not null,
TELEFONO varchar (20) not null,
UBICACION varchar (50) not null,
primary key (ID_PROVEEDOR)
);

-- 2.3 CREAMOS LA TABLA PRODUCTO
DROP TABLE if exists PRODUCTO;
CREATE TABLE if not exists PRODUCTO
(ID_PRODUCTO int not null auto_increment,
LINEA_PRODUCTO varchar (50) not null,
NOMBRE varchar (50) not null,
PRECIO int not null,
primary key (ID_PRODUCTO)
);

-- 2.4 CREAMOS LA TABLA LOCALIZACION
DROP TABLE if exists LOCALIZACION;
CREATE table IF NOT exists LOCALIZACION (
ID_LOCALIZACION int not null auto_increment,
PAIS varchar (50) not null,
CIUDAD varchar (50) not null,
CONTINENTE varchar (50) not null,
primary key (ID_LOCALIZACION)
);

-- 2.5 CREAMOS LA TABLA TIPO_PAGO
DROP table if exists TIPO_PAGO;
CREATE table IF not exists TIPO_PAGO (
ID_TIPO_PAGO int not null auto_increment,
TIPO_PAGO varchar (50) not null,
primary key (ID_TIPO_PAGO)
);

-- 2.6 CREAMOS LA TABLA CATALOGO-PROVEEDOR
 DROP TABLE if exists CATALOGO_PROVEEDOR;
CREATE TABLE if not exists CATALOGO_PROVEEDOR
(ID_CATALOGO int not null auto_increment,
MARCA varchar (50) not null,
CANTIDAD int not null,
ID_PRODUCTO int not null,
ID_PROVEEDOR int not null,
primary key (ID_CATALOGO),
constraint FK_CATALOGO_PRODUCTO foreign key (ID_PRODUCTO) references PRODUCTO (ID_PRODUCTO),
constraint FK_CATALOGO_PROVEEDOR foreign key (ID_PROVEEDOR) references PROVEEDOR (ID_PROVEEDOR)
);

-- 2.7 CREAMOS LA TABLA SUCURSAL
DROP TABLE if exists SUCURSAL; 
CREATE TABLE IF not exists SUCURSAL 
(ID_SUCURSAL int not null auto_increment,
NOMBRE varchar (50) not null,
ID_LOCALIZACION int not null,
TELEFONO varchar (20) not null,
ENCARGADO varchar (50) not null,
primary key (ID_SUCURSAL),
constraint FK_SUCURSAL_LOCALIZACION foreign key (ID_LOCALIZACION) references LOCALIZACION (ID_LOCALIZACION)
);

-- 2.8 CREAMOS LA TABLA VENDEDORES
DROP TABLE if exists VENDEDORES;
create table if not exists VENDEDORES 
(ID_VENDEDOR int not null auto_increment,
NOMBRE varchar (50) not null,
APELLIDO varchar (50) not null,
EDAD int not null,
TELEFONO varchar (20) not null,
ID_SUCURSAL int not null,
FECHA_INICIO date not null,
primary key (ID_VENDEDOR),
constraint FK_VENDEDORES_SUCURSAL foreign key (ID_SUCURSAL) references SUCURSAL (ID_SUCURSAL)
);

-- 2.9 CREAMOS LA TABLA DETALLE-VENTA
DROP TABLE if exists DETALLE_VENTA;
create table if not exists DETALLE_VENTA
( ID_REGISTRO_VENTA int not null auto_increment,
ID_PRODUCTO int not null,
ID_VENTAS int not null,
CANTIDAD int not null,
primary key (ID_REGISTRO_VENTA),
constraint FK_DETALLE_PRODUCTO foreign key (ID_PRODUCTO) references producto (ID_PRODUCTO),
constraint FK_DETALLE_VENTAS foreign key (ID_VENTAS) references ventas (ID_VENTAS)
);

-- 2.10 CREAMOS LA TABLA VENTAS
DROP TABLE IF exists VENTAS;
CREATE TABLE IF NOT exists VENTAS (
ID_VENTAS int not null auto_increment,
ID_SUCURSAL int not null,
ID_CLIENTE int not null,
ID_VENDEDOR int not null,
Total int not null,
ID_TIPO_PAGO int not null,
FECHA_VENTAS date not null,
primary key (ID_VENTAS),
constraint FK_VENTAS_SUCURSAL foreign key (ID_SUCURSAL) references SUCURSAL (ID_SUCURSAL),
constraint FK_VENTAS_CLIENTES foreign key (ID_CLIENTE) references CLIENTES (ID_CLIENTE),
constraint FK_VENTAS_VENDEDOR foreign key (ID_VENDEDOR) references VENDEDORES (ID_VENDEDOR),
constraint Fk_VENTAS_TPAGO foreign key (ID_TIPO_PAGO) references TIPO_PAGO (ID_TIPO_PAGO)
);
```
**3. INSERCION DE ARCHIVOS**
```sql
/*En la inserción de datos, decidir hacer el script de SQL con cinco tablas, las cuales son: Proveedor, Localización, Tipo-Pago, Catalogo-Proveedor y Sucursal.
Para las tablas: Cliente, Producto, Vendedores, Detalle-Venta y Ventas, hice la inserción con importación de archivos.*/
 
-- 3.1 INSERCION DE DATOS MEDIANTE SCRIPT SQL PARA LA TABLA PROVEEDOR
INSERT INTO proveedor
(NOMBRE, TELEFONO, UBICACION)
values
('Electronic Nine', '3155764103', 'Usa'),
('M&G', '3122698493', 'China'),
('Sanders', '3171192610', 'Usa'),
('Ubi', '3118171474', 'China'),
('Sams', '3209028008', 'Usa');

-- 3.2 INSERCION DE DATOS MEDIANTE SCRIPT SQL PARA LA TABLA LOCALIZACION
insert into localizacion
(Pais, Ciudad, Continente)
values
('Usa',	'New York',	'Norte America'),
('Usa',	'Los Angeles', 'Norte America'),
('Usa',	'San Francisco', 'Norte America'),
('Usa',	'Miami', 'Norte America'),
('Usa',	'Chicago',	'Norte America'),
('Argentina', 'Buenos Aires', 'Sur America'),
('Colombia', 'Bogota',	'Sur America'),
('Francia',	'Paris', 'Europa'),
('Alemania', 'Berlin', 'Europa'),
('Mexico',	'Monterrey', 'America Central'),
('Australia', 'Sydney',	'Australia'),
('Japon', 'Tokio',	'Asia');

-- 3.3 INSERCION DE DATOS MEDIANTE SCRIPT SQL PARA LA TABLA TIPO DE PAGO
INSERT INTO tipo_pago
(Tipo_Pago)
values
('Efectivo'),('Tarjeta de Credito'),
('NetBanking'), ('Prestamo');

-- 3.4 INSERCION DE DATOS MEDIANTE SCRIPT SQL PARA LA TABLA CATALOGO_PROVEEDOR
insert into catalogo_proveedor
(MARCA, CANTIDAD, ID_PRODUCTO, ID_PROVEEDOR)
values
('Asus', '50', '1', '4'),
('Apple', '50', '2', '2'),
('Sony', '50', '3', '3'),
('Xbox', '50', '4', '3'),
('Nintendo', '50', '5', '3'),
('Samsung', '35', '6', '1'),
('Samsung', '40', '7', '1'),
('Apple', '15', '8', '2'),
('Samsung', '23', '9', '1'),
('Xiaomi', '12', '10', '4'),
('Rimax', '8', '11', '5'),
('Rimax', '8', '12', '5');

-- 3.5 INSERCION DE DATOS MEDIANTE SCRIPT SQL PARA LA TABLA SUCURSAL
insert into sucursal
(NOMBRE, ID_LOCALIZACION, TELEFONO, ENCARGADO)
values
('Hamilton', '1', '3277032299', 'Danzel Mender'),
('Spencer', '2', '3423903895', 'Borgir Ham'),
('Truli', '3', '3161933489', 'Gabriel Diaz'),
('Guandia', '4', '3274069139', 'Jackson Gi'),
('Freeland', '5', '3167493541', 'Figueredo Sans'),
('Expector', '6', '3548890569', 'Fuquene Alberto'),
('Terra', '7', '3334423553', 'Rudy Giovany'),
('Hanover', '8', '3431386574', 'Tairon Estiven'),
('Dimmu', '9', '3200469808', 'Manuela Alejo'),
('Moth', '10', '3399406802', 'Juana Maria'),
('Tata', '11', '3213456578', 'Yesid Alvarez'),
('Feri', '12', '3176543421', 'Pedro Arias');
```
**4. SCRIPT DE LA CREACION DE VISTAS**
```sql
-- 4.1 PRIMERO DESARROLLAMOS UN SCHEMA NUEVO DONDE VAN LAS VIEWS CREADAS, ESTO LO HACEMOS POR BUENAS PRACTICAS. 
DROP schema IF exists Entrega2;
create schema if not exists Entrega2;

-- 4.2 EN ESTA VIEW, QUEREMOS VER CUAL ES LA SUCURSAL CON MAYOR CANTIDAD DE VENTAS
CREATE OR replace view
Entrega2.VW_VIEW1_CANTIDAD_VENTAS AS
(
SELECT sucursal.ID_SUCURSAL, sucursal.NOMBRE, sucursal.TELEFONO,
count(ventas.ID_SUCURSAL) AS Numero_Ventas
FROM sucursal
inner join ventas
on sucursal.ID_SUCURSAL = ventas.ID_SUCURSAL
group by sucursal.ID_SUCURSAL, sucursal.NOMBRE, sucursal.TELEFONO
order by Numero_Ventas desc
limit 1
);
select * from entrega2.vw_view1_cantidad_ventas;

-- 4.3 EN ESTA VIEW, QUEREMOS VER LA SUMA TOTAL DE VENTAS AGRUPADAS POR LOS DIAS DE LA SEMANA
CREATE OR replace view
Entrega2.VW_VIEW2_SUMA_VENTAS AS
(
SELECT dayname(FECHA_VENTAS) AS Dia,
sum(total) as Suma_Total
from ventas
group by Dia
order by Suma_Total desc
);

-- 4.4  EN ESTA VIEW, QUEREMOS HACER UN CONTEO, POR LA AGRUPACION DE TIPO DE PAGO
CREATE OR replace view
Entrega2.VW_VIEW3_AGRUPACION_PAGO AS
(
select TIPO_PAGO,
count(ventas.ID_TIPO_PAGO) as Numero_Ventas
from tipo_pago
inner join ventas 
on tipo_pago.ID_TIPO_PAGO = ventas.ID_TIPO_PAGO
group by TIPO_PAGO
);

-- 4.5  EN ESTA VIEW, QUEREMOS VER CUAL VENDEDOR HIZO LA VENTA DE MAYOR VALOR
CREATE OR replace view
Entrega2.VW_VIEW4_VENDEDOR_MAYOR AS
(
select vendedores.ID_VENDEDOR, NOMBRE, APELLIDO
FROM vendedores
inner join ventas
on vendedores.ID_VENDEDOR = ventas.ID_VENDEDOR
where total = (select max(total) from ventas)
);

-- 4.6 EN ESTA VIEW, QUEREMOS HACER UN CONTEO DE LOS CLIENTES POR GENERO MAYORES DE 35 
CREATE OR replace view
Entrega2.VW_VIEW5_CONTEO_CLIENTES_35 AS
(
SELECT genero,
count(*) Cantidad_Clientes
from clientes
where edad > 35
group by genero
);

-- 4.7 EN ESTA VIEW, QUEREMOS VER LA EDAD PROMEDIO DE LOS CLIENTES
CREATE OR replace view
Entrega2.VW_VIEW6_EDAD_PROMEDIO AS
(
SELECT  round(avg(edad)) as Edad_Promedio_Clientes
from clientes
);

-- 4.8 EN ESTA VIEW, QUEREMOS VER EL DIA CON LA MAYOR CANTIDAD DE ITEMS VENDIDOS
CREATE OR replace view
Entrega2.VW_VIEW7_DIA_MAYOR AS
(
SELECT FECHA_VENTAS, SUM(dv.CANTIDAD) AS Total_Productos_Vendidos
FROM ventas v
JOIN detalle_venta dv ON v.Id_Ventas = dv.Id_Ventas
GROUP BY FECHA_VENTAS
ORDER BY Total_Productos_Vendidos DESC
LIMIT 1
);

-- 4.9 EN ESTA VIEW, QUEREMOS VER TODOS LOS PRODUCTOS QUE TIENEN UN PRECIO MAYOR O IGUAL AL PRODUCTO MAS BARATO DE LA MARCA SAMSUNG
CREATE OR replace view
Entrega2.VW_VIEW8_SAMSUNG_BARATO AS
(
select ID_PRODUCTO, nombre, precio
from producto
where precio >= (select min(precio) from producto
where nombre like '%samsung%')
);

-- 4.10 EN ESTA VIEW, QUEREMOS VER EL PRODUCTO CON  MAS UNIDADES VENDIDAS 
CREATE OR replace view
Entrega2.VW_VIEW9_PRODUCTO_MAS_VENDIDO AS
(
select producto.ID_PRODUCTO, nombre, precio,
count(detalle_venta.ID_PRODUCTO) as Numero_Ventas,
sum(CANTIDAD) as Cantidad_Unidades_Vendidas
from producto
inner join detalle_venta
on producto.ID_PRODUCTO = detalle_venta.ID_PRODUCTO
group by producto.ID_PRODUCTO, nombre, precio
order by Cantidad_Unidades_Vendidas desc
limit 2
);

-- 4.11 EN ESTA VIEW, QUEREMOS VER EL PRODUCTO CON  MENOS UNIDADES VENDIDAS 
CREATE OR replace view
Entrega2.VW_VIEW10_PRODUCTO_MENOS_VENDIDO AS
(
select producto.ID_PRODUCTO, nombre, precio,
count(detalle_venta.ID_PRODUCTO) as Numero_Ventas,
sum(CANTIDAD) as Cantidad_Unidades_Vendidas
from producto
inner join detalle_venta
on producto.ID_PRODUCTO = detalle_venta.ID_PRODUCTO
group by producto.ID_PRODUCTO, nombre, precio
order by Cantidad_Unidades_Vendidas 
limit 1
);
```
**5. SCRIPT DE LA CREACION DE FUNCIONES**
```sql
-- PRUEBA DE ESCRITORIO
SELECT Producto
FROM (
SELECT producto.NOMBRE as Producto,
sum(detalle_venta.CANTIDAD) as cantidad
FROM producto
INNER JOIN catalogo_proveedor
ON producto.ID_PRODUCTO = catalogo_proveedor.ID_PRODUCTO
INNER JOIN proveedor
ON catalogo_proveedor.ID_PROVEEDOR = proveedor.ID_PROVEEDOR
INNER JOIN detalle_venta
ON catalogo_proveedor.ID_PRODUCTO = detalle_venta.ID_PRODUCTO
where proveedor.NOMBRE = 'Ubi'
group by producto.NOMBRE, proveedor.NOMBRE
order by cantidad desc
limit 1
) as Subconsulta;

-- 5.1 FUNCION PARA SABER EL PRODUCTO CON MAS CANTIDADES VENDIDAS DE CUALQUIER PROVEEDOR
DELIMITER $$ 
CREATE FUNCTION FN_PRODUCTO_MAS_VENDIDO_PROVEEDOR (P_PROVEEDOR varchar (100))
                           
RETURNS VARCHAR(100)
DETERMINISTIC 
BEGIN 
declare V_RESULTADO varchar (100) ;

SELECT Producto
FROM (
SELECT producto.NOMBRE as Producto,
sum(detalle_venta.CANTIDAD) as cantidad
FROM producto
INNER JOIN catalogo_proveedor
ON producto.ID_PRODUCTO = catalogo_proveedor.ID_PRODUCTO
INNER JOIN proveedor
ON catalogo_proveedor.ID_PROVEEDOR = proveedor.ID_PROVEEDOR
INNER JOIN detalle_venta
ON catalogo_proveedor.ID_PRODUCTO = detalle_venta.ID_PRODUCTO
where proveedor.NOMBRE = P_PROVEEDOR
group by producto.NOMBRE, proveedor.NOMBRE
order by cantidad desc
limit 1
) as Subconsulta 
into V_RESULTADO;

RETURN V_RESULTADO;
END $$
DELIMITER ;
-- Invocar a la funcion
select FN_PRODUCTO_MAS_VENDIDO_PROVEEDOR ('Sanders');

-- PRUEBA DE ESCRITORIO
SELECT  sum(TOTAL) AS Total_Ventas
FROM localizacion
INNER JOIN sucursal
ON localizacion.ID_LOCALIZACION = sucursal.ID_LOCALIZACION
INNER JOIN VENTAS
ON sucursal.ID_SUCURSAL = ventas.ID_SUCURSAL
WHERE PAIS = 'USA'

-- 5.2 FUNCION PARA SABER EL TOTAL DE VENTAS DE CADA PAIS QUE ELIJAMOS. 
DELIMITER $$ 
CREATE FUNCTION FN_PAIS_VENTAS_TOTALES(P_PAIS VARCHAR(100))
				
RETURNS VARCHAR(100)
DETERMINISTIC 
BEGIN 
DECLARE V_TOTAL VARCHAR(250) ;

SELECT  sum(TOTAL) AS Total_Ventas
FROM localizacion
INNER JOIN sucursal
ON localizacion.ID_LOCALIZACION = sucursal.ID_LOCALIZACION
INNER JOIN VENTAS
ON sucursal.ID_SUCURSAL = ventas.ID_SUCURSAL
WHERE PAIS = P_PAIS
INTO V_TOTAL;
RETURN V_TOTAL;
END$$ 
DELIMITER ;

-- Invocar a la funcion
select FN_PAIS_VENTAS_TOTALES ('USA');

-- PRUEBA DE ESCRITORIO
SELECT  
precio * sum(cantidad) as Ventas_Por_Producto
From detalle_venta
INNER JOIN producto
ON detalle_venta.ID_PRODUCTO = producto.ID_PRODUCTO
WHERE producto.NOMBRE = 'portatil asus' 
group by precio
having precio * sum(cantidad) > 1380;

-- 5.3 FUNCION PARA CONOCER CUANTO DINERO GENERO CADA PRODUCTO EN LAS VENTAS
DELIMITER $$ 
CREATE FUNCTION FN_VENTAS_POR_PRODUCTO(P_PRODUCTO VARCHAR(100), 
				                       P_VALOR int ) 
RETURNS int
DETERMINISTIC 
BEGIN 
DECLARE V_VALOR int ;
 
SELECT  
precio * sum(cantidad) as Ventas_Por_Producto
From detalle_venta
INNER JOIN producto
ON detalle_venta.ID_PRODUCTO = producto.ID_PRODUCTO
WHERE producto.NOMBRE =  P_PRODUCTO
group by precio
having precio * sum(cantidad) > P_VALOR
INTO V_VALOR;
RETURN V_VALOR;
END$$ 
DELIMITER ;
-- INVOCAR FUNCION
SELECT FN_VENTAS_POR_PRODUCTO ('portatil asus', 1380);

-- PRUEBA DE ESCRITORIO
SELECT  round(avg(total)) AS Pomedio_valor_Ventas
FROM localizacion
INNER JOIN sucursal
ON localizacion.ID_LOCALIZACION = sucursal.ID_LOCALIZACION
INNER JOIN VENTAS
ON sucursal.ID_SUCURSAL = ventas.ID_SUCURSAL
WHERE CONTINENTE = 'sur america';

-- 5.4 FUNCION PARA CONOCER EL PROMEDIO DEL VALOR DE LAS VENTAS TOTALES POR CONTINENTE
 DELIMITER $$ 
CREATE FUNCTION FN_PROMEDIO_VENTAS_CONTINENTE(P_CONTINENTE VARCHAR(50))
				
RETURNS int
DETERMINISTIC 
BEGIN 
DECLARE V_PROMEDIO int ;

SELECT  round(avg(total)) AS Pomedio_valor_Ventas
FROM localizacion
INNER JOIN sucursal
ON localizacion.ID_LOCALIZACION = sucursal.ID_LOCALIZACION
INNER JOIN VENTAS
ON sucursal.ID_SUCURSAL = ventas.ID_SUCURSAL
WHERE CONTINENTE = P_CONTINENTE
INTO V_PROMEDIO;
RETURN V_PROMEDIO;
END$$ 
DELIMITER ;
-- INVOCAR FUNCION
SELECT FN_PROMEDIO_VENTAS_CONTINENTE ('NORTE AMERICA');
```
**6. SCRIPT DE LA CREACION DE PROCEDIMIENTOS**
```sql
-- 6.1 PROCEDIMIENTO: Aca podemos reemplazar la columna que queramos de la tabla vendedores, y ordenarla con desc o asc. 
DELIMITER $$
CREATE PROCEDURE SP_VENDEDORES_ORDERBY (
    IN P_columna VARCHAR(50),
    IN P_DESC_ASC enum ('DESC', 'ASC'))
BEGIN
    SET @VENDE = CONCAT('SELECT ID_VENDEDOR, NOMBRE, APELLIDO, EDAD, TELEFONO, Fecha_Inicio FROM vendedores ORDER BY ', P_columna, ' ', P_DESC_ASC, ' LIMIT 1'  ) ;
    PREPARE statement FROM @VENDE ;
    EXECUTE statement ;
    DEALLOCATE PREPARE statement ;
END $$
DELIMITER ;
-- INVOCAR PROCEDIMIENTO
CALL SP_VENDEDORES_ORDERBY ('EDAD', 'asc');

-- PRUEBA DE ESCRITORIO
INSERT INTO LOCALIZACION 
(ID_LOCALIZACION, PAIS, CIUDAD, CONTINENTE)
values
(13, 'China', 'Bejing', 'Asia');
select * from localizacion;
-- 6.2 PROCEDIMIENTO CON INSERT: Aca basicamente insertamos datos a la tabla localizacion
DELIMITER $$
CREATE PROCEDURE SP_INSERCION_LOCALIZACION( IN P_ID int ,
                                                      IN P_PAIS varchar (30) ,                                             
                                                      in P_CIUDAD VARCHAR(30) ,
                                                      in P_CONTINENTE varchar (30) , 
                                                      OUT P_ID2 INT ,
                                                      out P_PAIS2 varchar (30) ,                                             
                                                      out P_CIUDAD2 VARCHAR(30) ,
                                                      out P_CONTINENTE2 varchar (30) ) 
BEGIN
INSERT INTO LOCALIZACION 
(ID_LOCALIZACION, PAIS, CIUDAD, CONTINENTE)
values
(P_ID, P_PAIS, P_CIUDAD, P_CONTINENTE) ;

END $$
DELIMITER ; 
-- Invocar procedimeinto
call SP_INSERCION_LOCALIZACION (14, 'Italia', 'Roma', 'Europa', @P_ID2, @P_PAIS2, @P_CIUDAD2, @P_CONTINENTE2 );
select @P_ID2 as ID, @P_PAIS2 AS PAIS, @P_CIUDAD2 AS CIUDAD, @P_CONTINENTE2 AS CONTINENTE;
SELECT * FROM localizacion;

-- PRUEBA DE ESCRITORIO: TRAER LA MARCA, NOMBRE DEL PROVEEDOR Y LA CANTIDAD DE PRODUCTOS, DE LA MARCA'SAMSUNG' Y QUE LA CANTIDAD DE PRODUCTOS SEA MAYOR A 1O
-- traer solo el producto con mayor cantidad de stock
select marca, nombre, cantidad
from catalogo_proveedor
inner join proveedor
on catalogo_proveedor.ID_PROVEEDOR = proveedor.ID_PROVEEDOR
where marca = 'Samsung' and cantidad > 10
order by CANTIDAD desc
limit 1;
-- 6.3 PROCEDIMIENTO
DELIMITER $$
CREATE PROCEDURE SP_MARCA_STOCK( IN P_MARCA VARCHAR(50) ,
                                                      IN P_CANTIDAD INT ,    -- entrada                                              
                                                      OUT P_MARCA2 VARCHAR(30) ,
                                                      OUT P_NOMBRE_PROVEEDOR varchar (30) , -- salida
                                                      OUT P_CANTIDAD2 INT ) 
BEGIN

select marca, nombre, cantidad
INTO
P_MARCA2, P_NOMBRE_PROVEEDOR, P_CANTIDAD2
from catalogo_proveedor
inner join proveedor
on catalogo_proveedor.ID_PROVEEDOR = proveedor.ID_PROVEEDOR
where marca =  P_MARCA and cantidad > P_CANTIDAD
order by CANTIDAD desc
limit 1;

END $$
DELIMITER ; 
-- INVOCACION DEL PROCEDIMIENTO
CALL SP_MARCA_STOCK ('samsung', 39, @P_MARCA2, @P_NOMBRE_PROVEEDOR, @P_CANTIDAD2 );
SELECT @P_MARCA2 AS MARCA, @P_NOMBRE_PROVEEDOR AS NOMBRE_PROVEEDOR, @P_CANTIDAD2 AS CANTIDAD;
```
**7. SCRIPT DE LA CREACION DE TRIGGERS**
```sql
-- 7.1 TRIGGER PARA LA TABLA CLIENTE CON LA ACCION BEFORE, CON UNA OPERACION DE INSERT
-- CREAMOS LA TABLA DE SEGUIMIENTO O BITACORA
CREATE TABLE Clientes_Seguimiento (
ID_CLIENTE int PRIMARY KEY,
DOCUMENTO int,
NOMBRE varchar (50),
APELLIDO varchar (50),
EDAD INT,
GENERO enum ('M','F'),
TIPO_CLIENTE enum ('Normal', 'Miembro'),
TELEFONO VARCHAR (20),
CORREO VARCHAR (50),
FECHA_TRIGGER datetime,
USUARIO VARCHAR (50)
);
-- DESARROLLAMOS EL TRIGGER PARA LA TABLA CLIENTES
CREATE TRIGGER tr_insert_before_clientes
before insert on clientes
for each row
insert into Clientes_Seguimiento (ID_CLIENTE, DOCUMENTO, NOMBRE, APELLIDO, EDAD, GENERO, TIPO_CLIENTE, TELEFONO, CORREO, FECHA_TRIGGER, USUARIO)
values
(new.ID_CLIENTE, new.DOCUMENTO, new.NOMBRE, new.APELLIDO, new.EDAD, new.GENERO, new.TIPO_CLIENTE, new.TELEFONO, new.CORREO,  now(), user() );

-- hacemos el insert
insert into clientes
(DOCUMENTO, NOMBRE, APELLIDO, EDAD, GENERO, TIPO_CLIENTE, TELEFONO, CORREO) 
VALUES
(03044050, 'Maxii', 'blue', 88, 'M', 'Normal', '3002410998', 'maxio@hotmail.com');

select * from clientes_Seguimiento;
select * from clientes;

-- 7.2 TRIGGER PARA LA TABLA VENDEDOR CON LA ACCION AFTER, CON UNA OPERACION DE UPDATE
-- CREAMOS LA TABLA DE SEGUIMIENTO O BITACORA
CREATE TABLE vendedores_Seguimiento (
ID_VENDEDOR int primary key,
NOMBRE varchar (50),
APELLIDO varchar (50),
EDAD int,
TELEFONO varchar (20),
FECHA_TRIGGER datetime,
USUARIO VARCHAR (50)
);
-- DESARROLLAMOS EL TRIGGER PARA LA TABLA VENDEDORES
CREATE TRIGGER tr_update_after_vendedores
after update on vendedores
for each row 
insert into vendedores_Seguimiento (ID_VENDEDOR, NOMBRE, APELLIDO, EDAD, TELEFONO, FECHA_TRIGGER, USUARIO)
values
(new.ID_VENDEDOR, new.NOMBRE, new.APELLIDO, new.EDAD, new.TELEFONO, now(), user());

-- hacemos el update
update vendedores
set telefono = '4002340987'
where ID_VENDEDOR = 1;

select * from vendedores_Seguimiento;
select * from vendedores;
```
**8. IMPLEMENTACION DE SENTENCIAS**
```sql
CREACION DE DOS USUARIOS 
8.1 ESTE USUARIO DEBE TENER PERMISOS DE SOLO LECTURA SOBRE TODAS LAS TABLAS
*/
use mysql;
select * from user;
CREATE user IF not exists AlexanderG@localhost identified by 'data'; -- creamos el usuario AlexanderG en nuestro localhost
grant select on proyecto_tienda_electronicos.* to AlexanderG@localhost; -- A este usuario le otorgamos el permiso de solo lectura para el schema proyecto_tienda_electronicos en todas las tablas.
show grants FOR AlexanderG@localhost;

-- 8.2 ESTE USUARIO DEBE TENER PERMISOS DE SOLO LECTURA, INSERCION Y UPDATE SOBRE TODAS LAS TABLAS
CREATE user IF not exists DATA_ANALITYCS@localhost identified by '12345'; -- creamos el usuario DATA_ANALITYCS en nuestro localhost
grant select, insert, update on proyecto_tienda_electronicos.* to DATA_ANALITYCS@localhost; -- A este usuario le otorgamos el permiso de lectura, insercion y update para el schema proyecto_tienda_electronicos en todas las tablas.
show grants FOR DATA_ANALITYCS@localhost;
```
**9. SENTENCIAS DEL SUBLENGUAJE TCL**
```sql
9.1 En la tabla vendedores, se elimino el registro con id 15, y se inicio con una una transacción.  
Deja en una línea siguiente, comentado la sentencia Rollback, y en una línea posterior, la sentencia Commit. Si eliminas registros importantes, deja comentadas las sentencias para re-insertarlos.

SELECT @@autocommit; -- vemos si esta activada o no el tcl
SET @@autocommit = 0; -- apagada las transacciones
SET  @@FOREIGN_KEY_CHECKS = 0; -- desactivo llaves foraneas, en caso que necesite borrar o manipular la tabla madre
SET SQL_SAFE_UPDATES = 0;

select * from vendedores; -- Para este caso seleccionamos la tabla vendedores
START TRANSACTION ; -- damos inicio a la transaccion
delete from vendedores 
where ID_VENDEDOR = 15; -- volver a insertar: 15 - Carla - Lopez - 51 - 3131036513 - 7 - 2022-07-25
-- rollback -- Deshace la transaccion
-- commit -- confirma la transaccion

/* 9.2 
En la tabla proveedor, se inserto ocho nuevos registros iniciando también una transacción. 
Agrega un savepoint a posteriori de la inserción del registro #4 y otro savepoint a posteriori del registro #8
Agrega en una línea comentada la sentencia de eliminación del savepoint de los primeros 4 registros insertados.
*/
SELECT @@autocommit; -- vemos si esta activada o no el tcl
SET @@autocommit = 0; -- apagada las transacciones
SET  @@FOREIGN_KEY_CHECKS = 0; -- desactivo llaves foraneas, en caso que necesite borrar o manipular la tabla madre
SET SQL_SAFE_UPDATES = 0;

select * from proveedor; -- Para este caso seleccionamos la tabla vendedores
START TRANSACTION ; -- damos inicio a la transaccion
insert into proveedor (ID_PROVEEDOR,NOMBRE,TELEFONO,UBICACION) values (6, 'Exit', '2323345', 'India');
insert into proveedor (ID_PROVEEDOR,NOMBRE,TELEFONO,UBICACION) values (7, 'Exito', '12653233400', 'India');
insert into proveedor (ID_PROVEEDOR,NOMBRE,TELEFONO,UBICACION) values (8, 'Tare', '5552334532', 'India');
insert into proveedor (ID_PROVEEDOR,NOMBRE,TELEFONO,UBICACION) values (9, 'Fuller', '54672334', 'Usa');
savepoint sp1; -- Primer sp
insert into proveedor (ID_PROVEEDOR,NOMBRE,TELEFONO,UBICACION) values (10, 'Insat', '0009845', 'China');
insert into proveedor (ID_PROVEEDOR,NOMBRE,TELEFONO,UBICACION) values (11, 'Ail', '232334532', 'Bangladesh');
insert into proveedor (ID_PROVEEDOR,NOMBRE,TELEFONO,UBICACION) values (12, 'Tere', '1113345', 'India');
insert into proveedor (ID_PROVEEDOR,NOMBRE,TELEFONO,UBICACION) values (13, 'Qred', '999334509', 'Taiwan');
savepoint sp2; -- Segundo sp
-- release savepoint sp1; -- Eliminacion del savepoint sp1
```





