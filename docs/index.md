--- 
title: "DLL 2021, R section"
author: 
    - "mesako"
    - "Margaret"
    - "Samson"
    - "Zac"
    - "darachm"
date: "Typeset on 2021-06-12"
output:
  bookdown::gitbook:
    css: style.css
    config:
      toc:
        before: |
          <li><a href="./">DLL 2021 R workshops</a></li>
      edit: https://github.com/darachm/dll-r/edit/main/%s
      download: ["html"] #,"pdf"]
      highlight: zenburn
    includes:
      after_body: footer.html
    #number_sections: false
    split_by: section
documentclass: book
github-repo: darachm/dll-r
description: "A living syllabus for the R section of DLL 2021, at Stanford"
#site: bookdown::bookdown_site
#biblio-style: apalike
#link-citations: yes
---

# Workshop Introduction

This short 3-day course in R aims to give you a basic framework and skills
for working effectively with your research mentors.
Together, we will get oriented with basic skills (e.g. using RStudio, 
documenting your process with R Markdown, reading data in,
basic data analysis, and visualization) 
and concepts for how to organize your research workflows in R.
We intend for this starting point to empower you to accomplish 
research-related tasks in R.

R is a high-level data analysis scripting language
^[and also a "GNU" project, apparently!].
While it is very easy to write programs in this language,
it is designed first as an environment that stitches together 
cutting edge research methods with flexible visualization and reporting
frameworks.
R has swept to be the de facto high-level language for data analysis
because of the rich ecosystem of dispersed open-source developers.

[Here's some examples](https://www.r-graph-gallery.com/) of plots you 
generate in R.
[Here's an example](https://bioconductor.org/packages/release/workflows/vignettes/cytofWorkflow/inst/doc/cytofWorkflow.html)
of the types of workflows and analyses you can generate in R (all the plots,
and the website too).
Heck, this website is generated by the R package `bookdown` from Rmd files,
which you will learn to write.

## Workshop goals

<!-- darach: I thought it might be good for this section to be more
    specific, whadda think? revert/change back if that's a no -->

We aim for all participants to be able to:

- use the [Rstudio IDE](https://www.rstudio.com/products/rstudio/) 
    (open source edition)
- know how to store and manipulate data in variables
- read in data from computer files in various formats
- process these with functions to generate statistical summaries
- turn these into various plots using the base graphics and ggplot2 library
- read in packages from various sources and know how to start using them
- do these steps in workflows that scale to analyzing many many files
- write all of this up as an Rmarkdown file to report your analysis and
    findings to collaborators

## Structure and resources

### Workshop schedule

Each day will have a slightly different schedule, but you can expect a mix of synchronous
and asynchronous work sessions, as well as two breaks in that day's workshop.

Please visit each day's specific page for the exact schedule:

+ [Day 3 Schedule](https://darachm.github.io/dll-r/day-3-intoduction-to-r.html)
+ [Day 4 Schedule](https://darachm.github.io/dll-r/day-4-tidyverse-and-visualizations.html)
+ [Day 5 Schedule](https://darachm.github.io/dll-r/day-5-building-workflows-using-packages-writing-reusable-code-sharing-analyses.html)


### Asynchronous sessions

Asynchronous here means self-paced learning that takes place off-Zoom.

You will be expected to progress through this website during asynchronous 
work time in this 3-day period. We developed this website/document for your reference,
as a living textbook and collection of "slides" and code snippets.

We have also made short teaching videos uploaded on Youtube that are embedded 
where appropriate on this site. These mini-lectures are intended to complement 
the text for those with different learning preferences.

As you go through this website during the asynchronous sessions, you should also complete
the exercises provided in the accompanying worksheets. If you have questions and/or
need help, you should reach out to us and your peers on Slack.

Tips:

- You can shrink the table of contents (left) by clicking the four lines icon
    in the top menu.
- You can click on footnotes ^[to read them, then go back by clicking the
    arrow].

#### Slack channel

While you are working in asynchronous sessions, or if you just need help 
during the remainder of your program, there is a Slack channel available
where you can go for ideas/help. The channel is called *#learn-R* and should be 
accessible to you on the SSRP Slack server.

### Synchronous sessions

Synchronous here means live, group learning that takes place on-Zoom.

During synchronous meetings, you should plan 
to work directly with your peers and us on focused tasks. We will also be 
there to help with confusing or challenging topics that you want to 
discuss with someone live.

During the schedule synchronous session (check each day's schedule for
timing), you should log on to the provided Zoom link.


## Workshop expectations

1. Be respectful and compassionate.
2. Teach one another, learn from one another.
3. Aim for productive struggle.
+ You will learn best if you make a good faith effort before seeking help.
+ However, you should always seek help if you feel truly stuck.
4. Create your own sense of challenge.
+ Pick activities that you will learn and grow from.
+ If you don't find something challenging, make it challenging for yourself.
