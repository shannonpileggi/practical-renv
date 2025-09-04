# practical {renv}

A posit::conf(2025) talk.

# abstract

The {renv} package aims to *help* users create reproducible environments for R projects. In theory, this is great! In practice, restoring a package environment can be a frustrating process due to overlooked R configuration requirements. Join me to better understand the source of environment restoration issues and learn strategies for successful maintenance of {renv}-backed projects.

# slides

Made in google slides. [slide deck linked here](https://docs.google.com/presentation/d/1MTjQujGJda1L4pFNNOjhyPpYYJtl4jReOAtFDjn1wtc/edit?usp=sharing)

text `#E8E9F0`; background `#30304B`; accent `#C3D350`

media

 * [plant photo](https://unsplash.com/photos/green-succulent-plants-on-brown-clay-pots-bf0FRl7p_rE) by Vera Cho on Unsplash
 * [framework photo](https://unsplash.com/photos/black-metal-canopy-frame-dSRhwPe6v9c) by D R on Unsplash
 * [pudgy penguin gifs](https://giphy.com/pudgypenguins) from GIPHY

# background

* {packrat}

  + Sep 2014 {packrat} 0.4.1 first released to CRAN
  
  + Sep 2023 {packrat} 0.9.2 last release
  
  + {packrat} has been soft-deprecated and is now superseded by {renv}.

* {renv}

  + Oct 2019 v0.8.0 released to CRAN 

  + Jul 2023 [v1.0.0](https://github.com/rstudio/renv/releases/tag/v1.0.0) released

     - May 2023 substantial documentation updates <https://github.com/rstudio/renv/pull/1236>
    
  + Jan 2025 v1.1.0 released

# related talks

* rstudio::conf(2020), Kevin Ushey, [renv: Project Environments for R](https://youtu.be/yjlEbIDevOs?si=xGEZ3NDMfclwNzf7)

* rstudio::conf(2022), E. David Aja, [You should use renv](https://www.youtube.com/watch?v=GwVx_pf2uz4)

# tips

These are tips that I did not have the time to include in the presentation.

## general tips

1. Manage expectations when using {renv}. Keep in mind that {renv} is a tool that falls in the middle of the reproducibilty spectrum; accordingly, there is user responsibility to ensure package installation requirements are met.
2. The happy path is to update R and packages regularly.
3. Keep projects small in scope.
   + When you update R and packages, test everything to make sure it still works without error. More items to test means more possible failure points.
   + More package dependencies could also potential points of failure in {renv} restoration.
5. Install binaries when available to speed up installation and reduce complications from source installation.

## non-{renv} tips

1. You can install/use multiple R versions and switch as needed for different projects. Consider [rig](https://github.com/r-lib/rig) to manage your R versions.

2. Use the `SETUP` interface on p3m at <[https://packagemanager.posit.co/client/#/](https://packagemanager.posit.co/client/#/repos/cran/setup)> to retrieve the URL for your operating system,
   snapshot date, and environment.

3. Set your default repository for package installation to p3m. You can add multiple repositories. This url is for latest package versions on Windows.

    -> `usethis::edit_r_profile()`

    -> in .Rprofile, add `options("repos" = c("p3m" = "https://packagemanager.posit.co/cran/latest"))`

4.  Inspect the packages installed in your libraries

   + names only `lapply(.libPaths(), list.files)`
     
   + data frame with full details
  
```
df_pkg <- lapply(.libPaths(), \(x) as.data.frame(installed.packages(x))) |>
    tibble::enframe() |> 
    tidyr::unnest(cols = c(value))
```

5.  Watch the [Personal R Administration](https://www.youtube.com/watch?v=m2eihAhl8so) workshop from the 2025 R in Medicine conference for more details on these topics.

## factors that can affect {renv} behavior

1. {renv} version
2. configured repositories
3. whether or not packages are already installed vs not yet installed in system library / cache.

## {renv} tips

1. The location of the global cache has changed with different versions of {renv}. This can lead to confusing results/messaging if you are switching between {renv} versions (e.g., seeing that a package is not installed but you thought it was).
   
2. You should probably use {renv} ≥ v1.1.0.

3. `renv::upgrade()` is expected to work for {renv} versions ≥ 1.0.1. To upgrade from prior versions of {renv}, users should
```
renv::deactivate()
install.packages("renv")
renv::activate()
renv::record("renv")
```

4. The default mode for capturing package dependencies is implicit, in which `renv::dependencies()` parses all .R, .Rmd, .qmd and DESCRIPTION files for packages used. This can lead to slow project start up or restart up if there are many files to scan. You can change this to [explict](https://rstudio.github.io/renv/articles/faq.html?q=explicit#capturing-explicit-dependencies), which captures dependencies in the `DESCRIPTION` file only (no longer parses files).

5. `renv::checkout()` is recommended for use after 2023-07-07 (v1.0.0 of {renv}, when the checkout function was released). Note that using `renv::checkout(date = "xxxx-xx-xx")` also forces the version of {renv} to be latest available at that date.
   
6. [`renv::restore()`](https://rstudio.github.io/renv/reference/restore.html) has a lot of arguments. Read the help file! In particular, if you suffer from a time consuming installation in which one single package fails and you have to start over with all of your installations, try `renv::restore(transactional = FALSE)`.

7. You can see different installation behaviors depending on the version of {renv} and whether or not your cache has any version of that package populated vs no version at all of that package.

8. When all else fails, force installation of latest versions.
```
pkgs <- renv::lockfile_read("renv.lock")
install.packages(names(pkgs$Packages))
```

# example projects

I created two example projects to demonstrate resuming projects over time. Both have the package repository set to CRAN at <https://cloud.r-project.org>.

|   | project                                                                | R version | {renv} version | package       | simulated initialization date |
| - | -----------------------------------------------------------------------| --------- | -------------- | ------------- | ----------------------------- |
| 1 | [glue-example](https://github.com/shannonpileggi/glue-example)         | 4.3.1     | 1.0.0          | glue 1.6.2   |  2023-08-01                   |
| 2 | [jsonlite-example](https://github.com/shannonpileggi/jsonlite-example) | 4.4.2     | 1.1.0          | jsonlite 1.8.9 |  2025-02-01                   |

I intially started working with the {glue} example. However, enough changed between the 1.0.0 and 1.1.0 release of {renv} to substantially complicate the presentation. 
As {glue} has few releases, {jsonlite} ended up being a better package to demonstrate changes over time that line up with more recent {renv} releases. However, it may
have also been a poor example choice as {jsonlite} is listed as a "Suggests" for {renv}.

# project playground

I experimented with {renv} on both my local Windows computer and in docker containers.

```
docker run --rm -ti -e DISABLE_AUTH=true -p 127.0.0.1:8787:8787 rocker/rstudio:4.5.1
```

Open `localhost:8787` in browser to see RStudio interface.

```
git clone https://github.com/shannonpileggi/jsonlite-example
```

# contributions welcome

Other tips you want to add or anything else? Feel free to create an issue.
