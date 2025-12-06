---
title: Create a CV with LaTeX
date: 2024-09-12T11:29:05+02:00
comments: true
draft: false
tags: [LaTeX]
cover:
    image: /posts/latex_cv.jpg
---

One of the nicest features of **Pandoc** is its ability to combine structured data (like YAML) with a LaTeX template to generate professional-looking PDFs. This makes it especially useful for creating a **curriculum vitae (CV)**, where you may want to keep your personal information separate from the style. That way, updating your CV is as simple as editing a `.yaml` file rather than touching your LaTeX template every time.

In this article, I’ll explain how to set up such a system: a `.yaml` file for your personal info, a `.tex` file for the design of your CV, and Pandoc with `pdflatex` to compile everything into a polished PDF.

## Why use Pandoc + LaTeX for a CV?

* **Separation of content and style**: Your data (education, work experience, skills) lives in a simple YAML file, while the look of the CV is controlled in a LaTeX template.
* **Easy updates**: Update your details in the YAML and regenerate the PDF without worrying about formatting.
* **Consistency**: You can maintain multiple CV styles (academic, industry, one-page résumé) without duplicating content.
* **Automation**: Generate CVs with a single command in the terminal.

## Step 1. Install the required tools

You’ll need:

* **Pandoc** – [install instructions](https://pandoc.org/installing.html)
* **TeX distribution** (e.g., TeX Live, MikTeX) with `pdflatex` available

Check your setup by running:

```bash
pandoc --version
pdflatex --version
```

## Step 2. Create your YAML file

Let’s call it `cv.yaml`. This file will store your personal information and CV sections. For example:

```yaml
name: "Jane Doe"
email: "jane.doe@example.com"
phone: "+34 600 123 456"
linkedin: "linkedin.com/in/janedoe"

education:
  - degree: "M.Sc. in Computer Science"
    institution: "University of Barcelona"
    year: "2023"
  - degree: "B.Sc. in Mathematics"
    institution: "Universitat Autònoma de Barcelona"
    year: "2021"

experience:
  - role: "Software Engineer"
    company: "TechCorp"
    years: "2023–present"
    details:
      - "Backend development in Python and Go"
      - "Designed and maintained CI/CD pipelines"
  - role: "Research Assistant"
    company: "Barcelona Supercomputing Center"
    years: "2022–2023"
    details:
      - "Worked on sparse tensor optimizations for RISC-V"
```

This structured format makes it very easy to maintain.

## Step 3. Create your LaTeX CV template

Now let’s create a file `cv.tex` that defines the style of your CV. Pandoc treats this as a **template**, where variables from the YAML will be injected.

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
    \href{mailto:$email$}{$email$} | $phone$ | \href{https://$linkedin$}{$linkedin$}
\end{center}

\vspace{1em}

\section*{Education}
$for(education)$
\textbf{$education.degree$}, $education.institution$ \hfill $education.year$ \\
$endfor$

\section*{Experience}
$for(experience)$
\textbf{$experience.role$} --- $experience.company$ \hfill $experience.years$ \\
\begin{itemize}[leftmargin=*]
$for(experience.details)$
    \item $experience.details$
$endfor$
\end{itemize}
$endfor$

\end{document}
```

Here, the `$variables$` will be replaced by the contents of your YAML file. Loops like `$for(experience)$ … $endfor$` let you repeat blocks for each item in a list.

## Step 4. Generate your PDF

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

## Step 5. Customize the style

The real power of this setup comes from LaTeX. You can:

* Add color with `xcolor`
* Use modern fonts with `fontspec` (if you use XeLaTeX or LuaLaTeX)
* Adjust layout with `parskip`, `titlesec`, or custom commands
* Even create multiple templates (`cv-academic.tex`, `cv-industry.tex`) while keeping a single `cv.yaml`

## Wrapping up

Using **Pandoc + LaTeX templates + YAML data** gives you a maintainable, automated way to build CVs. Instead of copy-pasting between Word files, you just update your structured YAML and rebuild.

This approach is not only cleaner and more flexible but also scales if you want to create **multiple styles of CVs** for different purposes.
