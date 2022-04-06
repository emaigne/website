---
title: "Dépendances entre fonctions d'un package"
author: "Elise Maigné"
date: "03/09/2020"
slug: []
categories:
  - codes
tags:
  - codes
draft: false
summary: Représenter les dependances entre fonctions d'un même package
---

<script src="{{< blogdown/postref >}}index_files/htmlwidgets/htmlwidgets.js"></script>
<script src="{{< blogdown/postref >}}index_files/viz/viz.js"></script>
<link href="{{< blogdown/postref >}}index_files/DiagrammeR-styles/styles.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/grViz-binding/grViz.js"></script>

Le code qui suit, écrit par [Carl B Frederik](https://www.carlbfrederick.com/post/uncovering-the-relationships-among-functions-in-a-package/) permet de tracer les relations entre les fonctions d’un package.

J’ai légèrement modifié sa fonction car ça me retourne une erreur, en ajoutant les lignes suivantes dans la fonction `scan_fcn`.

      if(is.infinite(start) | is.infinite(stop)){
        start <- 0
        stop <- 0
      }

**Edit.** Le code de l’article de blog a évolué depuis, le courant est sur son gist :
https://gist.github.com/carlbfrederick/b30d861ea80a27fad4e44623c41e0170

## Affichier les dépendances

On commence par créer la fonction :

``` r
suppressMessages(
  devtools::source_gist("b30d861ea80a27fad4e44623c41e0170", filename = "packageFunctionMap.R")  
)
```

On l’appelle sur un package, installé sur sa machine :

``` r
plotdep <- plotFcnDependencies("SOMbrero")
```

On l’affiche :

``` r
plotdep %>% 
    select_nodes_by_degree(expressions = "deg > 0") %>% 
    transform_to_subgraph_ws %>% 
    render_graph(width = 700, height = 700)
```

<div id="htmlwidget-1" style="width:700px;height:700px;" class="grViz html-widget"></div>
<script type="application/json" data-for="htmlwidget-1">{"x":{"diagram":"digraph {\n\ngraph [layout = \"dot\",\n       outputorder = \"edgesfirst\",\n       bgcolor = \"white\",\n       rankdir = \"LR\"]\n\nnode [fontname = \"Helvetica\",\n      fontsize = \"10\",\n      shape = \"circle\",\n      fixedsize = \"false\",\n      width = \"0.5\",\n      style = \"filled\",\n      fillcolor = \"aliceblue\",\n      color = \"gray70\",\n      fontcolor = \"gray50\"]\n\nedge [fontname = \"Helvetica\",\n     fontsize = \"8\",\n     len = \"1.5\",\n     color = \"gray80\",\n     arrowsize = \"0.5\"]\n\n  \"2\" [label = \"calculateClusterEnergy\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"3\" [label = \"calculateEnergy\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"4\" [label = \"calculateRadius\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"5\" [label = \"coords_polydist\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"6\" [label = \"cosinePreprocess\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"7\" [label = \"dendro3dProcess\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"8\" [label = \"depth3d\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"9\" [label = \"distEuclidean\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"10\" [label = \"distRelationalProto\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"11\" [label = \"gg_color\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"12\" [label = \"ggplotEnergy\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"13\" [label = \"ggplotFacet\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"14\" [label = \"ggplotGrid\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"15\" [label = \"initGrid\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"16\" [label = \"initProto\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"17\" [label = \"initSOM\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"18\" [label = \"korrespPreprocess\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"19\" [label = \"myTitle\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"20\" [label = \"obsAffectation\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"21\" [label = \"oneObsAffectation\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"22\" [label = \"orderIndexes\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"23\" [label = \"plot.myGrid\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"24\" [label = \"plot.somRes\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"25\" [label = \"plot.somSC\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"26\" [label = \"plot3d\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"27\" [label = \"plotAdd\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"28\" [label = \"plotObs\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"29\" [label = \"plotProjGraph\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"30\" [label = \"plotPrototypes\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"31\" [label = \"predict.somRes\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"32\" [label = \"predictRSOM\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"33\" [label = \"preprocessData\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"34\" [label = \"preprocessProto\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"36\" [label = \"print.paramSOM\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"39\" [label = \"projectFactor\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"40\" [label = \"projectGraph\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"42\" [label = \"projectIGraph.somRes\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"43\" [label = \"projectIGraph.somSC\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"44\" [label = \"protoDist\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"45\" [label = \"protoDist.somRes\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"46\" [label = \"prototypeUpdate\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"47\" [label = \"quality\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"48\" [label = \"quality.somRes\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"49\" [label = \"quantizationError\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"50\" [label = \"run.trainSOM\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"51\" [label = \"selectNei\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"52\" [label = \"selectObs\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"56\" [label = \"summary.somRes\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"57\" [label = \"summary.somSC\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"59\" [label = \"superClass.somRes\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"60\" [label = \"theme_facet\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"61\" [label = \"topographicError\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"62\" [label = \"trainSOM\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"63\" [label = \"trainSOM.data.frame\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"64\" [label = \"trainSOM.dist\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"65\" [label = \"trainSOM.double\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"66\" [label = \"trainSOM.kernel\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"67\" [label = \"trainSOM.matrix\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"68\" [label = \"trainSOM.numeric\", fontsize = \"20\", shape = \"rectangle\", fillcolor = \"#F0F8FF\", fontcolor = \"#000000\"] \n  \"3\"->\"2\" \n  \"50\"->\"3\" \n  \"3\"->\"4\" \n  \"50\"->\"4\" \n  \"14\"->\"5\" \n  \"33\"->\"6\" \n  \"25\"->\"7\" \n  \"26\"->\"8\" \n  \"21\"->\"9\" \n  \"45\"->\"10\" \n  \"25\"->\"11\" \n  \"26\"->\"11\" \n  \"29\"->\"11\" \n  \"39\"->\"11\" \n  \"24\"->\"12\" \n  \"27\"->\"13\" \n  \"28\"->\"13\" \n  \"30\"->\"13\" \n  \"23\"->\"14\" \n  \"25\"->\"14\" \n  \"27\"->\"14\" \n  \"28\"->\"14\" \n  \"30\"->\"14\" \n  \"17\"->\"15\" \n  \"50\"->\"16\" \n  \"31\"->\"18\" \n  \"33\"->\"18\" \n  \"13\"->\"19\" \n  \"14\"->\"19\" \n  \"31\"->\"20\" \n  \"20\"->\"21\" \n  \"50\"->\"21\" \n  \"13\"->\"22\" \n  \"25\"->\"26\" \n  \"30\"->\"26\" \n  \"24\"->\"27\" \n  \"24\"->\"28\" \n  \"24\"->\"30\" \n  \"32\"->\"31\" \n  \"50\"->\"31\" \n  \"50\"->\"32\" \n  \"45\"->\"33\" \n  \"49\"->\"33\" \n  \"50\"->\"33\" \n  \"56\"->\"33\" \n  \"57\"->\"33\" \n  \"61\"->\"33\" \n  \"16\"->\"34\" \n  \"31\"->\"34\" \n  \"45\"->\"34\" \n  \"49\"->\"34\" \n  \"61\"->\"34\" \n  \"50\"->\"36\" \n  \"25\"->\"39\" \n  \"27\"->\"39\" \n  \"27\"->\"40\" \n  \"42\"->\"40\" \n  \"43\"->\"40\" \n  \"43\"->\"42\" \n  \"25\"->\"43\" \n  \"30\"->\"44\" \n  \"59\"->\"44\" \n  \"50\"->\"46\" \n  \"56\"->\"47\" \n  \"48\"->\"49\" \n  \"63\"->\"50\" \n  \"64\"->\"50\" \n  \"65\"->\"50\" \n  \"66\"->\"50\" \n  \"67\"->\"50\" \n  \"68\"->\"50\" \n  \"2\"->\"51\" \n  \"20\"->\"51\" \n  \"21\"->\"51\" \n  \"32\"->\"51\" \n  \"45\"->\"51\" \n  \"50\"->\"51\" \n  \"61\"->\"51\" \n  \"50\"->\"52\" \n  \"13\"->\"60\" \n  \"48\"->\"61\" \n  \"63\"->\"62\" \n  \"64\"->\"62\" \n  \"65\"->\"62\" \n  \"66\"->\"62\" \n  \"67\"->\"62\" \n  \"68\"->\"62\" \n}","config":{"engine":"dot","options":null}},"evals":[],"jsHooks":[]}</script>

## Code de `plotFcnDependencies`

``` r
library(tidyverse)
library(DiagrammeR)

#Get package functions ----

ls_fcns <- function(pkg) {
  fcns <- unclass(lsf.str(envir = asNamespace(pkg), all = TRUE))
  return(as.character(fcns))
}

#Utility Function to weed out false positives
scan_fcn <- function(from, to) {
  this_fcn <- trimws(capture.output(getAnywhere(from)), which = "both")
  #subset to guts of function definition
  start <- min(grep("^function", this_fcn)) + 1
  stop <- max(grep("^}", this_fcn)) - 1
  if(is.infinite(start) | is.infinite(stop)){
    start <- 0
    stop <- 0
  }
  this_fcn <- this_fcn[start:stop]
  
  #Remove comment lines
  this_fcn <- this_fcn[!grepl("^#", this_fcn)]  
  
  #Lines called functions directly
  idx1 <- grepl(paste(to, "\\(", sep=""), this_fcn)
  
  #Lines called via *map*, *walk*, mutate_at/all, summarize_at/all, *apply
  idx2 <- grepl(to, this_fcn) & 
    (grepl("summari[sz]e\\_(all)?(if)?(at)?\\(", this_fcn) | 
       grepl("(trans)?mute?(ate)?\\_(all)?(if)?(at)?\\(", this_fcn) | 
       grepl("[lp]?map2?(\\_if)?(\\_at)?(\\_lgl)?(\\_chr)?(\\_int)?(\\_dbl)?(\\_raw)?(\\_dfr)?(\\_dfc)?(\\_depth)?\\(", this_fcn) |
       grepl("p?walk2?\\(", this_fcn) | 
       grepl("[ltsmvr]?apply\\(", this_fcn))
  
  sum(idx1 | idx2)
}


#Search for other package functions called by function

fcn_deps <- function(pkg) {
  fcns <- ls_fcns(pkg)
  
  out <- tibble(
    Function = fcns, 
    Dependency_Function = fcns
  ) %>% 
    expand(Function, Dependency_Function) %>% 
    filter(Function != Dependency_Function) %>% 
    mutate(
      Number_Calls = map2_int(Function, Dependency_Function, scan_fcn)
    ) %>% 
    filter(Number_Calls > 0)
  
  return(out)
}


plotFcnDependencies <- function(pkg) {
  fcns <- ls_fcns(pkg)
  depFcn <- fcn_deps(pkg)
  
  depth <- NULL
  
  nodes <- create_node_df(n = length(fcns),
                          label = fcns,
                          type = "",
                          fontsize = 20,
                          shape = "rectangle")
  
  nodes$id <- 1:nrow(nodes)
  
  edges <- data.frame(fromLab = depFcn$Function,
                      toLab = depFcn$Dependency_Function,
                      stringsAsFactors = FALSE)
  
  edges <- nodes %>% 
    select(from = id, fromLab = label) %>% 
    right_join(edges, by="fromLab")
  
  edges <- nodes %>% 
    select(to = id, toLab = label) %>% 
    right_join(edges, by="toLab") %>% 
    mutate(rel = "") %>% 
    select(from, to, rel, fromLab, toLab)
  library(tidyverse)
  library(DiagrammeR)
  
  #Get package functions ----
  
  ls_fcns <- function(pkg) {
    fcns <- unclass(lsf.str(envir = asNamespace(pkg), all = TRUE))
    return(as.character(fcns))
  }
  
  #Utility Function to weed out false positives
  scan_fcn <- function(from, to) {
    this_fcn <- trimws(capture.output(getAnywhere(from)), which = "both")
    #subset to guts of function definition
    start <- min(grep("^function", this_fcn)) + 1
    stop <- max(grep("^}", this_fcn)) - 1
    if(is.infinite(start) | is.infinite(stop)){
      start <- 0
      stop <- 0
    }
    this_fcn <- this_fcn[start:stop]
    
    #Remove comment lines
    this_fcn <- this_fcn[!grepl("^#", this_fcn)]  
    
    #Lines called functions directly
    idx1 <- grepl(paste(to, "\\(", sep=""), this_fcn)
    
    #Lines called via *map*, *walk*, mutate_at/all, summarize_at/all, *apply
    idx2 <- grepl(to, this_fcn) & 
      (grepl("summari[sz]e\\_(all)?(if)?(at)?\\(", this_fcn) | 
         grepl("(trans)?mute?(ate)?\\_(all)?(if)?(at)?\\(", this_fcn) | 
         grepl("[lp]?map2?(\\_if)?(\\_at)?(\\_lgl)?(\\_chr)?(\\_int)?(\\_dbl)?(\\_raw)?(\\_dfr)?(\\_dfc)?(\\_depth)?\\(", this_fcn) |
         grepl("p?walk2?\\(", this_fcn) | 
         grepl("[ltsmvr]?apply\\(", this_fcn))
    
    sum(idx1 | idx2)
  }
  
  
  #Search for other package functions called by function
  
  fcn_deps <- function(pkg) {
    fcns <- ls_fcns(pkg)
    
    out <- tibble(
      Function = fcns, 
      Dependency_Function = fcns
    ) %>% 
      expand(Function, Dependency_Function) %>% 
      filter(Function != Dependency_Function) %>% 
      mutate(
        Number_Calls = map2_int(Function, Dependency_Function, scan_fcn)
      ) %>% 
      filter(Number_Calls > 0)
    
    return(out)
  }
  
  
  plotFcnDependencies <- function(pkg) {
    fcns <- ls_fcns(pkg)
    depFcn <- fcn_deps(pkg)
    
    depth <- NULL
    
    nodes <- create_node_df(n = length(fcns),
                            label = fcns,
                            type = "",
                            fontsize = 20,
                            shape = "rectangle")
    
    nodes$id <- 1:nrow(nodes)
    
    edges <- data.frame(fromLab = depFcn$Function,
                        toLab = depFcn$Dependency_Function,
                        stringsAsFactors = FALSE)
    
    edges <- nodes %>% 
      select(from = id, fromLab = label) %>% 
      right_join(edges, by="fromLab")
    
    edges <- nodes %>% 
      select(to = id, toLab = label) %>% 
      right_join(edges, by="toLab") %>% 
      mutate(rel = "") %>% 
      select(from, to, rel, fromLab, toLab) 
    
    
    
    out <- DiagrammeR::create_graph(
      nodes_df = nodes,
      edges_df = edges,
      graph_name = paste(pkg, " (version ", packageVersion(pkg), ") Function Map", sep="")
    )
    
    out$global_attrs$value[out$global_attrs$attr == "layout"] <- "dot"
    out$global_attrs$value[out$global_attrs$attr == "fixedsize"] <- "false"
    out$global_attrs <- rbind(out$global_attrs, data.frame(attr = "rankdir", value = "LR", attr_type = "graph"))
    
    return(out)
  } 
  
  
  
  out <- DiagrammeR::create_graph(
    nodes_df = nodes,
    edges_df = edges,
    graph_name = paste(pkg, " (version ", packageVersion(pkg), ") Function Map", sep="")
  )
  
  out$global_attrs$value[out$global_attrs$attr == "layout"] <- "dot"
  out$global_attrs$value[out$global_attrs$attr == "fixedsize"] <- "false"
  out$global_attrs <- rbind(out$global_attrs, data.frame(attr = "rankdir", value = "LR", attr_type = "graph"))
  
  return(out)
}
```
