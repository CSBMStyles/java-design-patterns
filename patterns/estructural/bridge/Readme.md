# Bridge


> [!NOTE]
> Puente


## Proposito

**Bridge** es un patrón de diseño estructural que te permite dividir una clase grande, o un grupo de clases estrechamente relacionadas, en dos jerarquías separadas (abstracción e implementación) que pueden desarrollarse independientemente la una de la otra.


![alt text](image.png)


## Problema

¿Abstracción? ¿Implementación? ¿Asusta? Mantengamos la calma y veamos un ejemplo sencillo.

Digamos que tienes una clase geométrica ``Forma`` con un par de subclases: ``Círculo`` y ``Cuadrado``. Deseas extender esta jerarquía de clase para que incorpore colores, por lo que planeas crear las subclases de forma ``Rojo`` y ``Azul``. Sin embargo, como ya tienes dos subclases, tienes que crear cuatro combinaciones de clase, como ``CírculoAzul`` y ``CuadradoRojo``.


![alt text](image-1.png)


Añadir nuevos tipos de forma y color a la jerarquía hará que ésta crezca exponencialmente. Por ejemplo, para añadir una forma de triángulo deberás introducir dos subclases, una para cada color. Y, después, para añadir un nuevo color habrá que crear tres subclases, una para cada tipo de forma. Cuanto más avancemos, peor será.

## Solucion

Este problema se presenta porque intentamos extender las clases de forma en dos dimensiones independientes: por forma y por color. Es un problema muy habitual en la herencia de clases.

El patrón Bridge intenta resolver este problema pasando de la herencia a la composición del objeto. Esto quiere decir que se extrae una de las dimensiones a una jerarquía de clases separada, de modo que las clases originales referencian un objeto de la nueva jerarquía, en lugar de tener todo su estado y sus funcionalidades dentro de una clase.

![alt text](image-2.png)

Con esta solución, podemos extraer el código relacionado con el color y colocarlo dentro de su propia clase, con dos subclases: ``Rojo`` y ``Azul``. La clase ``Forma`` obtiene entonces un campo de referencia que apunta a uno de los objetos de color. Ahora la forma puede delegar cualquier trabajo relacionado con el color al objeto de color vinculado. Esa referencia actuará como un puente entre las clases ``Forma`` y ``Color``. En adelante, añadir nuevos colores no exigirá cambiar la jerarquía de forma y viceversa.

### Terminos

El libro de la GoF  introduce los términos *Abstracción e Implementación* como parte de la definición del patrón Bridge

La Abstracción (también llamada interfaz) es una capa de control de alto nivel para una entidad. Esta capa no tiene que hacer ningún trabajo real por su cuenta, sino que debe delegar el trabajo a la capa de implementación (también llamada plataforma).

Ten en cuenta que no estamos hablando de las interfaces o las clases abstractas de tu lenguaje de programación. Son cosas diferentes.

Cuando hablamos de aplicación reales, la abstracción puede representarse por una interfaz gráfica de usuario (GUI), y la implementación puede ser el código del sistema operativo subyacente (API) a la que la capa GUI llama en respuesta a las interacciones del usuario.

En términos generales, puedes extender esa aplicación en dos direcciones independientes:

- Tener varias GUI diferentes (por ejemplo, personalizadas para clientes regulares o administradores).

- Soportar varias API diferentes (por ejemplo, para poder lanzar la aplicación con Windows, Linux y macOS).

En el peor de los casos, esta aplicación podría asemejarse a un plato gigante de espagueti, en el que cientos de condicionales conectan distintos tipos de GUI con varias API por todo el código

![alt text](image-3.png)

Puedes poner orden en este caos metiendo el código relacionado con combinaciones específicas interfaz-plataforma dentro de clases independientes. Sin embargo, pronto descubrirás que hay *muchas* de estas clases. La jerarquía de clase crecerá exponencialmente porque añadir una nueva GUI o soportar una API diferente exigirá que se creen más y más clases.

Intentemos resolver este problema con el patrón Bridge, que nos sugiere que dividamos las clases en dos jerarquías:

- Abstracción: la capa GUI de la aplicación.
- Implementación: las API de los sistemas operativos.

![alt text](image-4.png)

El objeto de la abstracción controla la apariencia de la aplicación, delegando el trabajo real al objeto de la implementación vinculado. Las distintas implementaciones son intercambiables siempre y cuando sigan una interfaz común, permitiendo a la misma GUI funcionar con Windows y Linux.

En consecuencia, puedes cambiar las clases de la GUI sin tocar las clases relacionadas con la API. Además, añadir soporte para otro sistema operativo sólo requiere crear una subclase en la jerarquía de implementación.

## Estructura

![alt text](image-5.png)

1. La **Abstracción** ofrece lógica de control de alto nivel. Depende de que el objeto de la implementación haga el trabajo de bajo nivel.

2. La **Implementación** declara la interfaz común a todas las implementaciones concretas. Una abstracción sólo se puede comunicar con un objeto de implementación a través de los métodos que se declaren aquí.

> [!NOTE]
> La abstracción puede enumerar los mismos métodos que la implementación, pero normalmente la abstracción declara funcionalidades complejas que dependen de una amplia variedad de operaciones primitivas declaradas por la implementación.

3. Las **Implementaciones Concretas** contienen código específico de plataforma.

4. Las **Abstracciones Refinadas** proporcionan variantes de lógica de control. Como sus padres, trabajan con distintas implementaciones a través de la interfaz general de implementación.

5. Normalmente, el **Cliente** sólo está interesado en trabajar con la abstracción. No obstante, el cliente tiene que vincular el objeto de la abstracción con uno de los objetos de la implementación.


## Implementacion

Este ejemplo ilustra cómo puede ayudar el patrón **Bridge** a dividir el código monolítico de una aplicación que gestiona dispositivos y sus controles remotos. Las clases ``Dispositivo`` actúan como implementación, mientras que las clases ``Remoto`` actúan como abstracción.

![alt text](image-6.png)


## Aplicabilidad

- Utiliza el patrón Bridge cuando quieras dividir y organizar una clase monolítica que tenga muchas variantes de una sola funcionalidad (por ejemplo, si la clase puede trabajar con diversos servidores de bases de datos).

- Utiliza el patrón cuando necesites extender una clase en varias dimensiones ortogonales (independientes).

- Utiliza el patrón Bridge cuando necesites poder cambiar implementaciones durante el tiempo de ejecución.

> [!IMPORTANT]
> Por cierto, este último punto es la razón principal por la que tanta gente confunde el patrón Bridge con el patrón Strategy. Recuerda que un patrón es algo más que un cierto modo de estructurar tus clases. También puede comunicar intención y el tipo de problema que se está abordando.

## Como Usarlo

1. Identifica las dimensiones ortogonales de tus clases. Estos conceptos independientes pueden ser: *abstracción/plataforma, dominio/infraestructura, front end/back end, o interfaz/implementación*.

2. Comprueba qué operaciones necesita el cliente y defínelas en la clase base de abstracción

3. Determina las operaciones disponibles en todas las plataformas. Declara aquellas que necesite la abstracción en la interfaz general de implementación.

4. Crea clases concretas de implementación para todas las plataformas de tu dominio, pero asegúrate de que todas sigan la interfaz de implementación.

5. Dentro de la clase de abstracción añade un campo de referencia para el tipo de implementación. La abstracción delega la mayor parte del trabajo al objeto de la implementación referenciado en ese campo.

6. Si tienes muchas variantes de lógica de alto nivel, crea abstracciones refinadas para cada variante extendiendo la clase base de abstracción.

7. El código cliente debe pasar un objeto de implementación al constructor de la abstracción para asociar el uno con el otro. Después, el cliente puede ignorar la implementación y trabajar solo con el objeto de la abstracción.


## Pros y contras

Pros  | Contras
------------- | -------------
Puedes crear clases y aplicaciones independientes de plataforma.  |   Puede ser que el código se complique si aplicas el patrón a una clase muy cohesionada.
El código cliente funciona con abstracciones de alto nivel. No está expuesto a los detalles de la plataforma.  |    
Principio de abierto/cerrado. Puedes introducir nuevas abstracciones e implementaciones independientes entre sí.  | 
Principio de responsabilidad única. Puedes centrarte en la lógica de alto nivel en la abstracción y en detalles de la plataforma en la implementación.  | 
