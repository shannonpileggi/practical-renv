# practical {renv}

A posit::conf(2025) talk.

# abstract

The {renv} package aims to *help* users create reproducible environments for R projects. In theory, this is great! In practice, restoring a package environment can be a frustrating process due to overlooked R configuration requirements. Join me to better understand the source of environment restoration issues and learn strategies for successful maintenance of {renv}-backed projects.

# slides

Made in google slides. <insert link>

text `#E8E9F0`; background `#30304B`; accent `#C3D350`

# background

* {packrat}

  + Sep 2014 {packrat} 0.4.1 first released to CRAN
  
  + Sep 2023 {packrat} 0.9.2 last release
  
  + {packrat} has been soft-deprecated and is now superseded by {renv}.

* {renv}

  + Oct 2019 v0.8.0 released to CRAN 

  + Jul 2023 [v1.0.0](https://github.com/rstudio/renv/releases/tag/v1.0.0) released

     - May 2023 substanstial documentation updates <https://github.com/rstudio/renv/pull/1236>
    
  + Jan 2025 v1.1.0 released

# related talks

* rstudio::conf(2020), Kevin Ushey, [renv: Project Environments for R](https://youtu.be/yjlEbIDevOs?si=xGEZ3NDMfclwNzf7)

* rstudio::conf(2022), E. David Aja, [You should use renv](https://www.youtube.com/watch?v=GwVx_pf2uz4)

# example projects

I created two example projects to demonstrate resuming projects over time. Both have the package repository set to CRAN at <https://cloud.r-project.org>.

|   | project                                                                                                  | R version | {renv} version | package       |
| - | -------------------------------------------------------------------------------------------------------- | --------- | -------------- | ------------- |
| 1 | [glue-example](https://github.com/shannonpileggi/glue-example)         | 4.3.1     | 1.0.0          | glue v1.6.2   |
| 2 | [jsonlite-example](https://github.com/shannonpileggi/jsonlite-example) | 4.4.2     | 1.1.0          | jsonlite1.8.9 |

I intially started working with the {glue} example. However, enough changed between the 1.0.0 and 1.1.0 release of {renv} to substantially complicate the presentation. 
As {glue} has few releases, {jsonlite} ended up being a better package to demonstrate changes over time that line up with more recent {renv} releases.

# project playground

I used docker containers to assess workflows under various conditions. You can try this, too! 

```
docker run --rm -ti -e DISABLE_AUTH=true -p 127.0.0.1:8787:8787 rocker/rstudio:4.5.1
```

Open `localhost:8787` in browser to see RStudio interface.

File -> New project -> https://github.com/shannonpileggi/glue-example
