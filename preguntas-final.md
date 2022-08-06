**Julio 2022**

- <span style="color:red">Qué parte del DBMS se encarga de garantizar aislamiento</span>

<mark>TODO: Revisar esto</mark>

En un DBMS, llamamos aislamiento a la propiedad que dos transacciones que son ejecutadas simultaneamnete no interfieran entre si. El componente encargado de esto es el módulo de control de concurrencia (se llamaba así).

- <span style="color:green">Modelás una biblioteca con un DER, dar una interrelación 1-n y una n-m</span>

<img src="imgs/ejemplo-relaciones.png" width="500">

- <span style="color:red">Te daba una query en sql y había que escribirla en ar, dar el árbol canónico, dar 2 planes de ejecución y decir creo que una optimización que se pueda hacer si se tienen estadísticas sobre los datos</span>
- <span style="color:red">Para qué sirve el logging y explicar undo/redo</span>
- <span style="color:red">Qué es falsa sumarización y qué nivel de aislamiento se necesita en sql para evitarlas</span>
- <span style="color:red">Que es árbol de decisión y dar un ejemplo</span>
- <span style="color:red">Que dice el teorema CAP y relacionarlo con ACID</span>
- <span style="color:red">Por qué en No Sql se necesita la consistencia eventual</span>
- <span style="color:red">Qué es open data y sus características principales</span>
- <span style="color:green">**Qué es gobierno de datos y su relación con calidad de datos**</span>

Gobierno de datos se llama la desarrollo de técnicas, arquitectures y procedimientos para el manejo de datos dentro de una organización, como puede ser una empresa, gobierno, universidad, etc. Incluye aspectos de como se tratan los datos, como se almacenan y protejen. Como estás técnicas se diseñan e implementan con el fin de proteger los datos de las malas prácticas, de esta manera haciendo más valiosos ya que pueden ser explotados para la extracción de información, se dice que es escencial para asegurar la calidad de los datos.

- <span style="color:green">**Diferencia entre datos estructurados y datos semi-estructurados**</span>

Los datos estructurados son aquelos que fueron formateados y trasformados de forma de encajar en un modelo bien definido. Tienen campos pre-definidos los cuales poseen un nombre y tipo de datos. Por ejemplo, los datos que son almacenados en DBs relacionales son estructurados.

A diferencia de estos, los semiestructurados fueron transformados para acatar a un formato standard de forma de ser fácilmente parseados por un programa, pero carecen de la estructura que les da un modelo bien definido. A pesar de esto, se caracterizan por ser más flexibles y son muy usados en la industria para representar meta-información sobre alguna entidad, o atributos variables.

- <span style="color:red">Tipos de fragmentación que puede haber en una base distribuida y dar ejemplos</span>.

- <span style="color:green">**Qué es una ontología y dar un ejemplo**</span>

El estudio de ontologías trata de describir los conceptos y relaciones que son posibles en algún dominio por medio de un vocalubario común. Una ontología esta compuesta por una conceptualización que el conjunto de conceptos y relaciones entre ellos que son parte del dominio de nuestro interés, y un lenguaje y vocabulario definido para poder especificar estas conceptualizaciones.

Por ejemplo imaginemos que definimos una ontología sobre pizzas. Definimos un concepto o clase de **Pizza**, y otra **Pizza vegetariana** que es una Pizza también, pero no tiene ingredientes que correspondan a la clase **Carne**. Ahora si creamos una instancia de Pizza que no posee ingredientes carnivoros, mediante la ontología se puede inferir que esta es una Pizza vegetariana.

Otro ejemplo que es una trabajo en curso es la Web Semántica, que trata de definir una ontología compartida por todas computadoras de la Internet para facilitar el intercambio de información.