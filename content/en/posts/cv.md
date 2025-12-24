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

A lot of times when I find myself updating my CV, I end up spending more time struggling with formatting than actually doing the tiny update I wanted. From this frustration, I though that there should be a fastest way to manage this process.

A great solution is to separate the content from the format, with a file containing your personal information and another file defining how to present it. This problem is nicely solved by LaTex, a tool widely used in academia for document preparation.

In this article, we'll combine LaTex with **Pandoc** to provide a simple way to generate a professional CV from data. This way, the content of your CV is stored in a structured format (YAML), and the design is handled by a LaTex template. Whenever you need to update your CV, you just modify the YAML file and regenerate the PDF.

## 1. Install the tools

You’ll need:
- **Pandoc**. You can read the install instructions [here](https://pandoc.org/installing.html).
- **TeX distribution** with `pdflatex` available. For instance, on Windows, you can install [MiKTeX](https://miktex.org/download); on macOS, you can use [MacTeX](https://tug.org/mactex/); and on Linux, you can install TeX Live via your package manager.

Check the setup by running:

```bash
pandoc --version
pdflatex --version
```

They should print version information. If not, make sure they are correctly installed and added to your system PATH.

## 2. Create the YAML file

Create a YAML file named, for example, `cv.yaml`. This file will store your personal information and CV sections. You need to follow the YAML syntax, but don't worry, it is designed to be human-readable and easy to write. For reference, you can check the [YAML specification](https://yaml.org/spec/1.2/spec.html).

Here's a YAML example with my details:

```yaml
name: "Xicu Marí Prats"
profile: >
  Research Engineer with 3+ years of hands-on experience in digital hardware
  design. Seeking opportunities in dynamic R&D environments to drive 
  innovation in hardware acceleration.

contact:
  email: "hi@xicu.net"
  website: "xicu.net"
  linkedin: "linkedin.com/in/xicu"
  github: "github.com/XicuM"
  location: "Barcelona, Spain"

personal_info:
  birthdate: 23/06/1999
  citizenship: "Spanish"
  languages:
    - name: "Catalan"
      level: "native"
    - name: "Spanish"
      level: "native"
    - name: "English"
      level: "B2"

key_skills: [
  "VHDL", "SystemVerilog", "Xilinx Vivado", "Altera Quartus II", "FPGA Design",
  "OpenPiton", "C/C++", "Python", "Cadence", "KiCad", "Altium", "Linux", "Git", 
  "Team Leadership", "Project Management"
]

professional_experience:
  - position: "Research Engineer"
    company: "HPDSA Group at Barcelona Supercomputing Center"
    dates: "Dec 2024 -- Present"
    details:
      - "Implemented and integrated a state-of-the-art decoupled access-execution 
        (DAE) accelerator for AI workloads in OpenPiton manycore processor framework"

education:
  - degree: "Master's Degree in Electronic Engineering"
    institution: "Universitat Politècnica de Catalunya"
    dates: "Sep 2022 -- Sep 2025"
    details:
      - "Overall grade: 9.1 / 10"
      - "Master's Thesis with honors: \"Integrating the Tensor Marshaling Unit 
        for Sparse Tensor Operations with a RISC-V Processor\" - Advanced 
        research in AI hardware acceleration"
```

You can create your own sections and fields as needed, even create multiple versions of the file for different job applications.

## 3. Create the LaTeX template

Now, let's create a file, for instance `cv.tex`, that defines the style of your CV. Pandoc treats this as a **template**, where variables from the YAML will be injected.

A minimal example could look like this:

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

Here, the `$variables$` will be replaced by the contents of your YAML file. Loops like `$for(experience)$ … $endfor$` let you repeat blocks for each item in a list.

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

## 5. Automatize the process

You can automatize the generation process using a build system like Makefile or Scons, or keep it simple with a shell script. This way you can add more complex logic, like generating different versions of your CV for different job applications or support multiple languages.

I recommend that you look into my CV repository where I have implemented this approach with a more complete template and additional features. It is available at GitHub: [https://github.com/XicuM/cv](https://github.com/XicuM/cv).

---

I hope you have found this guide useful! Feel free to reach out if you have any questions or need further assistance. Happy CV crafting!
