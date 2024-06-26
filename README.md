# Portfolio-SQL01

**NOTA:**
Basicamente este proyecto lo iniciamos desde cero, ya que la idea es implementar varias funciones de SQL como views, funciones, procedimientos, triggers, DCL y TCL. Para asi poder ver todo lo que nos puede ofrecer SQL, en cualquier proyecto de datos que tengamos

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

