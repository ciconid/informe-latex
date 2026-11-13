---
name: informe-latex
description: Redactar informes y documentos en PDF con LaTeX (preámbulo propio con cajas, íconos TikZ y babel español), compilarlos con latexmk y revisarlos visualmente. Usar cuando el usuario pida un informe, documento, resumen, apunte o resolución en PDF/LaTeX.
---

# Informes y documentos en PDF con LaTeX

Archivos de esta skill (en el directorio de la skill, `~/.claude/skills/informe-latex/`):
- `preambulo.tex`: preámbulo común (paquetes, colores, cajas, citas, íconos TikZ).
- `ejemplo.tex`: documento mínimo que usa todo lo del preámbulo. Tomarlo como modelo.

## Preparar el proyecto

- Si el proyecto ya tiene `plantilla/preambulo.tex`, usar ese (puede tener macros propios del proyecto; no pisarlo).
- Si no, copiar `preambulo.tex` de la skill a `plantilla/preambulo.tex` del proyecto. Así el documento compila solo, sin depender de la skill. Si hacen falta macros propios (abreviaturas de fuentes, cajas nuevas), agregarlos a esa copia, no a la de la skill.
- El documento hace `\input{<ruta relativa>/plantilla/preambulo}` justo después de `\documentclass[11pt,a4paper]{article}`.

## Estructura del documento

- `\maketitle`, después `\notafuentes{...}` (si hay fuentes con abreviaturas) y después `\indice` (índice con hipervínculos). El índice se omite solo en documentos de una o dos páginas.
- `\section` / `\subsection` **sin asterisco**, para que entren al índice.
- Opciones en el preámbulo del documento:
  - `\sinnumeros`: secciones sin número visible (para documentos de tipo ejercicio/pregunta).
  - `\seccionesenpaginanueva`: cada `\section` empieza en página nueva.
- Cajas:
  - `recuadro{Título}` (gris): consigna, requisitos, datos de entrada.
  - `destacado{Título}` (azul): conclusión, resumen ejecutivo, definición clave.
  - `aviso{Título}` (ámbar): supuestos, ambigüedades, datos faltantes, erratas.
  - `extras` o `extras[TÍTULO]` (franja azul): ampliación opcional. Usarla para lo que profundiza sin ser necesario.

## Reglas de redacción

- **Contenido principal**: lo mínimo y suficiente para responder o cumplir el objetivo, sin verbosidad innecesaria.
- **Ampliación** (caja `extras`): lo que profundiza sin hacer falta.
- **No repetir información.** `extras` solo agrega información **nueva**. Si algo ya está arriba (en otro idioma, con otras palabras o como cita), no va; a lo sumo se suma la otra fuente a la nota al pie existente. Antes de compilar, revisar cada viñeta de `extras` contra el cuerpo y contra las demás viñetas.
- **Viñetas antes que prosa**, siempre que se pueda.
- **Tablas y pictogramas siempre que se pueda** (comparaciones, clasificaciones, ciclos, procesos, arquitecturas). Van **además** del texto, aunque muestren lo mismo, y no agregan información que no esté en el texto. Dibujar con TikZ; hay íconos en el preámbulo: `persona`, `compu`, `robot`, `documento`, `sobre`, `arbol` (`\pic[scale=0.8] at (x,y) {robot};`).
- **Figuras donde se usan, aunque se repitan**: si una sección necesita una figura que ya apareció, se vuelve a poner completa en vez de remitir a ella. Esto vale solo para figuras; el texto no se repite (ahí sí se remite).

## Fuentes y citas

- Citar siempre las fuentes, **solo en notas al pie**, nunca en el cuerpo: `\fuente{Autor, \textit{Título}, §2.1, p.~34}`. Varias fuentes en la misma nota, separadas con `;`.
- Si varias fuentes dicen lo mismo, citar el contenido de una sola y agregar las otras en la misma nota ("Misma idea en ...").
- Si varias viñetas seguidas salen de la misma fuente, poner una sola nota en la viñeta que las agrupa.
- Citas textuales en otro idioma con `\cita{...}`, **sin traducir**.
- Para fuentes de uso frecuente, definir macros cortos en la copia del preámbulo del proyecto (p. ej. `\newcommand{\aima}[2]{\footnote{AIMA, §#1, p.~#2.}}`) y explicar las abreviaturas en `\notafuentes{...}`.
- Para leer PDFs fuente: `pdftotext -layout archivo.pdf -` o `pdftotext -f N -l M` para un rango de páginas (no extraer libros completos), con la salida en el scratchpad, nunca dentro del repo. Si una página tiene diagramas o tablas relevantes, o la fuente sale mal codificada, leerla como imagen (`pdftoppm -r 110 -png -f N -l N`).

## Compilar y revisar

1. En el directorio del `.tex`: `latexmk -pdf -interaction=nonstopmode doc.tex`.
2. Corregir los errores y los warnings relevantes (overfull hbox visibles, referencias indefinidas, glifos faltantes).
3. Revisar el PDF a ojo: `pdftoppm -r 70 -png doc.pdf <scratchpad>/pag` y mirar las páginas (figuras que se desbordan, tablas cortadas, notas al pie superpuestas, cajas partidas mal).
4. Limpiar: `latexmk -c doc.tex` (dejar solo `.tex` y `.pdf`).

## Trampas de babel español

- El preámbulo ya trae `es-noshorthands` y `\usetikzlibrary{babel}`; no sacarlos (sin ellos se rompen TikZ y algunos símbolos).
- En matemática: `$35\%$`, no `$35\,\%$`. Decimales con coma: `0{,}65`.
- Las notas al pie dentro de una `tcolorbox` que se parte entre páginas se superponen con el contenido. Por eso `extras` no es una tcolorbox. Si una caja con notas al pie se parte, pasar las notas fuera de la caja o usar `extras`.

## Reporte al terminar

- No resumir lo que se hizo. Reportar solo lo que salió mal: errores, datos faltantes, supuestos tomados, cosas que no se pudieron verificar.
- Si no hubo ningún problema, una sola línea avisando que terminó con éxito y dónde está el PDF.
