---
layout: default
title: Make, un lenguaje incomprendido
---

# Make, un lenguaje incomprendido

[Make](https://www.gnu.org/software/make/) es un lenguaje para describir relaciones de dependencia y las acciones necesarias para satisfacerlas. Aunque suele asociarse con la compilación de programas, su ámbito de aplicación es considerablemente más amplio. Durante décadas ha formado parte de la infraestructura habitual de los sistemas tipo Unix y continúa presente, por defecto, en una gran cantidad de entornos de desarrollo.

Sin embargo, Make goza de una reputación desigual. Para algunos desarrolladores constituye una herramienta sobria, fiable y extraordinariamente flexible. Para otros, representa una reliquia obsoleta: un lenguaje envejecido cuyos archivos de configuración, los denominados *Makefiles*, resultan difíciles de comprender y costosos de mantener.

Estas críticas no son enteramente infundadas. No es difícil encontrar proyectos cuyos *Makefiles* han crecido sin disciplina alguna hasta convertirse en un entramado opaco y frágil. Tampoco puede ignorarse que el desarrollo de software contemporáneo plantea exigencias que Make no siempre satisface con comodidad, especialmente en contextos marcadamente multiplataforma o cuando resulta necesario integrar un número elevado de herramientas heterogéneas.

En respuesta a estas dificultades han surgido, a lo largo del tiempo, diversos sistemas de construcción, tales como CMake, Meson o Bazel. Estas herramientas introducen distintos niveles de abstracción sobre el proceso de compilación con el propósito de simplificar tareas frecuentes, mejorar la portabilidad o proporcionar mecanismos adicionales de organización.

Nada de esto convierte a Make en una herramienta obsoleta.

---

Con cierta frecuencia, la discusión en torno a Make adopta la forma de una falsa dicotomía: o bien se considera un vestigio histórico que conviene evitar siempre que sea posible, o bien se le atribuyen virtudes casi universales. Ninguna de estas posiciones resulta particularmente útil. Como ocurre con tantas otras herramientas de programación, la cuestión relevante no consiste en determinar si Make es superior o inferior a sus alternativas, sino en comprender cuáles son sus capacidades, cuáles son sus limitaciones y en qué circunstancias constituye una elección razonable.

Este artículo articula una idea más modesta: Make es un sistema perfectamente válido para una amplia variedad de proyectos, siempre que se emplee con disciplina y se comprendan adecuadamente los principios sobre los que se sustenta. De hecho, el conocimiento de dichos principios proporciona competencias que trascienden al propio Make. La comprensión explícita de las dependencias entre componentes, la reconstrucción incremental de artefactos, la composición de herramientas sencillas o la automatización reproducible de tareas son conceptos que conservan su relevancia independientemente del sistema de construcción finalmente adoptado.

Por otra parte, muchas de las críticas dirigidas hacia Make se refieren, en realidad, a determinados estilos de escritura de *Makefiles* más que al lenguaje en sí mismo. Un *Makefile* desorganizado puede resultar tan dañino como cualquier otro artefacto software mal diseñado. Del mismo modo, un uso disciplinado de Make puede dar lugar a sistemas de construcción sorprendentemente robustos, expresivos y duraderos.

No pretendemos argumentar que Make sea la solución adecuada para cualquier problema ni que los sistemas alternativos carezcan de justificación técnica. Existen escenarios en los que herramientas de mayor nivel ofrecen ventajas evidentes. Tampoco defenderemos que todo proyecto deba renunciar a ellas en favor de un retorno a planteamientos más tradicionales.

El propósito de este artículo es otro. Trataremos de exponer una interpretación de Make que permita comprender qué problemas resuelve realmente, cuáles son los mecanismos que pone a disposición del programador y por qué continúa ocupando un lugar destacado dentro de la infraestructura del software contemporáneo. A partir de esa exposición, corresponderá al lector juzgar si Make se adecua a sus necesidades o si, por el contrario, su proceso de construcción requiere una capa adicional de abstracción.

Las afirmaciones anteriores resultarían poco convincentes si permaneciesen en el terreno de la abstracción. Por este motivo, el resto del artículo se articula en torno a un ejemplo concreto: el diseño progresivo de un sistema de construcción basado en Make. Nuestro objetivo no es presentar una receta universal, sino ilustrar una serie de principios que, en nuestra opinión, permiten emplear Make de forma sostenible.

## Diseño de un *Makefile* modular

## Extractores de dependencias

### El caso de C

### El caso de Fortran

## Integración de artefactos de terceros

## Perfiles

### Depuración y pruebas

### Producción y distribución

## Conclusión

Make obliga al programador a hacer explícitas ciertas relaciones que otras herramientas tienden a ocultar. Esta explicitud tiene un coste: exige disciplina y comprensión del proceso de construcción. Sin embargo, también proporciona una ventaja significativa. El sistema de compilación deja de ser una caja negra y pasa a convertirse en una característica más del software, susceptible de ser razonada, revisada y mantenida con el mismo cuidado que el resto del código.
