# UNIDAD-3-BASE-DE-DATOS-EJERCICIO-10
El diagrama muestra un **Modelo Relacional (MR)** orientado a la gestión de una flota de vehículos, controlando viajes, choferes, cargas de combustible, operaciones de mantenimiento y talleres externos.

A continuación, se detalla la estructura de cada tabla y las relaciones que se establecen:

---

### 1. Tablas y Estructura

* **VIAJES**
* `ID_VIAJE` (PK)


* `FECHA`


* `ORIGEN`


* `DESTINO`


* `CARGA_KG`




* **VEHICULO**
* `PATENTE` (PK)


* `NUMERO_CHASIS`


* `AÑO_FABRICACION`


* `MARCA`


* `MODELO`


* `KILOMETRAJE`




* **ASIGNACION_CHOFER** *(Tabla intermedia / asociativa entre Viajes y Chofer)*
* `ID_VIAJE` (PK / FK)


* `ID_CHOFER` (PK / FK)


* `ROL`




* **CHOFER**
* `ID_CHOFER` (PK)


* `NOMBRE`


* `DNI`


* `FECHA_VENCIMIENTO_LICENCIA`


* `CATEGORIA_LICENCIA`




* **CARGA_COMBUSTIBLE**
* `ID_CARGA` (PK)


* `FECHA`


* `LITROS_CARGADOS`


* `COSTO_LITRO`


* `LECTURA_ODOMETRO`


* `PATENTE` (FK) — Relacionado con `VEHICULO`




* **MANTENIMIENTO**
* `ID_MANTENIMIENTO` (PK)


* `FECHA_INGRESO`


* `COSTO`


* `DESCRIPCION`


* `REPUESTOS_UTILIZADOS`


* `CUIT` (FK) — Relacionado con `TALLER`


* `PATENTE` (FK) — Relacionado con `VEHICULO`




* **TALLER**
* `CUIT` (PK)


* `NOMBRE`


* `DIRECCION`





---

### 2. Relaciones Principales

* **Vehículo y Viajes:** Un vehículo realiza múltiples viajes a lo largo del tiempo, mientras que un viaje es asignado a un vehículo específico (relación de uno a muchos).


* **Viajes y Chofer (`ASIGNACION_CHOFER`):** La tabla asociativa resuelve la relación de muchos a muchos entre los viajes y los choferes, permitiendo asignar uno o varios conductores a un viaje y especificando el rol que cumple cada uno (por ejemplo, conductor principal o acompañante).


* **Vehículo y Carga de Combustible:** Un vehículo registra múltiples cargas de combustible para el control de consumo, asociadas mediante su `PATENTE` (relación de uno a muchos).


* **Vehículo, Mantenimiento y Taller:** Un vehículo puede recibir diversas intervenciones de mantenimiento a lo largo de su vida útil. A su vez, los mantenimientos se vinculan con el taller externo que los realiza mediante el `CUIT` del establecimiento.
<img width="1486" height="832" alt="EJERCICIO 10-MR drawio" src="https://github.com/user-attachments/assets/bc5e9354-2763-4e4a-9228-ed4427127b99" />

La imagen muestra el proceso completo de **normalización de bases de datos** aplicado de manera secuencial —desde una tabla inicial sin normalizar hasta alcanzar la Tercera Forma Normal (3NF)— para un sistema de gestión de flota de vehículos, viajes, choferes, cargas de combustible, operaciones de mantenimiento y talleres externos.

A continuación, se detalla lo que se realiza en cada fase representada en el esquema:

---

### 1. Fase Normal N°0 (Tabla Desnormalizada)

* **Estado inicial:** Se parte de una **única tabla grande y plana** donde cada fila agrupa los datos de un vehículo junto con listas anidadas y multivaluadas en una misma celda (datos de los viajes realizados y sus cargas en kg, listas empaquetadas de choferes asignados con sus roles, múltiples cargas de combustible con fechas, litros y costos, y registros detallados de mantenimiento junto con los datos de los talleres externos).
* **Problema:** Presenta grupos repetitivos anidados, falta de atomicidad y una severa redundancia de información, lo que genera anomalías operativas de inserción, actualización y borrado.

### 2. Primera Forma Normal (1NF)

* **Condición:** Se exige que todos los atributos contengan **valores atómicos** (indivisibles, un solo dato por celda) y que cada tabla disponga de una clave primaria identificativa.
* **Proceso aplicado:** Se desglosan los grupos anidados en filas tabulares individuales:
* **TABLA PLANILLA:** Aplana la información operativa vinculando la patente del vehículo, los datos del viaje, los choferes y los roles en una estructura tabular estándar.



### 3. Segunda Forma Normal (2NF)

* **Condición:** La base de datos debe estar en 1NF y **todos los atributos que no forman parte de la clave deben depender por completo de toda la clave primaria** (eliminando las dependencias parciales generadas por claves compuestas).
* **Proceso aplicado:** Se separan las entidades base según la clave exacta de la que dependen:
* **TABLA VEHICULO:** Aísla las características técnicas y la patente del vehículo (`PATENTE` como PK).
* **TABLA VIAJES:** Contiene la información propia de cada trayecto (`ID_VIAJE` como PK, fecha, origen, destino y carga en kg).



### 4. Tercera Forma Normal (3NF)

* **Condición:** Debe estar en 2NF y **carecer de dependencias transitivas** (ningún atributo no clave debe depender de otro atributo que tampoco sea clave; todo atributo no clave debe depender estrictamente de la clave primaria).
* **Proceso aplicado:** Se termina de modularizar y aislar por completo todas las entidades independientes del sistema logístico, conformando las tablas definitivas:
* **TABLA VEHICULO y TABLA VIAJES:** Mantienen el registro central de la flota y los trayectos.
* **TABLA CHOFER y TABLA ASIGNACION_CHOFER:** Desglosan la información personal de los conductores (`ID_CHOFER` como PK) y resuelven la relación de muchos a muchos mediante la tabla asociativa con los viajes.
* **TABLA CARGA_COMBUSTIBLE:** Registra de manera independiente cada recarga de combustible por vehículo y su lectura de odómetro.
* **TABLA TALLER y TABLA MANTENIMIENTO:** Aíslan los datos de los talleres externos (`CUIT` como PK) y el historial de reparaciones, repuestos y costos asociados a cada vehículo.
<img width="5552" height="3037" alt="EJERCICIO 10-FASES DE NORMALIZACION drawio" src="https://github.com/user-attachments/assets/7e70a83a-2195-4c92-9922-728c319156d3" />

