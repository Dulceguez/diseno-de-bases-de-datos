# Modelo Lógico y Modelo Físico

Este repositorio contiene apuntes y conceptos sobre el proceso de transformación desde el **Modelo Entidad-Relación (ER)** hacia el **Modelo Lógico** y finalmente el **Modelo Físico Relacional**.
<br><br><br>
---

# Modelado Lógico

El propósito de generar un **Modelo ER Lógico** es convertir el esquema conceptual en una representación más cercana a cómo será entendida por un **SGBD (Sistema de Gestión de Bases de Datos)**.

Mientras el diseño conceptual busca representar claramente las necesidades del usuario, para ser entendido por éste, el diseño lógico busca generar un esquema equivalente pero más eficiente para su implementación en bases de datos relacionales.
<br><br><br>
---

# Decisiones sobre el Diseño Lógico

Las decisiones del diseño lógico están relacionadas principalmente con:

- Rendimiento del sistema
- Compatibilidad con modelos relacionales
- Resolución de estructuras que no existen directamente en los SGBD relacionales

## Principales decisiones

- Resolver jerarquías
- Resolver atributos compuestos
- Resolver atributos polivalentes
<br><br><br>
---

# 🌳 Resolver Jerarquías

Las jerarquías pueden clasificarse según:


### 📌 Total Exclusiva T,E)

Existen tres posibilidades:

- Dejar todas las entidades
- Dejar solo los hijos
- Dejar solo el padre


### 📌 Total Superpuesta (T,S)

Existen dos posibilidades:

- Dejar todas las entidades
- Dejar solo el padre

⚠️ No se puede eliminar el padre.


### 📌 Parcial Exclusiva (P,E)  

Existen dos posibilidades:

- Dejar todas las entidades
- Dejar solo el padre

⚠️ No se puede eliminar el padre.

### 📌 Parcial Superpuesta (P,S)

Existen dos posibilidades:

- Dejar todas las entidades
- Dejar solo el padre

⚠️ No se puede eliminar el padre.


# Resolución de Jerarquías

### Primera opción — Dejar todas las entidades

- Si las entidades hijas no tienen identificador, debe bajarse desde el padre.
- Si el hijo posee identificador propio, bajar el identificador es opcional.
- El identificador externo se toma desde la relación y no desde la entidad.

#### Ventajas

- Mayor claridad conceptual
- Menor redundancia

#### Desventajas

- Más tablas
- Consultas más complejas
<br><br><br>
---

### Segunda opción — Dejar solo el padre

#### Reglas

- Todos los atributos de los hijos pasan al padre.
- Los atributos heredados deben ser opcionales.
- Las relaciones de los hijos también pasan como opcionales.
- Un atributo identificador del hijo deja de ser identificador.
- Puede agregarse un atributo discriminador.


#### Ventajas

- Menor cantidad de tablas
- Consultas simples

#### Desventajas

- Muchos valores NULL
- Menor normalización
<br><br><br>
---

### Tercera opción — Dejar solo los hijos

#### Reglas

- Los atributos del padre se copian en cada hijo.
- También se copian las relaciones del padre.

#### Ventajas

- Tablas especializadas
- Mejor rendimiento

#### Desventajas

- Duplicación de datos
- Mayor mantenimiento
<br><br><br>
---

# Resolver Atributos Compuestos

Los atributos compuestos pueden eliminarse de dos maneras.
<br><br>

## Opción 1 — Separar atributos individuales

Ejemplo:

```text
Dirección
 ├── Calle
 ├── Número
 └── Ciudad
```

Se transforma en:

```sql
calle
numero
ciudad
```

#### Ventajas

- Mayor precisión
- Mejor filtrado
- Datos normalizados

#### Desventajas

- Mayor cantidad de columnas
<br><br><br>
---

## Opción 2 — Mantener un único atributo

```sql
direccion_completa
```

#### Ventajas

- Simplicidad
- Menor cantidad de atributos

#### Desventajas

- Difícil validación y búsqueda
- Menor normalización
<br><br><br>
---

# Resolver Atributos Polivalentes

Los atributos polivalentes son aquellos que pueden tener múltiples valores.

Ejemplo:

```text
Persona
 └── teléfonos
```

## Solución

Se debe:

1. Crear una nueva entidad
2. Crear una interrelación

### Resultado

```text
Persona ─── Tiene ─── Teléfono
```
<br><br>
---

# Modelo Físico

El modelo físico representa cómo se almacenarán realmente los datos dentro del SGBD.

La base de datos se representa como una colección de archivos llamados tablas.

## Conceptos básicos

- Cada tabla se denomina **relación**
- Cada fila se denomina **tupla**
- Cada columna representa un **atributo**
<br><br>
---

### 📌 Conversión de Entidades

Cada entidad se transforma en una tabla.

## Ejemplo

```text
Alumno = (dni, nombre, calle, nro, piso?, dpto?)
```

### Tabla resultante

```sql
Alumno(
    dni PK,
    nombre,
    calle,
    nro,
    piso NULL,
    dpto NULL
)
```

---

## Conversión de Relaciones

La transformación depende de las cardinalidades de la relación:


### 📌 Relación 0,1:1,1

### Caso 

```text
A (0,1) ─── R ─── B (1,1)
```

### Transformación

```sql
A(idA, atA)

B(
    idB,
    idA (FK),
    atB
)
```
### 📌 Relacion 1,1:1,1

### Caso

```text
A (1,1) ─── R ─── B (1,1)
```

### Transformacion

```sql
A(
    idA,
    idB (FK),
    atA
)

B(idB, atB)
```

---
### 📌 Relacion 0,1:0,1

### Caso

```text
A (0,1) ─── R ─── B (0,1)
```

### Transformación

```sql
A(
    idA,
    atA
)

B(
    idB,
    atB
)
R(idA(fk),
  idB(fk)
) 

o

R(idA(fk), 
  idB(fk)
)
```

### 📌 Relación 1,1:(0,N o 1,n)

### Caso

```text
A (1,1) ─── R ─── B (0,n)o(1,n)
```

### Transformación

```sql
A(
    idA,
    idB FK,
    atA
)

B(
    idB,
    atB
)
```

###  📌 Relacion 0,1:(0,n o 1,n)

### Caso 
```text
A (0,1) ─── R ─── B (0,n o 1,n)
```

### Transformación

```sql
A(
    idA,
    idB
)

B(
    idB,
    atB
)

R(
    idA(FK),
    idB(FK),
)
```

### 📌 Relación (1,n o 0,n):(1,n o 0,n)

### Caso

```text
A (1,n)o(0,n) ─── R ─── B (1,n)o(0,n)
```

### Transformación

```sql
A(idA)

B(idB, atB)

R(
    idA FK,
    idB FK,
    atR
)
```

### 🔁 Relacion Recursiva (0,n):(0,n) 

### Caso
```text
A (0,n) ─── Tiene correlativa a ─── A (0,n)
```

### Transformación

```sql
R(
    idA(FK),
    idB(FK),
)
```


# 🎯 Objetivos del Diseño Lógico y Físico

- Optimizar almacenamiento
- Mejorar rendimiento
- Garantizar integridad de datos
- Facilitar mantenimiento
- Adaptar el modelo al SGBD
