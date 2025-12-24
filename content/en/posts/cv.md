---
title: Craft a CV with LaTeX
date: 2025-12-24T10:29:05+01:00
comments: true
showToc: true
draft: false
tags: [LaTeX]
cover:
    image: /posts/cv/cover.png
---

A lot of times when I find myself updating my CV, I end up spending more time struggling with formatting than actually doing the tiny update I wanted. Frustrated by this, I thought that there should be a faster way to manage this process. The solution? Separate the content from the format. That means having a file containing your personal information and another file defining how to present it. This problem is nicely solved by LaTex, a tool widely used in academia for document preparation.

In this article, we'll combine LaTex with **Pandoc** to provide a simple way to generate a professional CV from data. This way, the content of your CV is stored in a structured format (YAML), and the design is handled by a LaTex template. Whenever you need to update your CV, you just modify the YAML file and regenerate the PDF.

## The workflow

The architecture is simple. Pandoc will read your data from a YAML file and use a LaTex template to format it into a PDF:

**YAML** *(The Data)* + **LaTeX** *(The Template)* → **Pandoc** *(The Engine)* → **PDF** *(The Result)*

Here’s how to set it up step-by-step.

## 1. Prerequisites

You will need two main tools installed on your system:
- **Pandoc**, the "universal document converter". You can read the install instructions [here](https://pandoc.org/installing.html).
- **TeX Distribution** that provides the PDF rendering engine. The recommended options are:
  - Windows: [MiKTeX](https://miktex.org/download)
  - MacOS: [MacTeX](https://tug.org/mactex/)
  - Linux: TeX Live via your package manager.

Verify your installation by running:

```bash
pandoc --version
pdflatex --version
```

They should print version information. If not, make sure they are correctly installed and added to your system PATH.

## 2. Create the YAML file

Create a YAML file named, for example, `cv.yaml`. This file will store your personal information and CV sections, acting as your "Single Source of Truth". If you get a new job or change your phone number, this is the only file you need to edit.

You need to follow the YAML syntax, but don't worry, it is designed to be human-readable and easy to write. For reference, you can check the [YAML specification](https://yaml.org/spec/1.2/spec.html).

Here's a YAML example with my details:

```yaml
name: "Xicu Marí Prats"
profile: >
  Research Engineer with 3+ years of hands-on experience in digital hardware
  design. Seeking opportunities in R&D environments to drive innovation.

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
      - "Implemented a state-of-the-art DAE accelerator for AI workloads."
      - "Integrated hardware modules into the OpenPiton framework."

education:
  - degree: "Master's Degree in Electronic Engineering"
    institution: "Universitat Politècnica de Catalunya"
    dates: "Sep 2022 -- Sep 2025"
    details:
      - "Overall grade: 9.1 / 10"
```

> **Note**: If your data contains special LaTeX characters like & or %, remember to escape them (e.g., R\&D) to avoid build errors.

## 3. Create the template

Now, let's define the style of your CV. Pandoc treats this as a **template**, where variables from the YAML will be injected.

A minimal example could look like the following. Here, the `$variables$` will be replaced by the contents of your YAML file. Loops like `$for(experience)$ … $endfor$` let you repeat blocks for each item in a list. Save this as `cv.tex`:

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

## 4. Generate the PDF

With everything set up, you can generate your CV PDF with:

```bash
pandoc cv.yaml \
    --template=cv.tex \
    -o cv.pdf \
    --pdf-engine=pdflatex
```

This command tells Pandoc to:
1. Load the data from `cv.yaml`.
2. Use `cv.tex` as the template.
3. Produce a PDF with `pdflatex`.

## 5. Taking it Further: Automation

You can automatize the generation process using a build system like **Makefile** or **Scons**, or keep it simple with a shell script. This way you can add more complex logic, like generating different versions of your CV for different job applications or support multiple languages.

---

I have shared my full repository, including a more advanced template and automated scripts, on GitHub: 

👉 [https://github.com/XicuM/cv](https://github.com/XicuM/cv).

Happy crafting! If you run into any issues with Pandoc or LaTeX environments, feel free to drop a comment below.
