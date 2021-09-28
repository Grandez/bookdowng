# BOOKDOWNG
A workflow/tools to automate writing proffessional books

## Motivation

Bookdown y RMarkdown son unas potentes herramientas para escribir articulos en diferentes formatos, pero presenta limitaciones importantes cuando queremos escribir libros o manuales complejos o mas profesionales; por ejemplo, consideremos el apartado de convenciones y tipografias de un libro técnico:

Tipicamente, un libro deberia contener un apartado en el que se indicara explicitamente la tipografía usada:

1) SMALL-CAPS: statement type
2) typewriter: data fields
3) sans-serif: variables
4) italic: functions
_Tomado de The Grammar of Graphics_

Usar esto de manera consistente en bookdown es complejo, y sin embargo es extremadamente simple en procesadores de texto (Word, OpenOffice, etc.). Tambien es complejo el manejo de secciones, cabeceras, pies, etc. en función del tipo de documento a generar.

## Pandoc and Officeverse

**Pandoc** se define como una herramienta para convertir documentos entre diferentes formatos y **Officeverse** es un conjunto de paquetes que facilitan el uso de funciones avanzadas de los procesadores de texto; por ejemplo, uso de estilos personalizados, tablas de textos **no** de datos, etc. en desarrollo activo pero que actualmente no esta muy integrado, o al menos no de forma muy natural, en los documentos de Bookdown.

Por ejemplo:

- Usando Pandoc podemos definir estilos personalizados que será tratados como estilos en un documento Word y como tags <div> y <span> en HTML usando _custom_styles_: Con corchetes ([texto]{custom-style="style"} o bloques "::: [{custom-style="style"] ... :::
  - Usando Officedown pordemos usar el conjunto de codigos: <!-- BLOCK_xxxx -->. En este caso notese que lo que se esta haciendo en incluir una "instruccion" dentro de un comentario HTML

## Objetive
  
La idea principal es que, si usando Officeverse y Pandoc podemos hacer uso de las funciones avanzadas de los procesadores de texto, manteniendo una unica fuente comun podemos generar documentos en diferentes formatos que tenga una aspecto "profesional" y coherente entre los diferentes formatos aprovechando un paso intermedio (o varios) usando Word como plataforma intermedia y aprovechando toda su funcionalidad sin tener que utilizar "_hacks_" para conseguir los obejtivos deseados.
  
 Actualmente, la idea basica es:
  
  1. Implementar un nuevo conjunto de marcas independientes del formato y faciles de usar
  2. Preprocesar los documentos para adecuarlos a cada uno de los formatos en funcion de esas marcas
  3. Generar los documentos apropiados con Pandoc, incluyendo Word
  4. Usar el documento Word generado para generar el resto de documentos
  
  ### Example
  
  Supongamos que tenemos un lenguaje **G** conocido por knitr
  Supongamos que para crear una seccion en Word, que no en HTML, se usan los "tags" de Officedown:
  <!-- BLOCK_SECTION_START -->
  ....
  text
  ...
  <!-- BLOCK_SECTION_END -->
  
  Podriamos escribir un documento sample.Gmd:
  
  ...
  Esto es texto que tiene una tipografia inline `g nombre_de_funcion` y mas lineas
  A partir de aqui se crea otra seccion:
  
  ```{g section type=x, ....}```
  ....
  Aqui el texto de la seccion
  ....
  ```
  Para una salida **HTML** es preprocesado a _sample_html.Rmd_:
  
    ...
  Esto es texto que tiene una tipografia inline [nombre_de_funcion]{custom_style="function_class"} y mas lineas
  A partir de aqui se crea otra seccion:
  
  <!-- En HTML no hay secciones -->
  <div class="....">
  ....
  Aqui el texto de la seccion
  ....
       </div>
  
  Para una salida **DOCX** es preprocesado a _sample_docx.Rmd_:
  
    ...
  Esto es texto que tiene una tipografia inline [nombre_de_funcion]{custom_style="function_class"} y mas lineas
  A partir de aqui se crea otra seccion:
  
  <!-- En HTML no hay secciones -->
  <!-- BLOCK_SECTION_START -->
  ....
  Aqui el texto de la seccion
  ....
<!-- BLOCK_SECTION_END -->
  
