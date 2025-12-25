---
title: Crea tu currículum con LaTeX
date: 2025-12-24T10:29:05+01:00
comments: true
showToc: true
draft: false
tags: [LaTeX]
cover:
    image: /posts/cv/cover.png
---

Muchas veces, cuando me pongo a actualizar mi currículum, acabo pasando más tiempo peleándome con el formato que haciendo la pequeña modificación que quería. Frustrado por esto, pensé que debería haber una forma más rápida de gestionar este proceso. ¿La solución? Separar el contenido del formato. Esto significa tener un archivo que contenga la información personal y otro archivo que defina su estilo. Este problema se resuelve elegantemente con LaTeX, una herramienta muy utilizada en el mundo académico para la preparación de documentos.

En este artículo, utilizaremos LaTeX y Pandoc para ofrecer una forma sencilla de generar un CV profesional a partir de datos. De esta manera, el contenido del CV se almacena en un formato estructurado (YAML) y el diseño se gestiona mediante una plantilla de LaTeX. Cada vez que necesites actualizar tu CV, solo tendrás que modificar el archivo YAML y regenerar el PDF.

## El flujo de trabajo

La arquitectura es sencilla. Pandoc lee tus datos de un archivo YAML y utiliza una plantilla de LaTeX para darle formato en un PDF:

**YAML** *(Los Datos)* + **LaTeX** *(La Plantilla)* → **Pandoc** *(El Motor)* → **PDF** *(El Resultado)*

Aquí te explico cómo configurarlo paso a paso.

## 1. Requisitos previos

Necesitarás tener instaladas dos herramientas principales en tu sistema:
- **Pandoc**, el "convertidor universal de documentos". Puedes leer las instrucciones de instalación [aquí](https://pandoc.org/installing.html).
- **Distribución TeX** que proporciona el motor para renderizar PDFs. Las opciones recomendadas son:
  - Windows: [MiKTeX](https://miktex.org/download)
  - MacOS: [MacTeX](https://tug.org/mactex/)
  - Linux: TeX Live via your package manager.

Verifica tu instalación ejecutando los siguientes comandos en el terminal:

```bash
pandoc --version
pdflatex --version
```

Deberían mostrar información sobre la versión. Si no es así, asegúrate de que estén correctamente instalados y añadidos al PATH de tu sistema.

## 2. Crear el archivo YAML

Crea un archivo YAML. Puede llamarse, por ejemplo, `cv.yaml`. Este archivo almacenará tu información personal y las secciones de tu CV, actuando como tu "Fuente Única de Verdad". Si consigues un nuevo trabajo o cambias tu número de teléfono, este es el único archivo que necesitarás editar.

Es necesario escribir siguiendo la sintaxis YAML. No te preocupes, está diseñado para ser legible y fácil de escribir. Para referencia, puedes consultar la [especificación YAML](https://yaml.org/spec/1.2/spec.html).

Aquí tienes un ejemplo de YAML con mis datos:

```yaml
name: "Xicu Marí Prats"
profile: >
  Ingeniero de investigación con más de 3 años de experiencia práctica en diseño 
  de hardware digital. Buscando oportunidades en entornos de I+D para impulsar la innovación.

contact:
  email: "hi@xicu.net"
  website: "xicu.net"
  linkedin: "linkedin.com/in/xicu"

key_skills: [
  "VHDL", "SystemVerilog", "Xilinx Vivado", "Altera Quartus II", "FPGA Design",
  "OpenPiton", "C/C++", "Python", "Cadence", "KiCad", "Altium", "Linux", "Git"
]

professional_experience:
  - position: "Research Engineer"
    company: "HPDSA Group at Barcelona Supercomputing Center"
    dates: "Dec 2024 -- Present"
    details:
      - "Implementación de un acelerador DAE de última generación para cargas de trabajo de IA."
      - "Integración de módulos de hardware en el framework OpenPiton."

education:
  - degree: "Máster en Ingeniería Electrónica"
    institution: "Universitat Politècnica de Catalunya"
    dates: "Sep 2022 -- Sep 2025"
    details:
      - "Nota media: 9.1 / 10"
```

> **Nota**: Si tus datos contienen caracteres especiales de LaTeX como & o %, recuerda escaparlos (por ejemplo, 80\%) para evitar errores de compilación.

## 3. Crear la plantilla

Ahora, definamos el estilo de tu CV. Pandoc trata esto como una **plantilla**, donde las variables serán sustituidas por los datos de tu archivo YAML.

Un ejemplo mínimo podría ser el siguiente. Aquí, las `$variables$` serán reemplazadas por su correspondiente valor. Los bucles como `$for(experience)$ … $endfor$` te permiten repetir bloques para cada elemento de una lista. Puedes guardar el archivo como `cv.tex`:

```tex
\documentclass[11pt,a4paper]{article}
\usepackage{geometry}
\geometry{margin=2cm}
\usepackage{enumitem}
\usepackage{hyperref}

\begin{document}

\begin{center}
    {\Huge \textbf{$name$}} \\[0.5em]
    \href{mailto:$contact.email$}{$contact.email$} | \href{https://$contact.website$}{$contact.website$} | \href{https://$contact.linkedin$}{$contact.linkedin$}
\end{center}

\vspace{1em}

\section*{Education}
$for(education)$
\textbf{$education.degree$}, $education.institution$ \hfill $education.dates$ \\
$endfor$

\section*{Experience}
$for(professional_experience)$
\textbf{$professional_experience.position$} --- $professional_experience.company$ \hfill $professional_experience.dates$ \\
\begin{itemize}[leftmargin=*]
$for(professional_experience.details)$
    \item $professional_experience.details$
$endfor$
\end{itemize}
$endfor$

\end{document}
```

## 4. Generar el PDF

Con todo listo, puedes generar el PDF de tu CV con el siguiente comando en el terminal:

```bash
pandoc cv.yaml \
    --template=cv.tex \
    -o cv.pdf \
    --pdf-engine=pdflatex
```

Este comando le indica a Pandoc que:
1. Cargue los datos de `cv.yaml`.
2. Use `cv.tex` como plantilla.
3. Genere un PDF mediante `pdflatex`.

## 5. Un paso más: Automatización

Puedes automatizar el proceso de generación utilizando un sistema de construcción como Makefile o Scons, o simplemente con un script de shell. De esta forma, puedes añadir una lógica más compleja, como generar diferentes versiones de tu CV para distintas ofertas de empleo o dar soporte a varios idiomas.

---

He compartido mi repositorio completo, que incluye una plantilla más avanzada y scripts automatizados, en GitHub:

👉 [https://github.com/XicuM/cv](https://github.com/XicuM/cv)

¡Espero que te sirva de ayuda! Si tienes algún problema con Pandoc o LaTeX, no dudes en dejar un comentario abajo.
