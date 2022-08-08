**Julio 2022**

- <span style="color:green">Qué parte del DBMS se encarga de garantizar aislamiento</span>

En un DBMS, llamamos aislamiento (isolation) a la propiedad que dos transacciones que son ejecutadas simultaneamnete no interfieran entre si. El componente encargado de esto es el subsistema de control de concurrencia. Realiza esta tarea controlando el acceso de las transacciones a los múltiples data items que forman una DB.

- <span style="color:green">Modelás una biblioteca con un DER, dar una interrelación 1-n y una n-m</span>

<img src="imgs/ejemplo-relaciones.png" width="500">

- <span style="color:red">Te daba una query en sql y había que escribirla en ar, dar el árbol canónico, dar 2 planes de ejecución y decir creo que una optimización que se pueda hacer si se tienen estadísticas sobre los datos</span>
- <span style="color:red">Para qué sirve el logging y explicar undo/redo</span>
- <span style="color:green">Qué es falsa sumarización y qué nivel de aislamiento se necesita en sql para evitarlas</span>

<img src="imgs/falsa-sumarizacion-ejemplo.png" width="500">

Falsa sumarización es un problema que surge cuando una transacción realiza algun tipo de agregación sobre data items (sumarización) y otra afecta alguno/s de ellos, siendo esta modificación no incliuida en la sumarización.

Se necesita el nivel de aislamiento SERIALIZABLE para que no ocurra este tipo de errores.

> Quoting [SQL server docs](https://docs.microsoft.com/en-us/sql/t-sql/statements/set-transaction-isolation-level-transact-sql?view=sql-server-ver16): Range locks are placed in the range of key values that match the search conditions of each statement executed in a transaction. This blocks other transactions from updating or inserting any rows that would qualify for any of the statements executed by the current transaction.

- <span style="color:green">Que es árbol de decisión y dar un ejemplo</span>

Los árboles de decisión una una técnica utilizada en data mining. Forman parte de lo que es llamado el aprendizaje supervisado, que apunta a resolver problemas de clasificación/descubrimiento/inferencia teniendo como entrada datos previamente clasificados.

En este caso, los árboles de decisión trabajan construyendo un árbol que en cada nivel toma una decisión con respecto a los atributos del dato de entrada, lleganod a una decisión con respecto a la clasificación para la que fue entrando en las hojas.

Un ejemplo para saber si comprar un auto o no podría ser:

<img src="imgs/arbol-de-decision.png" width="500">

- <span style="color:yellow">Que dice el teorema CAP y relacionarlo con ACID</span>

El teorema CAP es un teorema enunciado sobre los sistemas distribuidos. El mismo enuncia que de las siguientes 3 propiedades de un sistema distribuido:
- Consistencia
- Availability (disponibilidad)
- Tolearncia a particiones (partition tolearance)
Solo dos pueden ser garantizadas a la vez. También, teniendo un sistema distribuido como una base de datos que sabes que tiene que ser tolerante a particiones (de otra forma no tendría sentido que tenga múltiples nodos ya que son inevitables) solo es posible asegurar C o A.

ACID hace respecto un conjunto de propiedes que son deseables de un sistema transaccional con el cual interactua un usuario (como lo es una base de datos relacional). Generalmente, el mismo es pensado para una BD no distribuida, es decir, que tiene un solo nodo con el cual el usuario interactúa directamente. 

Las bases de datos tienen otro conjunto de propiedades llamado BASE, el cual caracteriza como es la interacción de un usuario con ellas. Este es:
- Basic Availability
- Soft State
- Eventual Consistency

<mark>TODO: Esta bien esto?</mark>

- <span style="color:red">Por qué en NoSQL se necesita la consistencia eventual</span>
- <span style="color:green">Qué es open data y sus características principales</span>

Se llama Open Data a la disponibilización de datos gubernamentales por parte del estado, a travez de internet, de forma de promover su análisis y re-utilización.

Las caracterísiticas principales de los mismos son:
- Que sean procesables por computadoras, es decir, un formate fácil de parsear y semi-estructurado por lo menos
- Sin licencia
- Totalmente abiertos, sin necesidad de registro
- Tener la menor alteración posible, y sean proveídos lo más cercano a su fuente posible
- Completos
- Compartidos en tiempo, sin retrasos a la fecha de los datos mismos

- <span style="color:green">**Qué es gobierno de datos y su relación con calidad de datos**</span>

Gobierno de datos se llama la desarrollo de técnicas, arquitectures y procedimientos para el manejo de datos dentro de una organización, como puede ser una empresa, gobierno, universidad, etc. Incluye aspectos de como se tratan los datos, como se almacenan y protejen. Como estás técnicas se diseñan e implementan con el fin de proteger los datos de las malas prácticas, de esta manera haciendo más valiosos ya que pueden ser explotados para la extracción de información, se dice que es escencial para asegurar la calidad de los datos.

- <span style="color:green">**Diferencia entre datos estructurados y datos semi-estructurados**</span>

Los datos estructurados son aquelos que fueron formateados y trasformados de forma de encajar en un modelo bien definido. Tienen campos pre-definidos los cuales poseen un nombre y tipo de datos. Por ejemplo, los datos que son almacenados en DBs relacionales son estructurados.

A diferencia de estos, los semiestructurados fueron transformados para acatar a un formato standard de forma de ser fácilmente parseados por un programa, pero carecen de la estructura que les da un modelo bien definido. A pesar de esto, se caracterizan por ser más flexibles y son muy usados en la industria para representar meta-información sobre alguna entidad, o atributos variables.

- <span style="color:green">Tipos de fragmentación que puede haber en una base distribuida y dar ejemplos</span>.

Fragmentar consiste en particionar los datos de una DB, a travéz de mútiples nodos que forman parte de la misma. De esta forma, se puede repartir la carga de almacenamiento entre todos, y escalar más fácilmente. Tomemeos como ejemplo una relación `Empleado(id, nombre, apellido, dirección, organizacion_id, manager_id, ...)` Hay 3 tipos de fragmentación

- Horizontal: Consiste en particionar las tuplas (o entradas) dentro de una relación entre múltiples nodos. Se utiliza un criterio fijo para realizar la fragmentación, ya que el mismo debe ser utilizado para insertar, modificar y buscar instancias en particulares. Por ejemplo, en nuestra relación `Empleado` podría ser por `organización_id`. De esta forma cada nodo tendría los empleados de algunas de las organizaciones. También es conocido como sharding.
- Vertical: Consisten en partir la relación en conjuntos de atributos, cada una asignada a un nodo, y almacenar en cada uno la porción de todas las tuplas que le corresponda. Notar que aquí hace falta guardar también la PK en cada una para poder reconstruir la relación entera. Un ejemplo sería guardar en un nodo la info. personal del empleado `[id, nombre, apellido, dirección]` y en otro la info de laboral `org_id, manager_id, ....`.
- Mixto: Consiste en mezclar los dos anteriores.

- <span style="color:green">**Qué es una ontología y dar un ejemplo**</span>

El estudio de ontologías trata de describir los conceptos y relaciones que son posibles en algún dominio por medio de un vocalubario común. Una ontología esta compuesta por una conceptualización que el conjunto de conceptos y relaciones entre ellos que son parte del dominio de nuestro interés, y un lenguaje y vocabulario definido para poder especificar estas conceptualizaciones.

Por ejemplo imaginemos que definimos una ontología sobre pizzas. Definimos un concepto o clase de **Pizza**, y otra **Pizza vegetariana** que es una Pizza también, pero no tiene ingredientes que correspondan a la clase **Carne**. Ahora si creamos una instancia de Pizza que no posee ingredientes carnivoros, mediante la ontología se puede inferir que esta es una Pizza vegetariana.

Otro ejemplo que es una trabajo en curso es la Web Semántica, que trata de definir una ontología compartida por todas computadoras de la Internet para facilitar el intercambio de información.