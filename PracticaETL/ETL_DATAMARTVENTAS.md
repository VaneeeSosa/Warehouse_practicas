# ETL con data warehouse

#### Creacion del data warehouse (modelo estrella)

___
#### Crear base de datos

```sql
create database DATAMARTVENTAS
USE DATAMARTVENTAS
```
___
#### Se uso vss2022 para poder hacer las tablas destinovy ademas cada ETL se hicieron en paquetes distintos para evitar errores 
![Diagrama](imgETL/etl1.png)
___

##### Se agrego un Data Flow Task y configuramos las conexiones de origen (Northwind) y de destino (DATAMARTVENTAS)
![Diagrama](imgETL/image.png)
##### Entramos al Data Flow
___
##### Agregamos dos ole db source, donde en uno tenemos nuestra conexion a Northwind y el otro es la conexion de nuestra tabla destino (DATAMARTVENTAS)
![Diagrama](imgETL/etl2.png)

___
##### Configuraremos el source de esta manera para Northwind 
![Diagrama](imgETL/p2.png)
___
##### Configuraremos el source de esta manera para DATAMARTVENTAS
![Diagrama](imgETL/p3.png)
##### Ordenamos ambas tablas por medio de un sort, ambos sort deben de coincidir en un campo para que puedan ser ordenados.
Este sort es para Northwind
![Diagrama](imgETL/p4.png)
Este sort es para DATAMARTVENTAS
![Diagrama](imgETL/p5.png)
___
##### Agreamos un merge join, siempre espicificando que sera left y agreamos las columnas que ocupamos
![Diagrama](imgETL/p6.png)

___
##### Tomamos un conditional split para agregar una condicion, en caso de ser nulo ese campo le pondra NULL
![Diagrama](imgETL/p7.png)
___
##### Tambien agregamos un column delivered para concatenar el firstName y lastName (por parte de Northwind)
![Diagrama](imgETL/p8.png)

___
##### Por ultimo nuestro ole db Destination donde se encontrara nuestra tabla DimEmpleado
![Diagrama](imgETL/p9.png)
##### y asi haremos con las demas tablas

___
##### Tabla DimProducto
![Diagrama](imgETL/etl3.png)

___
##### Tabla DimCliente
![Diagrama](imgETL/etl4.png)

___
##### Tabla DimProveedor
![Diagrama](imgETL/etl5.png)


___
##### Tabla DimTiempo
para esta tabla se ocupo una consulta directa para poder separar correctamente la fecha
```sql
SELECT 
    o.OrderDate, 
    YEAR(o.OrderDate) AS Anio, 
    DATEPART(quarter, o.OrderDate) AS Trimestre, 
    DATEPART(MONTH, o.OrderDate) AS Mes,
    DATEPART(week, o.OrderDate) AS Semana
FROM 
    Northwind.dbo.Orders o
```
##### esta consulta se pega en el source de Northwind y se siguieron los pasos anteriores.
![Diagrama](imgETL/etl6.png)
![Diagrama](imgETL/etl6_1.png)
![Diagrama](imgETL/etl6_2.png)
![Diagrama](imgETL/etl6_3.png)
![Diagrama](imgETL/etl6_4.png)
![Diagrama](imgETL/etl6_5.png)
![Diagrama](imgETL/etl6_6.png)
____
##### Tabla factVentas
Para esta tabla fue necesario hacer merge join a todas las tablas de DATAMARTVENTAS y se unieron con las tablas de Northwind para igualar las tablas y usar las ID correspondientes de Northwind y de DATAMARTVENTAS
![Diagrama](imgETL/fact1.png)
![Diagrama](imgETL/fact2.png)
![Diagrama](imgETL/fact3.png)

_____
##### Data flow final
![Diagrama](imgETL/FinalETL.png)

_____
##### Diagrama del data warehouse
![Diagrama](imgETL/etlDiagrama.png)