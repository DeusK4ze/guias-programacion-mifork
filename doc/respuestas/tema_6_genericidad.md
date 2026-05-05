# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta
En lenguajes como C, la forma tradicional de crear estructuras que acepten múltiples tipos de datos es mediante el uso de punteros genéricos `void*`, los cuales pueden apuntar a cualquier dirección de memoria independientemente de su contenido. En Java, el equivalente arquitectónico para lograr esto es utilizar la clase matriz `Object`. 

Dado que en el ecosistema Java absolutamente todas las clases heredan implícitamente de `Object` (gracias al polimorfismo), un array declarado de este tipo superior se convierte en un contenedor universal. Puede almacenar referencias a cadenas de texto, números envueltos (clases *wrapper*) o cualquier objeto personalizado dentro de la misma estructura contigua.

Esta estrategia permite definir la lógica de almacenamiento, inserción y extracción una única vez, evitando la necesidad de programar una clase lista específica para cada tipo de dato existente en el programa.

```java
// Ejemplo de estructura de datos universal en Java previa a la genericidad
class ListaUniversal {
    private Object[] elementos;
    private int tamaño;

    public ListaUniversal(int capacidad) {
        // Se instancia un array del tipo más alto en la jerarquía
        this.elementos = new Object[capacidad];
        this.tamaño = 0;
    }

    public void añadir(Object elemento) {
        if (tamaño < elementos.length) {
            elementos[tamaño] = elemento;
            tamaño++;
        }
    }

    public Object obtener(int indice) {
        return elementos[indice];
    }
}
```

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta
La **programación genérica** es un paradigma de diseño de software que persigue la creación de algoritmos y estructuras de datos independientes de los tipos de datos sobre los que operan. El objetivo principal es maximizar la reutilización del código, escribiendo una lógica unificada que se adapte y funcione de manera segura con distintos tipos sin necesidad de duplicar el código fuente para cada variante.

El ejemplo anterior, basado en `void*` o en la clase `Object`, representa efectivamente una forma primitiva y rudimentaria de programación genérica. A nivel funcional, cumple con el objetivo de reutilizar una misma estructura para almacenar cualquier entidad, logrando un comportamiento independiente del tipo específico del dato. A esta técnica se la suele denominar "genericidad polimórfica".

Sin embargo, en la ingeniería de software moderna, el concepto de programación genérica conlleva un requisito adicional indispensable: la seguridad de tipos (*type safety*). Aunque la técnica basada en `Object` es genérica en su almacenamiento, carece de controles estrictos durante la compilación, lo que la aleja del estándar actual de lo que se considera un diseño genérico robusto.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta
El inconveniente fundamental de emplear referencias universales como `void*` u `Object` es la pérdida total de la identidad del tipo durante la fase de compilación. Cuando se introduce un elemento en la estructura, el compilador "olvida" su tipo original. Por lo tanto, no existe ningún mecanismo automático que impida introducir accidentalmente datos lógicamente incompatibles en la misma colección (por ejemplo, mezclar objetos `Punto` y cadenas `String` en una lista pensada para cálculos matemáticos).

El segundo problema crítico surge al momento de extraer los datos. Dado que la estructura devuelve un tipo genérico, es estrictamente obligatorio que el programador realice un *downcasting* manual (una conversión forzada, como `(String) obj` o `(int*) ptr`) para recuperar el tipo original y poder operar con él. Este proceso traslada la responsabilidad de la verificación de tipos desde el compilador (una máquina infalible) hacia la memoria del desarrollador.

Si el programador se equivoca al recordar qué tipo de dato extrae y realiza un *casting* erróneo, el error no se detectará al compilar el código. En su lugar, provocará fallos catastróficos durante la ejecución del programa. En C, desreferenciar incorrectamente un `void*` desencadena corrupción de memoria o un error de segmentación (*segmentation fault*). En Java, la Máquina Virtual abortará el proceso lanzando una excepción incontrolada denominada `ClassCastException`.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta
Los parámetros de tipo son un mecanismo avanzado introducido en los lenguajes de programación para resolver los problemas de seguridad derivados del uso de identificadores genéricos. Actúan de forma conceptualmente idéntica a los parámetros convencionales de una función, pero en lugar de recibir valores de datos (como números o textos) durante la ejecución, reciben "tipos" de datos (como clases o interfaces) durante la fase de compilación o instanciación.

Se declaran utilizando marcadores de posición (habitualmente letras mayúsculas como `<T>`, `<E>`, o `<K>`) en la cabecera de la clase o interfaz. Dentro del código fuente de esa clase, el marcador de posición se emplea como si fuera un tipo de dato real y existente para declarar variables, parámetros de métodos o tipos de retorno, manteniendo la lógica genérica.

La verdadera fortaleza de este mecanismo es que, al momento de crear un objeto de dicha clase, se debe especificar explícitamente el tipo real que sustituirá al parámetro (por ejemplo, `<String>`). Esto faculta al compilador para realizar un chequeo de tipos exhaustivo, garantizando matemáticamente que solo se inserten objetos de ese tipo específico y eliminando por completo la necesidad de realizar *downcastings* manuales al extraerlos.

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta
Tanto Java como C++ proveen librerías estándar con colecciones dinámicas construidas sobre estos mecanismos. En Java, el sistema se denomina *Generics* y se aplica fuertemente sobre jerarquías de objetos. En C++, la técnica se llama *Templates* (plantillas) y posee un enfoque de generación de código muy potente.

En ambos lenguajes, la declaración exige introducir el tipo concreto entre símbolos de menor y mayor. Al recorrer la estructura, no es necesario aplicar conversiones explícitas; el compilador certifica que la variable de iteración es directamente un `String` (o `std::string` en C++) y permite invocar sus métodos particulares, como obtener la longitud del texto, con absoluta seguridad.

```java
// Ejemplo en Java utilizando Generics
import java.util.ArrayList;
import java.util.List;

public class GenericsJava {
    public static void main(String[] args) {
        // Se restringe la lista para que solo admita String
        List<String> lista = new ArrayList<>();
        lista.add("Hola");
        lista.add("Mundo");

        // Al recorrerla, se extrae un String directamente. Sin castings.
        for (String elemento : lista) {
            System.out.println("Longitud: " + elemento.length());
        }
    }
}
```

```cpp
// Ejemplo en C++ utilizando Templates
#include <iostream>
#include <vector>
#include <string>

int main() {
    // Se instancia la plantilla vector especificando std::string
    std::vector<std::string> lista;
    lista.push_back("Hola");
    lista.push_back("Mundo");

    // Al iterar, el tipo recuperado es fuertemente std::string
    for (const std::string& elemento : lista) {
        std::cout << "Longitud: " << elemento.length() << std::endl;
    }
    return 0;
}
```

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta
Los compiladores de Java y C++ procesan la programación genérica de formas radicalmente distintas. No realizan las mismas operaciones internas, lo que se traduce en diferencias de rendimiento y tamaño del código binario final.

En C++, el proceso se conoce como **"instanciación de plantillas"**. Cuando el compilador detecta el uso de una clase plantilla con un tipo concreto (por ejemplo, `vector<int>` y luego `vector<string>`), genera literalmente copias separadas del código fuente de toda la clase sustituyendo la letra `T` por el tipo exacto en cada copia. Esto resulta en múltiples clases independientes en el código máquina. Garantiza un rendimiento óptimo al no haber conversiones ocultas, pero incrementa significativamente el tamaño del ejecutable (fenómeno conocido como *code bloat*).

En Java, se emplea un mecanismo llamado **"type erasure"** (borrado de tipos). El compilador utiliza la información de los parámetros `<T>` exclusivamente para realizar chequeos de seguridad estrictos durante la compilación. Una vez validado el código, el compilador "borra" todos los parámetros de tipo y los sustituye internamente por la clase `Object` en el *bytecode* final, insertando instrucciones ocultas de *downcasting* donde sea necesario. Como resultado, en tiempo de ejecución solo existe una única versión compilada de la clase genérica, compartida por todas las instanciaciones, independientemente de sus tipos.

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta
La programación genérica no se limita a un único parámetro. Es posible declarar múltiples parámetros de tipo en una misma clase, separándolos mediante comas. Esta técnica es especialmente útil para diseñar estructuras que asocian o agrupan conjuntos de datos dispares manteniendo un estricto control de tipos para cada uno de forma individual.

La clase `Par<T, U>` representa una tupla genérica básica. Al momento de instanciarla o utilizarla como valor de retorno en una subrutina, se concretan ambos tipos independientemente. Esto soluciona un problema arquitectónico clásico: la imposibilidad nativa de devolver dos valores distintos desde una función, sin recurrir a variables globales, punteros (en C) o perder el control de tipos mediante arreglos de objetos genéricos.

```java
// Clase genérica con dos parámetros de tipo
class Par<T, U> {
    private final T primero;
    private final U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() { return primero; }
    public U getSegundo() { return segundo; }
}

public class Estadistica {
    // Función que utiliza el tipo Par para retornar dos valores fuertemente tipados
    public static Par<Double, Double> calcularMediaYDesviacion(double[] datos) {
        double media = 0.0;
        for (double d : datos) media += d;
        media /= datos.length;

        double varianza = 0.0;
        for (double d : datos) varianza += Math.pow(d - media, 2);
        double desviacion = Math.sqrt(varianza / datos.length);

        return new Par<>(media, desviacion);
    }

    public static void main(String[] args) {
        double[] muestra = {2.5, 4.0, 5.5, 3.8};
        
        // Uso seguro del Par. Los getters devuelven el tipo concreto (Double)
        Par<Double, Double> resultados = calcularMediaYDesviacion(muestra);
        System.out.println("Media: " + resultados.getPrimero());
        System.out.println("Desviación: " + resultados.getSegundo());
    }
}
```

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta
Los parámetros de tipo pueden ser limitados al ámbito de una única subrutina. Para ello, el marcador `<T>` se define en la firma del método, justo antes de especificar el tipo de retorno. Esta capacidad resulta idónea para programar funciones utilitarias o algoritmos que operan genéricamente, pero que no requieren que la clase entera que los aloja sea genérica.

Si la función `seleccionaUno` se definiera utilizando `Object`, el compilador carecería de información para vincular lógicamente los dos argumentos. Permitiría mezclar tipos dispares (por ejemplo, enviar un número y una cadena simultáneamente), y el método devolvería forzosamente un `Object`. En consecuencia, quien invoque la función estaría obligado a ejecutar un *downcasting* peligroso sobre el resultado para poder utilizarlo.

Al emplear un parámetro de tipo a nivel de método, se establecen restricciones inviolables. El compilador verifica en el acto de la llamada que los dos argumentos suministrados pertenezcan estrictamente al mismo linaje de tipos (o a un subtipo común). Adicionalmente, el tipo de retorno se acopla dinámicamente al tipo de los argumentos, entregando el objeto final ya procesado con su tipo original y extinguiendo cualquier necesidad de *downcasting*.

```java
import java.util.Random;

public class Utilidades {

    // Opción 1: Sin generics (Uso de Object)
    // - No fuerza que los argumentos sean del mismo tipo.
    // - Obliga a quien llama a hacer downcasting.
    public static Object seleccionaUnoObject(Object a, Object b) {
        return new Random().nextBoolean() ? a : b;
    }

    // Opción 2: Método Genérico
    // - Obliga a que 'a' y 'b' compartan la jerarquía T.
    // - Devuelve directamente el tipo T, sin castings.
    public static <T> T seleccionaUnoGenerico(T a, T b) {
        return new Random().nextBoolean() ? a : b;
    }

    public static void main(String[] args) {
        String texto1 = "Rojo";
        String texto2 = "Azul";

        // Con Object: Requiere conversión manual al extraer
        String ganadorObjeto = (String) seleccionaUnoObject(texto1, texto2);

        // Con Generics: Tipado estricto y retorno directo y seguro
        String ganadorGenerico = seleccionaUnoGenerico(texto1, texto2);
        
        // Error de compilación prevenido por Generics:
        // Integer fallo = seleccionaUnoGenerico("Hola", 5); // Incompatible
    }
}
```

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta
Sí, los lenguajes permiten establecer límites sobre los parámetros de tipo, una funcionalidad conocida como "tipos acotados" (*bounded type parameters*). En Java, esto se instruye mediante la directiva `<T ClaseBase extends>`. Esta restricción impone que cualquier tipo proporcionado como reemplazo de `<T>` deba ser obligatoriamente una subclase de la clase o interfaz especificada, posibilitando invocar de manera segura métodos de dicha clase base desde dentro de la estructura genérica.

Se pueden modelar dos soluciones para la clase `Punto`. La primera prescinde de la genericidad y declara directamente las coordenadas bajo la superclase abstracta de Java denominada `Number`. La segunda solución emplea un tipo acotado `<T Number extends>`, forzando a que las instancias fijen su contexto matemático en torno a un subtipo concreto de número (como `Integer` o `Double`).

Respecto al proceso de "type erasure" (borrado de tipos), cuando el compilador procesa la variante con generics acotados, no borra el parámetro `<T>` sustituyéndolo por `Object` como haría habitualmente. Dado que existe una cota superior definida explícitamente, el compilador utiliza dicha cota como barrera lógica. Por tanto, tras la compilación, el tipo final que reside internamente en el *bytecode* para los atributos es precisamente la clase `Number`.

```java
// Solución 1: Sin generics. Utilizando polimorfismo puro con Number.
class PuntoSinGenerics {
    private Number x, y;
    
    public PuntoSinGenerics(Number x, Number y) {
        this.x = x; this.y = y;
    }
    public Number getX() { return x; }
    public Number getY() { return y; }
    
    public double distanciaA(PuntoSinGenerics otro) {
        return Math.sqrt(Math.pow(this.x.doubleValue() - otro.x.doubleValue(), 2) + 
                         Math.pow(this.y.doubleValue() - otro.y.doubleValue(), 2));
    }
}

// Solución 2: Con generics acotados (Bounded Type Parameters).
class PuntoConGenerics<T extends Number> {
    private T x, y;

    public PuntoConGenerics(T x, T y) {
        this.x = x; this.y = y;
    }
    public T getX() { return x; }
    public T getY() { return y; }
    
    public double distanciaA(PuntoConGenerics<T> otro) {
        return Math.sqrt(Math.pow(this.x.doubleValue() - otro.x.doubleValue(), 2) + 
                         Math.pow(this.y.doubleValue() - otro.y.doubleValue(), 2));
    }
}
```

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta
El contraste entre ambas soluciones revela la superioridad arquitectónica del refuerzo genérico para imponer coherencia estructural. En la solución desprovista de generics (usando atributos `Number`), el sistema no impone ninguna restricción de homogeneidad interna. Permite configurar instancias híbridas o corruptas matemáticamente, insertando una coordenada `X` de tipo `Integer` y una coordenada `Y` de tipo `Double`. 

En la solución implementada con generics (`PuntoConGenerics<T Number extends>`), dado que ambas coordenadas se declaran bajo un único identificador universal `T`, se garantiza una homogeneidad inquebrantable. Al instanciar un `PuntoConGenerics<Integer>`, ambas coordenadas quedan herméticamente bloqueadas por el compilador para aceptar exclusivamente enteros reales.

Esta diferencia estructural se refleja directamente en los tipos de retorno de la función `getX()`. En la variante carente de generics, `getX()` retorna ineludiblemente una referencia amorfa de tipo `Number`. En consecuencia, es mandatario realizar un *casting* manual hacia el subtipo numérico exacto si se pretende utilizar funciones matemáticas específicas posteriores. Por el contrario, en la variante genérica, `getX()` devuelve el tipo concreto establecido (ej. `Integer`), permitiendo un flujo de datos directo, íntegro y libre de conversiones forzadas.

## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.

### Respuesta
El diseño original sufre de una vulnerabilidad arquitectónica severa: el contrato del polimorfismo exige que la función admita cualquier objeto compatible con la interfaz matriz `Punto`. Esto obliga a las clases específicas a implementar defensas internas (`instanceof` y lanzar excepciones de tiempo de ejecución) para evitar calcular, por ejemplo, la distancia entre un entorno tridimensional y uno bidimensional.

Para subsanar esta debilidad y transferir el esfuerzo lógico hacia el analizador estático en tiempo de compilación, se debe parametrizar la propia interfaz. Al definir la interfaz como `Punto<T>`, se altera el contrato base: la función declarará que solo interactuará contra otro objeto rigurosamente parametrizado con el mismo tipo `T`.

Al implementar esta interfaz evolucionada, la clase concreta se acopla a sí misma en el parámetro de tipo (técnica que emula el "Curiously Recurring Template Pattern" en arquitecturas orientadas a objetos). Así, `Punto2D` implementará `Punto<Punto2D>`, logrando que la sobreescritura requiera nativamente una referencia exacta a `Punto2D`, aboliendo por completo los *castings* manuales y erradicando toda comprobación condicional de tipos.

```java
// Se añade el parámetro de tipo a la interfaz
public interface Punto<T extends Punto<T>> { 
    public double distanciaA(T p); 
} 

// La clase concreta se inyecta a sí misma como parámetro (Self-referencing generic)
public class Punto2D implements Punto<Punto2D> { 
    private final double x, y; 
    
    public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    // La sobreescritura ahora está fuertemente tipada. Se elimina el Object/Punto genérico.
    @Override 
    public double distanciaA(Punto2D p2d) { 
        // No hay necesidad de 'instanceof' ni downcastings. El compilador lo garantiza.
        return Math.sqrt(Math.pow(x - p2d.x, 2) + Math.pow(y - p2d.y, 2)); 
    } 
} 

public class Punto3D implements Punto<Punto3D> { 
    private final double x, y, z;
    
    public Punto3D(double x, double y, double z) {
        this.x = x; this.y = y; this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p3d) {
        return Math.sqrt(Math.pow(x - p3d.x, 2) + Math.pow(y - p3d.y, 2) + Math.pow(z - p3d.z, 2));
    }
} 
```

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta
Aunque un `String` hereda ineludiblemente de `Object`, la colección `List<String>` **no** ostenta una relación de subtipo con `List<Object>`. Esta vinculación se deniega terminantemente durante la fase de análisis del compilador para proteger la consistencia de los datos almacenados. En diametral contraste, y por decisiones originarias del diseño del lenguaje Java pre-generics, los arrays estáticos primitivos sí establecen que `String[]` es un subtipo válido de `Object[]`.

Esta disparidad regulatoria desencadena un defecto colosal en tiempo de ejecución al manipular los arrays. Si se asigna un vector real de cadenas (`String[]`) sobre una referencia superior de objetos (`Object[] ptr`), el compilador autorizará posteriormente instruir la inserción de un número entero en dicha referencia (`ptr[0] = 5;`), al estimarlo coherente con la naturaleza `Object`. El error detonará letalmente bajo la forma de una interrupción `ArrayStoreException` durante el procesado dinámico de la operación en memoria. Para abortar este peligro intrínseco, la programación genérica reestructuró sus cimientos imponiendo rigidez absoluta y cortando el paso a esta anomalía en tiempo de compilación.

Estas interacciones definen los perfiles de la varianza estructural de tipos complejos respecto a los subtipos subyacentes. Si el envoltorio superior preserva íntegramente la relación genealógica de sus componentes (como hacen los arrays), el diseño es catalogado como **covariante**. Si el comportamiento revierte diametralmente el sentido de la herencia (la colección base se estima subtipo de la derivada), es un sistema **contravariante**. Si, como acontece en las arquitecturas formales de *Generics*, el lenguaje corta toda atadura jerárquica dictaminando que conjuntos de derivaciones diferentes resultan inconmensurables e independientes, la concepción adoptada es **invariante**.

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta
Un *wildcard* o comodín, expresado mediante el operador topológico `?`, es una herramienta sintáctica proyectada para reincorporar la flexibilidad relacional sin menoscabar el blindaje compilatorio. Simboliza un "tipo genérico desconocido", cediendo las riendas a las directrices paramétricas de las funciones para que admitan rangos genealógicos más extensos en lugar de exigencias de tipo únicas e inamovibles.

La expresión paramétrica `List<? extends T>` perfila una demarcación limítrofe superior (covarianza mitigada). Su uso certifica que el conglomerado de elementos conforma estrictamente una derivación descendente de la superclase `T`. Resulta imperativo aplicarlo de modo exclusivo cuando el método opera como un ente **productor** (únicamente efectúa extracciones de lectura sobre los datos). En diametral oposición, `List<? super T>` delinea un horizonte jerárquico inferior (contravarianza mitigada), dictando que los elementos agrupan clases predecesoras a la estructura `T`. Este patrón rige cuando el método desempeña el rol de ente **consumidor** (realiza volcado y almacenamiento de datos, cerciorándose de que el contenedor provee espacio taxonómico holgado y seguro).

```java
import java.util.List;
import java.util.ArrayList;

public class OperacionesWildcards {

    // (i) COVARIANZA (Productor - Sólo lectura)
    // Acepta List<Number>, List<Integer>, List<Double>...
    // Se extrae la información para operar sin riesgo de alterar el contenedor original.
    public static double calcularSuma(List<? extends Number> listaNumeros) {
        double totalAcumulado = 0.0;
        // Se asegura que todo lo extraíble se comparta como Number
        for (Number num : listaNumeros) {
            totalAcumulado += num.doubleValue();
        }
        return totalAcumulado;
        // Intento bloqueado por el compilador: listaNumeros.add(5);
    }

    // (ii) CONTRAVARIANZA (Consumidor - Sólo escritura)
    // Acepta List<Integer>, List<Number>, List<Object>...
    // El sistema se cerciora de que la lista receptora admita inserciones de Integer seguro.
    public static void rellenarConEnteros(List<? super Integer> listaDestino) {
        listaDestino.add(10);
        listaDestino.add(20);
        listaDestino.add(30);
        // Operación bloqueada: No se puede hacer lectura asumiendo el tipo resultante Integer.
    }

    public static void main(String[] args) {
        List<Integer> coleccionEnteros = new ArrayList<>();
        
        // El mismo objeto actúa de forma segura en ambos contextos acotados
        rellenarConEnteros(coleccionEnteros); 
        System.out.println("Suma total: " + calcularSuma(coleccionEnteros));
    }
}
```</Object></String></Object></String></Punto2D></T></Integer></T></T></T></T></T></T></T></T,></T></String></K></E></T>