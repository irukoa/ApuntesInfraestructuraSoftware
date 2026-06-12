---
layout: default
title: Programación orientada a pruebas
---

# Programación orientada a pruebas

La programación orientada a pruebas consiste en diseñar el software de forma simultánea a una infraestructura que permita verificar su comportamiento. Esta infraestructura se materializa en forma de pruebas: ejecuciones controladas del código destinadas a comprobar la adecuada conservación de varias propiedades del software. Las ventajas de adoptar esta sistemática son numerosas. Obliga a definir interfaces cuidadas, proporciona un mecanismo para detectar regresiones de manera temprana y dota al proyecto de una referencia ejecutable acerca del comportamiento esperado del software. A menudo, esta colección de pruebas constituye además una valiosa fuente de documentación para nuevos desarrolladores, al mostrar ejemplos concretos de utilización de los distintos componentes del sistema.

Las pruebas pueden clasificarse de múltiples maneras. La literatura especializada distingue entre pruebas unitarias, de integración o funcionales; entre pruebas de caja blanca y caja negra; entre pruebas aisladas o dependientes del entorno de ejecución. Muchos de estos criterios son ortogonales entre sí y responden a necesidades distintas. Por este motivo, el propósito de este artículo no consiste en profundizar en una taxonomía concreta de pruebas, sino en retroceder un paso en el nivel de abstracción y reflexionar sobre la propia infraestructura que las hace posibles.

Con frecuencia, la discusión en torno a la programación orientada a pruebas se centra en qué herramientas emplear. Existen marcos ampliamente adoptados, como [GoogleTest](https://github.com/google/googletest), que proporcionan soluciones maduras a problemas recurrentes. Sin embargo, no siempre resulta evidente que la incorporación de una infraestructura externa constituya la opción más adecuada para un proyecto determinado. Del mismo modo, desarrollar una infraestructura propia tampoco es una decisión exenta de costes.

En este artículo examinaremos distintas estrategias para implementar una infraestructura de pruebas, prestando especial atención a los compromisos que cada una de ellas implica. Nuestro objetivo no es defender una solución universal, sino proporcionar al lector los elementos necesarios para evaluar qué enfoque se ajusta mejor a las características y necesidades de su propio software. Este artículo puede leerse como una descripcion de técnicas y usos en la implementación de esta metodología.

## Diseño de infraestructura

Una prueba consta de tres secuencias. Primero, diseñamos un escenario. Esto aporta contexto a la prueba. Seguidamente, la prueba ejecuta cierta funcionalidad del proyecto. Finalmente, usando aserciones, verificamos el comportamiento esperado y restauramos el entorno a su estado original. Desde esta perspectiva, los requisitos fundamentales de una infraestructura de pruebas son relativamente modestos.
- Permitir el diseño de escenarios.
- Habilitar un registro de pruebas, ya sea manual o automático.
- Proveer un ejecutor de pruebas.
- Diseñar un sistema de aserciones.
- Emitir informes: al menos, debe de identificar las pruebas fallidas.

## Infraestructura propia o externa

Un marco externo aporta documentación y convenciones consolidadas, además de externalizar el mantenimiento de la infraestructura. Por otro lado, introduce costes, tales como la integración en el proyecto y posibles disonancias con respecto al lenguaje de programación. En esencia, los marcos de pruebas consolidados resuelven problemas reales, pero también introducen dependencias, convenciones y costes de integración que no siempre resultan proporcionales a las necesidades del proyecto.

En consecuencia, la elección de una infraestructura de pruebas no debe responder únicamente a criterios de disponibilidad. Debe tener en cuenta las características del proyecto, los lenguajes implicados y el coste real de integración y mantenimiento.

En proyectos extensos escritos mayoritariamente en C++, marcos como GoogleTest ofrecen ventajas evidentes: proporcionan un lenguaje de aserciones expresivo, mecanismos avanzados de parametrización y una amplia colección de utilidades auxiliares. En estos contextos, los beneficios suelen compensar sobradamente la dependencia introducida. Sin embargo, esta situación no es universal. En proyectos escritos principalmente en C, la incorporación de una infraestructura fuertemente ligada a C++ puede introducir una cierta disonancia técnica y conceptual. Del mismo modo, en proyectos desarrollados en Fortran u otros lenguajes menos representados en esta clase de marcos, la integración puede exigir adaptaciones adicionales y aumentar la complejidad del mantenimiento.

La cuestión relevante no consiste en determinar qué infraestructura es objetivamente superior, sino en identificar cuál resulta proporcionada a las necesidades reales del proyecto. Debe emplearse la herramienta adecuada. Cuando ninguna de las alternativas disponibles se ajusta razonablemente a los requisitos del proyecto, el desarrollo de una infraestructura propia constituye también una opción legítima.

## Infraestructura mínima en C

En esta sección expondremos brevemente una pequeña implementación de marco de pruebas escrito en C. Elegimos este lenguaje porque las interfaces binarias generadas por compiladores de C constituyen un punto de interoperabilidad común: numerosos lenguajes proporcionan mecanismos para invocar funciones escritas en C o enlazar bibliotecas generadas por compiladores de C, lo que facilita reutilizar la infraestructura de pruebas desde distintos entornos. C dispone además de una facil integración con Make, el minimo común denominador entre los sistemas de construcción. Finalmente, C es un lenguaje orientado a la programación de sistemas, lo cual aporta muchas utilidades auxiliares en la implementación.

El marco de pruebas que yo desarrollé se llama TSD. La implementación completa está disponible [aquí](https://github.com/irukoa/ApuntesInfraestructuraSoftware/tree/main/TSD). Ahora enunciamos las abstracciones fundamentales. Representamos una prueba así:

```C
typedef void (*TSD_FncProto)(void *Data);

typedef struct _TSD_Context {
  const TSD_FncProto SetUp;
  const TSD_FncProto TearDown;
  const size_t ContextSize;
} TSD_Context;

typedef struct _TSD_Test {
  const char *const  Name;
  const TSD_FncProto Procedure;
  const TSD_Context *Ctx; // Opcional.
} TSD_Test;
```

En este contexto, una prueba no es más que una función acompañada de cierta información descriptiva y, opcionalmente, de un contexto de ejecución. Las pruebas pueden agruparse en un conjunto de pruebas:

```C
typedef struct _TSD_TestSuite {
  const char *const Name;
  const TSD_Test   *Tests; // Matriz de pruebas.
  const size_t      Count;
} TSD_TestSuite;
```

Desde el punto de vista de la implementación, solamente falta mencionar las funciones ejecutoras:

```C
void TSD_RunTest(const TSD_Test *TestInstance,
                 size_t         *NFailedTests);
void TSD_RunSuite(const TSD_TestSuite *Suite,
                  size_t              *NFailedTests);
void TSD_RunAll(const TSD_TestSuite *const Suites[],
                const size_t               SuitesCount,
                size_t                    *NFailedTests);
```

Desde el punto de vista del uso, conviene definir macros, tales como:

```C
#define TEST(NAME)                                                  \
  static void TEST_##NAME(void);                                    \
  static void TEST_##NAME##_Wrapper(void *ContextData) {            \
    (void)ContextData;                                              \
    TEST_##NAME();                                                  \
  }                                                                 \
  static const TSD_Test NAME = {.Name      = #NAME,                 \
                                .Procedure = TEST_##NAME##_Wrapper, \
                                .Ctx       = NULL};                 \
  static void           TEST_##NAME(void)
```

Algunas implementaciones automatizan el registro de pruebas mediante extensiones específicas del compilador. Sin embargo, el registro manual suele resultar suficiente y presenta una portabilidad considerablemente mayor. TSD emplea una simple variable global

```C
static const TSD_TestSuite **Suites; // Matriz de punteros.
```

Finalmente, la integración con Make resulta natural. Basta con compilar la implementación del marco de pruebas junto con la implementación de las pruebas y los archivos objeto del proyecto. Así generamos un ejecutable específico. Desde el punto de vista del sistema de construcción, las pruebas constituyen simplemente otro artefacto del software.

El objetivo de esta implementación no consiste en sustituir infraestructuras más sofisticadas. Pretende únicamente mostrar que los mecanismos fundamentales sobre los que se construyen estos sistemas son relativamente sencillos. Comprenderlos facilita evaluar con criterio cuándo conviene adoptar una solución existente y cuándo una infraestructura mínima puede resultar suficiente.

## Aislamiento funcional

POR REDACTAR

## Conclusión
