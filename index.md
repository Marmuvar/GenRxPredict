--- 
output:
  bookdown::pdf_document2:
    toc: no
    latex_engine: xelatex
    keep_tex: yes
    keep_md: true
  bookdown::html_document2: default
bibliography: [book.bib, packages.bib]
biblio-style: apalike
fontsize: 11 pt
geometry: left=1in, right=1in, top=1in, bottom=1in
linkcolor: blue
urlcolor: blue
citecolor: blue
link-citations: yes
editor_options:
  chunk_output_type: console
header-includes:
- \usepackage{setspace}
- \doublespace
- \usepackage{booktabs}
- \usepackage{longtable}
- \usepackage{array}
- \usepackage{multirow}
- \usepackage{wrapfig}
- \usepackage{float}
- \usepackage{colortbl}
- \usepackage{pdflscape}
- \usepackage{tabu}
- \usepackage{threeparttable}
- \usepackage{threeparttablex}
- \usepackage[normalem]{ulem}
- \usepackage{makecell}
- \usepackage{xcolor}

---

\begin{center}Classification of Generic Manufacturers and Competition in the Pharmaceutical Industry
  
  
A Thesis Submitted in Partial Fulfillment of the  
Requirements for the Degree of  
Master of Arts  
  
  
  
by  

Mark Benmuvhar  
  
Dr. Scott Alberts, Thesis Advisor
  
Department of Data Science
  
School of Arts and Letters
  
2022
  
  
TRUMAN STATE UNIVERSITY  
Kirksville, Missouri
\end{center}

# Abstract {-}
Patent and formulation complexity protect branded pharmaceutical products from generic product competition.  Custom modules coded in Python extracted data from Food and Drug Administration product approvals and label databases to establish brand and generic product relationships.  Between 1982 and 2021, product approvals increased alongside an increase in patents per product.  Generic competition during patent lifespan were assessed in R using tree-based classification, support vector machines, and gradient boosted tree algorithms.  Using the gradient boosted approach, models with significant accuracies (p <0.05) of 80.9%, 70.9%, and 78.9% predicted generic competition before first, before last, or after last patent expiry, respectively.
\newpage

