# 1. Entidades

## Vivero

Representa cada uno de los establecimientos de Tajinaste S.A. donde se almacenan y venden productos.

**Atributos:**

* **id_vivero:** clave principal del vivero.
  **Ejemplo:** `V001`.
  
* **Nombre:** indica el nombre del vivero.
  **Ejemplos:** `"Vivero de Santa Cruz"`.
  
* **Georreferenciación:** indica la ubicación exacta del vivero y está formada por latitud y longitud.
  **Ejemplo:** latitud `28.4636`, longitud `-16.2518`.

## Zona

Representa cada una de las zonas en las que se divide un vivero, como pueden ser la zona exterior, el almacén, etc. Cada zona pertenece a un único vivero y dispone de su propia georreferenciación.

**Atributos:**

* **id_zona:** clave principal de la zona.
  **Ejemplo:** `Z01`.

* **Nombre:** indica el nombre de la zona.
  **Ejemplos:** `"Almacén"`, `"Exterior"`, `"Invernadero"`.

* **Georreferenciación:** indica la ubicación de la zona y está formada por latitud y longitud.
  **Ejemplo:** latitud `28.4638`, longitud `-16.2520`.

## Producto

Representa cada uno de los productos que comercializa Tajinaste S.A., incluyendo plantas, productos de jardinería y artículos de decoración. Un producto puede encontrarse disponible en diferentes zonas de los viveros.

**Atributos:**

* **id_producto:** código único que permite identificar cada producto comercializado por la empresa.
  **Ejemplo:** `P001`.

* **Nombre del producto:** nombre utilizado para identificar el producto.
  **Ejemplos:** `"Rosal rojo"`, `"Maceta de cerámica"`, `"Tierra para plantas"`.

* **Precio:** coste de venta del producto.
  **Ejemplos:** `"12,50€"`.

* **Tipo:** categoría a la que pertenece el producto.
  **Ejemplos:** `"Planta"`, `"Decoración"`.

## Empleado

Representa a cada una de las personas que trabajan para Tajinaste S.A. Los empleados pueden ser destinados a diferentes viveros dependiendo de la época del año. Además, se registra el histórico de los puestos y zonas en los que han trabajado.

**Atributos:**

* **NIF:** clave primaria del empleado.
  **Ejemplo:** `12345678A`.

* **Nombre:** nombre del empleado.
  **Ejemplo:** `"Juan García López"`.

* **Teléfono:** número de contacto del empleado.
  **Ejemplo:** `"600123456"`.

* **Dirección:** domicilio del empleado.
  **Ejemplos:** `"Calle alguna, 12, Santa Cruz"`.

## Cliente

Representa a las personas que realizan compras en Tajinaste S.A. Los clientes pueden pertenecer al programa de fidelización Tajinaste Plus. En el caso de pertenecer al programa, se realiza un seguimiento de los pedidos realizados desde su ingreso.

**Atributos:**

* **NIF:** identificador del cliente.
  **Ejemplo:** `87654321B`.

* **Nombre:** nombre del cliente.
  **Ejemplo:** `"María Rodríguez Pérez"`.

* **Dirección:** domicilio del cliente.
  **Ejemplo:** `"Calle alguna, 20, La Laguna"`.

* **Email:** correo electrónico de contacto.
  **Ejemplo:** `"m.rodrig@gmail.com"`.

* **fecha_plus:** fecha en la que el cliente se dio de alta en el programa de fidelización Tajinaste Plus.
  **Ejemplo:** `"15/04/2026"`.

## Bonificación

Representa las recompensas asignadas a los clientes del programa Tajinaste Plus según el volumen de sus compras. Es una entidad débil que depende de Cliente.

**Atributos:**

* **Mes:** indica el mes al que corresponde el cálculo de las compras.
  **Ejemplo:** `10/2026`.

* **Volumen:** total del importe de las compras acumuladas por el cliente durante ese mes.
  **Ejemplo:** `"350,00 €"`.

* **Bonificación:** cantidad económica asignado como premio al cliente.
  **Ejemplo:** `"15,00 €"`.

## Pedido

Representa cada una de las compras realizadas por los clientes de Tajinaste S.A. Cada pedido tiene un único empleado responsable de su gestión. En el caso de los clientes pertenecientes a Tajinaste Plus, sus pedidos se utilizan para controlar el volumen de compras y determinar las bonificaciones correspondientes.

**Atributos:**

* **id_pedido:** código único que permite identificar cada pedido.
  **Ejemplo:** `PED001`.

* **Fecha:** fecha en la que se realizó el pedido.
  **Ejemplo:** `07/10/2026`.

* **Importe:** cantidad económica correspondiente al pedido.
  **Ejemplo:** `125,50 €`.

* **Responsable:** referencia al empleado que se encarga de la gestión de este pedido en concreto.
  **Ejemplo:** `12345678A`.

# 2. Relaciones

## Vivero — tiene — Zona

Cardinalidad 1:N

## Cliente — realiza — Pedido

Cardinalidad 1:N

## Cliente - recibe - Bonificación

Un cliente puede recibir varias bonificaciones a lo largo de los meses, pero cada registro de bonificación mensual pertenece a un único cliente.

Cardinalidad 1:N 

## Producto — está asignado a — Zona

* **Cantidad disponible:** cantidad de unidades de un determinado producto que están disponibles en una zona concreta. Es un atributo de la relación porque la cantidad puede variar dependiendo de la zona.
  **Ejemplo:** el producto `"Rosal rojo"` tiene `150` unidades disponibles en la zona `"Almacén"` y `75` unidades en la zona `"Exterior"`.
  
Cardinalidad N:M

## Empleado — destinado en — Vivero

* **fecha0:** fecha en la que el empleado comienza a estar destinado en un determinado vivero.
  **Ejemplo:** `01/01/2026`.
  
* **fecha1:** fecha en la que el empleado finaliza a estar destinado en un determinado vivero.
  **Ejemplo:** `09/01/2026`.

* **Puesto:** puesto que ocupa en el vivero.
  **Ejemplo:** `30/06/2026`.

Cardinalidad N:M

## Empleado — trabaja en — Zona

* **tarea:** actividad que realiza el empleado dentro de la zona.
  **Ejemplos:** `"Reposición"`, `"Mantenimiento"`, `"Atención al cliente"`.

* **fecha0:** fecha desde la que el empleado realiza la tarea en esa zona.
  **Ejemplo:** `01/03/2026`.

* **fecha1:** fecha hasta la que el empleado realiza la tarea en esa zona.
  **Ejemplo:** `31/05/2026`.

Cardinalidad N:M

## Empleado — gestiona — Pedido

Esta relación no necesita necesariamente atributos propios, ya que el empleado responsable puede identificarse mediante la relación.

Cada pedido tendrá un único empleado responsable de su gestión.

**Ejemplo:** el pedido `PED001` está gestionado por el empleado con NIF `12345678A`.

Cardinalidad 1:N

# 3. Atributo compuesto

## Georreferenciación

La **georreferenciación** es un atributo compuesto formado por:

* **Latitud:** coordenada geográfica norte-sur.
  **Ejemplo:** `28.4636`.

* **Longitud:** coordenada geográfica este-oeste.
  **Ejemplo:** `-16.2518`.

Por tanto:

**Georreferenciación = (latitud, longitud)**

Este atributo aparece tanto en **Vivero** como en **Zona**.

# 4. Restricciones semánticas

* **Empleado - Vivero:** Un empleado solo puede tener un destino activo simultáneamente. Por lo tanto, se establece la restricción de que las fechas `fecha0` y `fecha1` de un mismo empleado no pueden solaparse en el tiempo.
* **Entidades Débiles:** Entidades cuya existencia depende de una entidad fuerte:
  * **Zona:** dependiente de **Vivero**. Una zona no puede existir si no está asociada a un vivero concreto.
  * **Bonificación:** dependiente de **Cliente**. No existe de manera aislada, siempre debe pertenecer al cliente que generó ese beneficio.

* **Programa Tajinaste Plus:** La generación de bonificaciones están restringidas a aquellos clientes que pertenecen al programa Tajinaste Plus. Por lo que solo los clientes que tengan un valor asignado en el atributo `fecha_plus` podrán participar en la relación **recibe** asociada a la entidad **Bonificación**

