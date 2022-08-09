## Julio 2022

- <span style="color:green">Qué parte del DBMS se encarga de garantizar aislamiento</span>

En un DBMS, llamamos aislamiento (isolation) a la propiedad que dos transacciones que son ejecutadas simultaneamnete no interfieran entre si. El componente encargado de esto es el subsistema de control de concurrencia. Realiza esta tarea controlando el acceso de las transacciones a los múltiples data items que forman una DB.

- <span style="color:green">Modelás una biblioteca con un DER, dar una interrelación 1-n y una n-m</span>

<img src="imgs/ejemplo-relaciones.png" width="500">

- <span style="color:red">Te daba una query en sql y había que escribirla en ar, dar el árbol canónico, dar 2 planes de ejecución y decir creo que una optimización que se pueda hacer si se tienen estadísticas sobre los datos</span>
- <span style="color:green">Para qué sirve el logging y explicar undo/redo</span>

Logging es un mecanismo que sirve para, al ocurrir una falla no catastrófica en el DBMS, poder recuperar el estado de la base de datos que maneja a uno consistente. Esto consiste en que las transacciones que son ejecutadas, aparte de afectar tanto la base de datos en disco como la parte en memoria de la misma (que se encuentra en el buffer, o cacheada), crean un registro en un archivo write-ahead por cada operación de escritura que realizan. De esta forma, y con diversos mecanismos, se puede recuperar el estado a uno consistente con respecto a qué transacciones fueron commiteadas y cuáles no.

Undo/redo es uno de estos mecanismos, el cual aparte caracteriza de qué forma maneja los commit el cache manager. En este caso, se dice que es:
- Steal: Permite que se bajen a disco escrituras en bloques, cuya transacción no fue commiteada. Esto permite que se reutilice espacio en el buffer en memoria principal.
- No Force: Cuando una transacción se commitea, no hace falta que todas las modificaciones de la misma sean escritas en disca en el acto. Esto hace que si ocurren sucesivas modificaciones sobre un data item, ahorrar tráfico de IO.

Estas dos características hace que durante el recovery, haya que tanto re-hace (REDO) operaciones que fueron commiteadas, pero no escritas en disco (por no-force); y des-hacer escrituras en disco de transacciones que no hayan commiteado (UNDO). Por eso, ante cada write, una transacción debe escribir en el log una entrada con la siguiente forma:
```
(write_item, transaction_id, data_item_id, value_before, value_after)
```

De esta forma, al ocurrir una falla (asumiendo que hubo un checkpoint previamente), se procede de la siguiente manera:
```
commited_txs = transacciones commiteadas desde el último checkpoint, es decir que se encuentre después del checkpoint una entrada `COMMIT tx_i`
active_txs = transacciones activas desde el último checkpoint, es decir que estaban en el checkpoint como activas y no commitearon, o que empezaron y no commitearon

// UNDO
for (write_item, tx, X, before, after) leyendo desde adelante para atras:
    if tx in active_txs:
        write(X, before)

for (write_item, tx, X, before, after) leyendo desde atras para adelante:
    if tx in commited_txs:
        write(X, after)

cancelar y re-submittear tx in active_txs
```

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

- <span style="color:green">Por qué en NoSQL se necesita la consistencia eventual</span>

Un caso de uso muy común de bases de datos NoSQL, es en web services de gran escala, los cuales comunmente son distribuidos. De esta forma, y al corresponder a un sistema compuesto por muchos nodos comunicados entre si por Internet (o algun tipo de red de computadoras), entra en juego el teorema CAP. El mismo dice que de las tres propiedades que enuncia, solo dos pueden ser válidas. Particion a tolerancia es un requisito obligatorio, ya que de otra forma si un nodo no fuera alcanzable (ya sea por una falla de red o en el mismo, lo cual ocurre muy a menudo a la escala en la que estos sistemas suelen usarse) el sistema se tornaría inutilizable. 
De estas dos, por el teorema CAP, solo nos queda garantizar disponibilidad o consistencia. Como usualmente los servicios que consumen estas DBs sirven usuarios finales, los cuales esperan una respuesta en tiempo acorde, disponibiblidad es la propiedad más frecuentemente elegida. Ya que aparte, la solución consiste generalmente en relajar la definición de consistencia, dando paso a la consistencia eventual.

La misma dice que el sistema no tiene que ser perfectamente consistente (que al ejecutar un READ, el valor respondido corresponda al del WRITE más reciente), sino que es admisible que en algún momento en el futuro el cambio se vea reflejado en todo nodo. Es decir, que **eventualmente** sea consistente.

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

## Agosto 2022

- <span style="color:red">**¿Qué son las propiedades BASE? ¿Qué relación tienen con las ACID?**</span>
- <span style="color:red">**¿Qué es una transacción? ¿Qué significa que una transacción lea de otra? Definir dirty read y dar ejemplo.**</span>
- <span style="color:red">**¿Qué diferencia hay entre bases distribuídas de sitio primario y de copia primaria? Dar ventajas y desventajas de ambas.**</span>
- <span style="color:red">**¿Qué es una historia y cuándo dos historias son equivalentes en conflicto?**</span>
- <span style="color:red">**¿Qué es el proceso de normalización y para qué sirve? ¿Cómo está relacionado con la calidad de un diseño de bases de datos?**</span>

## Marzo 2020

- <span style="color:red">**Qué es una base de datos distribuida. Describir el protocolo 3FN.**</span>
- <span style="color:red">**Ejercicio optimización**</span>
```
Tenés 2 relaciones E = {idEstudiante, nombreEstudiante, idFacultad, fechaInscripción} y F = {idFacultad, nombreFacultad}. Los registros de E miden 30 B, y hay 10.000 de ellos. Los registros de F miden 20 B y hay 500 de ellos. Hay 3 nodos N1, N2 y N3. N1 tiene a E, N2 tiene a F, y N3 hace la query 'Devolver id de estudiante y nombre de la facultad de los estudiantes que se inscribieron después de 1980'.

i) ¿Que agregarías a la BD para capturar la siguiente situación: 'un estudiante puede inscribirse a varias facultades, pero en fechas distintas'?
ii) Escribir la query en AR y en CR
iii) Describir una estrategia de resolución de la query, junto con cuantos bytes son transferidos. Reemplace por variables las cantidades desconocidas (con eso se refiere a que no sabés cuantos estudiantes se inscribieron después de 1980, y que tendrías que reemplazar dicha cantidad por una variable).
```
- <span style="color:red">**Ejercicio raro**</span>
```
7) Supongamos que hay 4 facultades f1, f2, f3 y f4; y 4 nodos correspondientes N1, N2, N3 y N4. Se quiere fragmentar la información de E y F del anterior punto, para que tanto la información de las facultades, como la de estudiantes esten solo en el nodo de su facultad correspondiente.

i) Definir qué queries en AR hacen falta para definir la fragmentación, y definir un esquema de asignación para obtener lo requerido.
ii) Decir qué tipo de fragmentación se usó en el anterior punto. ¿Cómo obtendría las relaciones originales luego de la fragmentación?
```

## Febrero 2020
- <span style="color:red">**Definir superclave, clave primaria y dependencia funcional.**</span>
- <span style="color:red">**¿Qué es gobierno de datos? Diferencias entre datos, información y conocimiento.**</span>
- <span style="color:red">**¿Qué es Data Mining? Describir las distintas técnicas.**</span>
- <span style="color:red">**¿Qué es la interoperabilidad de datos? Describir los dos enfoques que se mencionan en la bibliografía.**</span>

