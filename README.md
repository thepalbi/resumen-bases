## Introducción

Bases de datos, algunas definiciones:
- Colección de datos relacionados y organizados.
- Colección lógicamente coherente de datos con un signficado que depende de su dominio de aplicación.

DBMS (database management system): Software para el manejo y administración de bases de datos.

<img src="imgs/README-2022-07-07-20-34-25.png" width="500">

<img src="imgs/README-2022-07-07-20-34-35.png" width="500">

> Qué funcionalidades proveen? Taxonomía?

MER: Modelo entidad relación Representa entidades del mundo real y relaciones entre ellas. Consiste en el diseño o planificación de una base de datos.

DER: Diagrama entidad relación Herramienta pra visualizar un MER.

Pasos para construir un MER:
1. Identificar entidades
1. Identificar atributos para entidades. Estos pueden ser simples, compuestos, derivados o multivaluados.
1. Identificar las claves en cada entidad
1. Identificar relaciones. Estas pueden ser unarias, binarias o ternarias. Además, cada uno puede ser total o parcial. Además, en el caso de las relaciones ternarias, estas pueden poseer atributos.
1. Cardinalidad de las relaciones.
1. Entidades débiles.

Un MER puede ser expresado como un modelo relacional. En este, una entidad con sus atributos son repsentados como una **relación**, donde cada instancia es llamada una tupla.

> Sobre estas relaciones, tuplas y atributos son sobre las que están definidos los lenguajes de consulta.

## Lenguajes de consulta
### AR: Algebra relacional

Lenguaje imperativo (explícito) sobre un modelo relacional. Permite expresar consultas sobre estos. Contruído como si el modelo relacional y sus operaciones definieran un álgebra (también llamado álgebra relacional).

> TODO: Hacer resumen de las operaciones, con algún ejemplito, para tener una idea del poder de expresividad que tiene.

### CRT: Cálculo relacional de tuplas

Lenguaje declarativo que permite expresar cómo es el conjunto de respuestas que buscamos. SQL tiene sus bases fundacionales en este.

Las fórmulas tienen la siguiente estructura básica: 
$
{t | COND(t) }
$
Dónde $COND$ pueden ser expresiones de lógica de primer orden sobre tuplas de relaciones. $t$ es una tupla que prepresenta las tuplas resultado.

<img src="imgs/crt-ejemplo.png" width="500">

> Las slides tiene un descripción concisa del lenguaje, y después son puro ejercicio.

### Sobre expresividad

La **expresividad** de un lenguaje de consulta equivale al conjunto de conuslta que se le pueden hacer a una base de datos por medio de este lenguaje.

**CRT vs LPO**
CRT es una especialización de LPO. Una relación puede ser pensada como un predicado que indica si una instancia se encuentra o no en la base de datos.

CRT con expresiones seguras, o safe-CRT: CRT restringido a expresiones donde se garantiza que solo producen una cantidad finita de tuplas como resultado.

<img src="imgs/ejemplo-unsafe-crt.png" width="500">

**AR vs CRT** 
Bajo la condición de safe-CRT, tienen el mismo poder expresivo.

> LPO no tiene poder expresivo suficiente, por ejemplo si nuestro modelo fuera un grafo de vuelos de una aerolinea, y quisieramos consultar `es posible viajar de x hacia y?`

**Y con SQL?**
La semántica de SQL está basada en safe-CRT, por lo que a priori tiene el mismo poder expresivo que ART y safe-CRT.

## Normalización

Sea una relación **R** conformada por por atributos $A_1, A_2, A_3, ...$. Sea **S** un subconjunto de ellos, entonces se dice que S es una super clave de R si 
$
\not\exists t_1, t_2 ( t_1 \neq t_2 \land legal(t_1) \land legal(t_2) \land t_1(S) = t_2(S) )
$

Es decir, si $t_1$ y $t_2$ son dos tuplas de la relación legales, si sus atributos dentro de $s_i \in S$ cumplen que $t_1(s_i) = t_2(s_i)$ entonces $t_1=t_2$.

> está bien esto de arriba?

**Definiciones pegadas del libro**

<img src="imgs/relation-schema-quote.png" width="500">
<img src="imgs/relation-state-quotoe.png" width="500">

Básicamente **relation schema**, o esquema de relación denota a la estructura $R(A_1, A_2, ..., A_n)$. Esta es considerada el "esquema" de la relación, el prototipo. Dentro de un esquema $R$, se pueden tener **relation states**, o **estado** a secas.
Se denotan como $r(R)$.
Estos son un conjunto ${t_1, t_2, ..., t_m}$ de $m$ tuplas. Cada una de estas representa una instancia del esquema $R$. Es decir, una valuación de cada atributo $A_i$.

> Libro página 151

De aqui suerge el concepto de estado legal (o **legal relation state**). Dado un conjunto $F$ de dependencias funcionales (DFs), se dice que un estaod $r(R)$ es legal si cumple toda las DF $f \in F$. 
Recordar que **cumplir o validar** una DF es una propiedad semántica, ua que depende del significado de los atributos.

<img src="imgs/diagrama-de-claves.png" width="500">