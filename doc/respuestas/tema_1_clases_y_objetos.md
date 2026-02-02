<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

Aquí tienes las respuestas al cuestionario sobre "Clases y Objetos", adaptadas a un perfil con conocimientos de C/C++ procedimental y redactadas en tono impersonal, tal como se ha solicitado.

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta

Las cuatro características fundamentales son la **abstracción**, el **encapsulamiento**, la **herencia** y el **polimorfismo**. La **abstracción** consiste en aislar un elemento de su contexto o del resto de elementos que lo acompañan, definiendo una entidad del mundo real mediante sus atributos y comportamientos esenciales. En lugar de pensar en los detalles internos de implementación (como se haría con funciones complejas en C), se define una interfaz clara que representa "qué" hace el objeto, ignorando el "cómo".

El **encapsulamiento** es el mecanismo que agrupa datos y métodos en una sola unidad (la clase) y protege la integridad de los datos restringiendo el acceso directo a ellos. A diferencia de una `struct` en C donde todos los campos son públicos por defecto, aquí se pueden definir niveles de visibilidad para evitar modificaciones accidentales desde otras partes del código.

La **herencia** permite crear nuevas clases basadas en clases existentes, reutilizando su código y extendiendo su funcionalidad. Es similar a si se pudiera incluir una `struct` dentro de otra y automáticamente adquirir todas sus funciones asociadas, estableciendo una jerarquía de "padre" e "hijo". Por último, el **polimorfismo** es la capacidad que tienen objetos de diferentes clases de responder al mismo mensaje (o llamada a función) de distintas maneras. Esto permite escribir código genérico que funcione con diferentes tipos de datos, algo que en C requeriría punteros a función o uniones complejas.

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta

**Java** es uno de los lenguajes más representativos de este paradigma. Diseñado con la orientación a objetos desde su concepción, elimina la gestión manual de memoria y los punteros explícitos de C++, obligando a que casi todo el código resida dentro de clases. Es fuertemente tipado y se ejecuta sobre una máquina virtual.

**C++** es la evolución directa del lenguaje C que añade soporte para clases, herencia y polimorfismo, manteniendo la compatibilidad con el código C tradicional. Permite un control de bajo nivel sobre la memoria y es multiparadigma, ya que se puede programar en estilo puramente procedimental o completamente orientado a objetos.

**C# (C Sharp)** es un lenguaje desarrollado por Microsoft, sintácticamente similar a Java y C++, que se utiliza ampliamente en el entorno .NET. Al igual que Java, gestiona la memoria automáticamente y es puramente orientado a objetos en su estructura básica, aunque ha incorporado características de otros paradigmas modernos.

**Python** es un lenguaje interpretado de alto nivel que soporta múltiples paradigmas, incluida la orientación a objetos. A diferencia de los anteriores, su sintaxis es más limpia y no utiliza llaves para los bloques de código. En Python, "todo es un objeto", lo que ofrece una gran flexibilidad dinámica que no se encuentra en lenguajes compilados como C.

***Dinámicos*** Python, JavaScript,...
***Compilados*** C++, ¿Rust?, Java, C#,...

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta

La **programación estructurada** es un paradigma que busca mejorar la claridad y calidad del código utilizando únicamente subrutinas y tres estructuras de control básicas: secuencia, selección (if/else) e iteración (bucles for/while). Surge como solución al "código espagueti" provocado por el abuso de la sentencia `GOTO` en lenguajes antiguos. En el contexto de C, se refleja en el diseño de algoritmos claros dentro de funciones, donde el flujo de ejecución es predecible y ordenado de principio a fin.

Por su parte, la **programación modular** es un paso más allá en la organización del software, donde el programa se divide en partes más pequeñas e independientes llamadas módulos. Cada módulo agrupa procedimientos y datos relacionados, ocultando su complejidad interna al resto del programa.

Para un programador de C, la programación modular se materializa en la separación del código en archivos de cabecera (`.h`) y archivos de implementación (`.c`). Mientras que la programación estructurada se enfoca en el flujo lógico dentro de una función, la modular se enfoca en la arquitectura general del proyecto, permitiendo que diferentes equipos trabajen en distintos ficheros sin interferir entre sí, un concepto que la POO lleva al extremo uniendo datos y funciones en "clases".

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta

Un objeto se define por su **identidad**, su **estado** y su **comportamiento**. La **identidad** es la propiedad que permite distinguir a un objeto de todos los demás, incluso si sus contenidos son idénticos. En un lenguaje como C, esto podría compararse con la dirección de memoria única de una variable; aunque dos estructuras tengan los mismos datos, si están en direcciones distintas, son entidades distintas.

El **estado** representa el conjunto de valores de todos los atributos (o variables miembro) del objeto en un instante dado. Es la información que el objeto almacena y que puede cambiar a lo largo del tiempo. Sería equivalente al contenido de los campos de una variable de tipo `struct` en C en un momento específico de la ejecución.

El **comportamiento** se define por las operaciones (métodos) que el objeto puede realizar o que se pueden realizar sobre él. A diferencia de C, donde las funciones están separadas de los datos, aquí el comportamiento está intrínsecamente ligado a los datos. Es la forma en que el objeto responde a las interacciones con otros objetos o con el sistema.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta

Una **clase** es una plantilla, molde o definición abstracta que describe los atributos y métodos comunes a un conjunto de objetos. Se puede entender como la definición del tipo de dato (similar a definir un `struct` en C, pero incluyendo funciones), mientras que un **objeto** es la concreción de esa clase en la memoria. Por tanto, no son lo mismo: la clase es el concepto o plano, y el objeto es la entidad real que ocupa espacio en tiempo de ejecución.

El término **instancia** es sinónimo de objeto; se dice que un objeto es una "instancia de una clase" específica. Cuando se declara una variable de un tipo de clase y se crea en memoria, se está "instanciando" la clase. Es análogo a la diferencia entre definir `struct Persona { ... };` (clase) y declarar `struct Persona juan;` (instancia/objeto).

No todos los lenguajes orientados a objetos utilizan clases. Existen lenguajes basados en **prototipos**, como JavaScript (en sus versiones originales) o Self. En estos lenguajes, no existen moldes o clases estáticas; en su lugar, los objetos se crean clonando otros objetos existentes (prototipos) y modificándolos según sea necesario. Sin embargo, lenguajes como Java o C++ sí están basados en clases.

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**?

### Respuesta

En la mayoría de los lenguajes orientados a objetos modernos como Java, los objetos se almacenan en el área de memoria dinámica conocida como **Heap** (montículo). Las variables que manejamos en el código son simplemente referencias (similares a punteros en C) que apuntan a esa dirección en el Heap. En contraste, lenguajes como C++ permiten almacenar objetos tanto en el Stack (pila) como en el Heap, dependiendo de cómo se declaren, ofreciendo mayor control pero mayor complejidad.

No es igual en todos los lenguajes. En C++, si se declara un objeto localmente sin usar `new`, vive en el Stack y se destruye automáticamente al salir del ámbito. En Java, todos los objetos (instancias de clase) viven obligatoriamente en el Heap, y lo único que reside en el Stack son las referencias a ellos.

La **recolección de basura** (Garbage Collection) es un mecanismo automático de gestión de memoria. A diferencia de C, donde el programador debe liberar explícitamente la memoria con `free()` para evitar fugas, el recolector de basura detecta los objetos que ya no tienen referencias activas (es decir, que son inalcanzables por el programa) y libera su memoria automáticamente. Esto simplifica el desarrollo y evita errores comunes de gestión de memoria.

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**?

### Respuesta

Un **método** es una subrutina o función que está asociada exclusivamente a una clase o a un objeto. Define el comportamiento de los objetos de esa clase. Para alguien que viene de C, un método es conceptualmente una función, pero que se declara dentro de la estructura de datos y tiene acceso implícito a los datos internos (atributos) de la instancia sobre la que se invoca, sin necesidad de pasarlos como argumentos.

La **sobrecarga de métodos** es una característica que permite definir varios métodos con el mismo nombre dentro de la misma clase, siempre y cuando su "firma" sea diferente. La firma se compone del nombre del método y la lista de tipos de sus parámetros.

Esto no es posible en C estándar, donde cada función debe tener un nombre único. En POO, el compilador distingue qué método llamar basándose en los argumentos que se le pasan. Por ejemplo, se podría tener un método `calcularArea()` sin parámetros y otro `calcularArea(float radio)`; el sistema sabrá cuál ejecutar según si se le envían datos o no.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta

A continuación se presenta el código solicitado. En Java, la clase define tanto la estructura de datos (`int x`, `int y`) como la función que opera sobre ellos (`calculaDistanciaAOrigen`). Nótese que el método `main` se utiliza como punto de entrada para ejecutar el programa, similar a la función `main` en C, pero debe estar encapsulado dentro de una clase.

```java
// Definición de la clase Punto
class Punto {
    // Atributos con visibilidad por defecto (package-private)(ESTADO)
    int x;
    int y;

    // Método que calcula la distancia al origen (0,0) usando Pitágoras
    // Devuelve un double porque la raíz cuadrada no suele ser entera
    double calculaDistanciaAOrigen() {
        // Math.sqrt es una función de la biblioteca estándar de Java
        return Math.sqrt(x * x + y * y);
    }
}

// Clase principal para probar el ejemplo
class EjemploUso {
    public static void main(String[] args) {
        // 1. Creación de la instancia (Objeto) en el Heap
        Punto miPunto = new Punto();

        // 2. Asignación de valores a los atributos
        miPunto.x = 3;
        miPunto.y = 4;

        // 3. Uso del método
        double distancia = miPunto.calculaDistanciaAOrigen();

        // Salida por pantalla
        System.out.println("La distancia al origen es: " + distancia);
    }
}

```

En este ejemplo, `new Punto()` reserva memoria dinámica (equivalente a un `malloc` en C) para el objeto. La variable `miPunto` actúa como una referencia (puntero seguro) a esa memoria. Al llamar a `miPunto.calculaDistanciaAOrigen()`, el método accede internamente a `this.x` y `this.y` correspondientes a esa instancia concreta.


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta
