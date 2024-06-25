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

```sql
-- 1. CREAMOS LA BASE DE DATOS
DROP schema if exists Proyecto_Tienda_Electronicos;
CREATE schema if not exists Proyecto_Tienda_Electronicos;
USE Proyecto_Tienda_Electronicos;
