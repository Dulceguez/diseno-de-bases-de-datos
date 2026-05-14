# 📊 Modelado Conceptual – Bases de Datos

Este documento resume el proceso de **Diseño Conceptual** en bases de datos utilizando el Modelo Entidad–Relación (MER), incluyendo su definición, simbología y pasos para resolver ejercicios.<br><br><br>





## 📌 Definición

El diseño conceptual parte de la especificación de requerimientos y su resultado es el esquema conceptual de la base de datos.

Su objetivo es describir el contenido de información de la base de datos a un alto nivel de abstracción, sin considerar detalles de implementación.

La herramienta utilizada es el **Modelo Entidad–Relación (MER)**.
<br><br><br>


## 📌 Simbología del Modelo E/R

### 🧩 Entidad
Representa un objeto o elemento del mundo real con identidad propia.



### 🔗 Relación
Asociación entre dos o más entidades que describe dependencias o vínculos.



### 🔁 Relación recursiva
Relación donde una entidad se relaciona consigo misma.



### 🏷️ Atributo
Propiedad que describe a una entidad o relación (equivalente a un campo en una tabla).



### 🔑 Identificador (clave)
Atributo o conjunto de atributos que identifica de forma única una entidad.



### 🧱 Atributo compuesto
Atributo formado por la combinación de varios atributos simples.



## 🔢 Cardinalidad de atributos

- (1,1) → Monovalente obligatorio
- (0,1) → Monovalente opcional
- (0,N) → Multivaluado opcional
- (1,N) → Multivaluado obligatorio
<br><br>


## 🔗 Cardinalidad en relaciones

Define el grado de correspondencia entre entidades:

Ejemplo:
```text
Alumno (1,N) —— Cursada —— (0,N) Materia