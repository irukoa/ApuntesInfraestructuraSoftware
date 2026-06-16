---
layout: default
title: La distribución de bibliotecas más allá de la interfáz binaria
---

# La distribución de bibliotecas más allá de la interfáz binaria

En este artículo tratamos un tema de importancia critica en el diseño de la infraestructura de software: la distribución de bibliotecas. Asumiremos de forma general que no hay ningún tipo de garantía con respecto a la interfáz binaria (o ABI, por sus siglas en inglés).

Comenzamos con un hecho: el modelo de distribución binaria de bibliotecas quiebra una vez que su cimiento fundamental, el contrato ABI estable, desaparece. En entornos donde desarrollamos bibliotecas en C o C++, tal y como hemos mencionado en [otros artículos](https://irukoa.github.io/ApuntesInfraestructuraSoftware/Articulos/ElEnlazador.html), podemos, por lo general, emplear el cómodo modelo de la distribución binaria de bibliotecas sin mayores problemas. En otros lenguajes, como por ejemplo en Fortran, surgen ciertos problemas de ingeniería que trataremos en este artículo. Al desaparecer el contrato ABI, el problema de la distribución cambia de naturaleza. La cuestión ya no consiste únicamente en compilar una biblioteca, sino en determinar cómo preservar su capacidad de integración en entornos heterogéneos. En estos contextos, el código fuente y el sistema de construcción recuperan un papel central como mecanismos para preservar la portabilidad y facilitar la integración del software.

## El modelo de distribución de código fuente

En casos donde no hay un contrato ABI lo suficientemente estable, la distribución se complica: para emplear el modelo de distribución binaria debemos distribuir un conjunto de imágenes binarias, no solo compiladas en sistemas operativos diferentes, si no también compilados usando diferentes compiladores y opciones. Esta problemática no solo afecta al desarrollador de la biblioteca, que lidia con esta explosión combinatoria de configuraciones, si no también al consumidor. El consumidor emplea la biblioteca, bien instalandola a nivel global en su sistema, o bien instalandola a nivel local de proyecto. En cualquiera de estos casos, instalar una biblioteca equivale, en cierto sentido, a imponer una parte del entorno de construcción original sobre los consumidores. Debe homogenizarse la configuración de compilación de la biblioteca con respecto a la compilación del proyecto.

Frente a este modelo está el modelo de distribución de código fuente. Este modelo otorga mas control y mas responsabilidad al consumidor. Su procedimiento es sencillo: el desarrollador distribuye no solo el código fuente, si no el sistema de compilación y construcción. Esto, tal y como hemos apuntado puede hacerse con [Make](https://irukoa.github.io/ApuntesInfraestructuraSoftware/Articulos/MakeUnLenguajeIncomprendido.html), pero también con cualquier otro sistema. En este contexto, el desarrollador otorga al consumidor las herramientas de construcción suficientes, delegando al consumidor aquellas decisiones dependientes de la integración, como la elección del compilador, las opciones de compilación o el destino de instalación. Un equilibrio especialmente satisfactorio es el siguiente: el desarrollador dispone de la libertad de escoger el perfil de compilación (deshabilitar pruebas y demás) mientras que el consumidor escoge compilador, opciones y tipo de instalación (bien global o bien local por proyecto). Como observamos, este modelo requiere mas disciplina, pero distribuye de forma más equilibrada la responsabilidad de compilación  entre el desarrollador y el consumidor. Lo que este modelo pierde en inmediatez, lo gana en flexibilidad y capacidad de adaptación.

En este sentido, el sistema de construcción forma parte de la interfaz de distribución de la biblioteca: constituye la especificación ejecutable del proceso mediante el cual dicha biblioteca puede adaptarse a distintos entornos de compilación. El éxito de este modelo depende, por tanto, de varios factores: tales como el aislamiento entre la interfáz de la biblioteca y su implementación, del diseño de la herramienta adecuada para la construcción y su capacidad de integración en otros sistemas de construcción y de la disciplina y la sinergia que exista entre desarrollador y consumidor.

### Un consejo personal

Yo empleo este sistema de distribución con gran asiduidad. Mi experiencia personal me ha llevado a favorecer la integración sencilla sobre la sofisticación excesiva del sistema de construcción. Cuanto menor sea el esfuerzo necesario para incorporar la biblioteca a un proyecto existente, menor será la fricción a la hora de la adopción, así como el coste de mantenimiento para el consumidor. Conviene que el proceso de integración sea lo suficientemente sencillo como para que el consumidor pueda incorporar la biblioteca mediante unas pocas reglas de Make o mecanismos equivalentes. Yo suelo hacer esto mediante la distribución de una secuencia de comandos que obtiene el código fuente e invoca al sistema de construcción. Ajusto la mayoría de las opciones de forma que queden predefinidas (versión del código, perfil de producción y otras opciones parte de la jurisdicción exclusiva de la implementación), mientras que delego al usuario las opciones relativas a la interfaz publica, destino de instalación y opciones de compilación. Esto le garantiza al usuario una integración facil en su proyecto.

---

En este modelo, el código fuente deja de desempeñar exclusivamente el papel de implementación del software. Se convierte también en un medio para preservar la portabilidad de la biblioteca y permitir su adaptación a distintos entornos de construcción. Podríamos condensar lo dicho hasta ahora así: el modelo binario desplaza la complejidad hacia el desarrollador, mientras que el modelo basado en código fuente redistribuye esa complejidad entre productor y consumidor. Ahora trataremos el caso de Fortran como ejemplo canónico de este modelo de distribución.

## El caso de Fortran

POR REDACTAR
