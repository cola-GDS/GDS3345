cola Report for GDS3345
==================

**Date**: 2019-12-25 20:43:55 CET, **cola version**: 1.3.2

----------------------------------------------------------------

<style type='text/css'>

body, td, th {
   font-family: Arial,Helvetica,sans-serif;
   background-color: white;
   font-size: 13px;
  max-width: 800px;
  margin: auto;
  margin-left:210px;
  padding: 0px 10px 0px 10px;
  border-left: 1px solid #EEEEEE;
  line-height: 150%;
}

tt, code, pre {
   font-family: 'DejaVu Sans Mono', 'Droid Sans Mono', 'Lucida Console', Consolas, Monaco, 

monospace;
}

h1 {
   font-size:2.2em;
}

h2 {
   font-size:1.8em;
}

h3 {
   font-size:1.4em;
}

h4 {
   font-size:1.0em;
}

h5 {
   font-size:0.9em;
}

h6 {
   font-size:0.8em;
}

a {
  text-decoration: none;
  color: #0366d6;
}

a:hover {
  text-decoration: underline;
}

a:visited {
   color: #0366d6;
}

pre, img {
  max-width: 100%;
}
pre {
  overflow-x: auto;
}
pre code {
   display: block; padding: 0.5em;
}

code {
  font-size: 92%;
  border: 1px solid #ccc;
}

code[class] {
  background-color: #F8F8F8;
}

table, td, th {
  border: 1px solid #ccc;
}

blockquote {
   color:#666666;
   margin:0;
   padding-left: 1em;
   border-left: 0.5em #EEE solid;
}

hr {
   height: 0px;
   border-bottom: none;
   border-top-width: thin;
   border-top-style: dotted;
   border-top-color: #999999;
}

@media print {
   * {
      background: transparent !important;
      color: black !important;
      filter:none !important;
      -ms-filter: none !important;
   }

   body {
      font-size:12pt;
      max-width:100%;
   }

   a, a:visited {
      text-decoration: underline;
   }

   hr {
      visibility: hidden;
      page-break-before: always;
   }

   pre, blockquote {
      padding-right: 1em;
      page-break-inside: avoid;
   }

   tr, img {
      page-break-inside: avoid;
   }

   img {
      max-width: 100% !important;
   }

   @page :left {
      margin: 15mm 20mm 15mm 10mm;
   }

   @page :right {
      margin: 15mm 10mm 15mm 20mm;
   }

   p, h2, h3 {
      orphans: 3; widows: 3;
   }

   h2, h3 {
      page-break-after: avoid;
   }
}
</style>




## Summary





All available functions which can be applied to this `res_list` object:


```r
res_list
```

```
#> A 'ConsensusPartitionList' object with 24 methods.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows are extracted by 'SD, CV, MAD, ATC' methods.
#>   Subgroups are detected by 'hclust, kmeans, skmeans, pam, mclust, NMF' method.
#>   Number of partitions are tried for k = 2, 3, 4, 5, 6.
#>   Performed in total 30000 partitions by row resampling.
#> 
#> Following methods can be applied to this 'ConsensusPartitionList' object:
#>  [1] "cola_report"           "collect_classes"       "collect_plots"         "collect_stats"        
#>  [5] "colnames"              "functional_enrichment" "get_anno_col"          "get_anno"             
#>  [9] "get_classes"           "get_matrix"            "get_membership"        "get_stats"            
#> [13] "is_best_k"             "is_stable_k"           "ncol"                  "nrow"                 
#> [17] "rownames"              "show"                  "suggest_best_k"        "test_to_known_factors"
#> [21] "top_rows_heatmap"      "top_rows_overlap"     
#> 
#> You can get result for a single method by, e.g. object["SD", "hclust"] or object["SD:hclust"]
#> or a subset of methods by object[c("SD", "CV")], c("hclust", "kmeans")]
```

The call of `run_all_consensus_partition_methods()` was:


```
#> run_all_consensus_partition_methods(data = mat, mc.cores = 4, anno = anno)
```

Dimension of the input matrix:


```r
mat = get_matrix(res_list)
dim(mat)
```

```
#> [1] 11993    50
```

### Density distribution

The density distribution for each sample is visualized as in one column in the
following heatmap. The clustering is based on the distance which is the
Kolmogorov-Smirnov statistic between two distributions.




```r
library(ComplexHeatmap)
densityHeatmap(mat, top_annotation = HeatmapAnnotation(df = get_anno(res_list), 
    col = get_anno_col(res_list)), ylab = "value", cluster_columns = TRUE, show_column_names = FALSE,
    mc.cores = 4)
```

![plot of chunk density-heatmap](figure_cola/density-heatmap-1.png)





### Suggest the best k



Folowing table shows the best `k` (number of partitions) for each combination
of top-value methods and partition methods. Clicking on the method name in
the table goes to the section for a single combination of methods.

[The cola vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13)
explains the definition of the metrics used for determining the best
number of partitions.


```r
suggest_best_k(res_list)
```


|                            | The best k| 1-PAC| Mean silhouette| Concordance|   |Optional k |
|:---------------------------|----------:|-----:|---------------:|-----------:|:--|:----------|
|[SD:skmeans](#SD-skmeans)   |          2| 1.000|           0.968|       0.987|** |           |
|[SD:NMF](#SD-NMF)           |          2| 1.000|           0.944|       0.978|** |           |
|[MAD:kmeans](#MAD-kmeans)   |          2| 1.000|           0.944|       0.977|** |           |
|[MAD:skmeans](#MAD-skmeans) |          3| 1.000|           0.968|       0.983|** |2          |
|[MAD:NMF](#MAD-NMF)         |          2| 1.000|           0.939|       0.976|** |           |
|[ATC:kmeans](#ATC-kmeans)   |          2| 1.000|           0.990|       0.995|** |           |
|[ATC:skmeans](#ATC-skmeans) |          2| 1.000|           0.973|       0.990|** |           |
|[ATC:NMF](#ATC-NMF)         |          2| 1.000|           0.951|       0.981|** |           |
|[MAD:pam](#MAD-pam)         |          2| 0.999|           0.956|       0.981|** |           |
|[SD:kmeans](#SD-kmeans)     |          2| 0.974|           0.948|       0.974|** |           |
|[CV:mclust](#CV-mclust)     |          3| 0.958|           0.928|       0.966|** |2          |
|[ATC:pam](#ATC-pam)         |          5| 0.946|           0.892|       0.933|*  |3          |
|[SD:pam](#SD-pam)           |          2| 0.916|           0.962|       0.982|*  |           |
|[MAD:mclust](#MAD-mclust)   |          3| 0.907|           0.898|       0.951|*  |2          |
|[SD:mclust](#SD-mclust)     |          3| 0.850|           0.898|       0.955|   |           |
|[CV:kmeans](#CV-kmeans)     |          2| 0.742|           0.893|       0.948|   |           |
|[CV:NMF](#CV-NMF)           |          2| 0.722|           0.845|       0.936|   |           |
|[CV:skmeans](#CV-skmeans)   |          2| 0.674|           0.845|       0.931|   |           |
|[MAD:hclust](#MAD-hclust)   |          4| 0.654|           0.840|       0.899|   |           |
|[CV:hclust](#CV-hclust)     |          3| 0.636|           0.828|       0.936|   |           |
|[SD:hclust](#SD-hclust)     |          4| 0.582|           0.742|       0.876|   |           |
|[ATC:mclust](#ATC-mclust)   |          2| 0.542|           0.911|       0.918|   |           |
|[ATC:hclust](#ATC-hclust)   |          3| 0.489|           0.769|       0.806|   |           |
|[CV:pam](#CV-pam)           |         NA|    NA|              NA|          NA|   |           |

\*\*: 1-PAC > 0.95, \*: 1-PAC > 0.9




### CDF of consensus matrices

Cumulative distribution function curves of consensus matrix for all methods.




```r
collect_plots(res_list, fun = plot_ecdf)
```

![plot of chunk collect-plots](figure_cola/collect-plots-1.png)



### Consensus heatmap

Consensus heatmaps for all methods. ([What is a consensus heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_9))


<style type='text/css'>



.ui-helper-hidden {
	display: none;
}
.ui-helper-hidden-accessible {
	border: 0;
	clip: rect(0 0 0 0);
	height: 1px;
	margin: -1px;
	overflow: hidden;
	padding: 0;
	position: absolute;
	width: 1px;
}
.ui-helper-reset {
	margin: 0;
	padding: 0;
	border: 0;
	outline: 0;
	line-height: 1.3;
	text-decoration: none;
	font-size: 100%;
	list-style: none;
}
.ui-helper-clearfix:before,
.ui-helper-clearfix:after {
	content: "";
	display: table;
	border-collapse: collapse;
}
.ui-helper-clearfix:after {
	clear: both;
}
.ui-helper-zfix {
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	position: absolute;
	opacity: 0;
	filter:Alpha(Opacity=0); 
}

.ui-front {
	z-index: 100;
}



.ui-state-disabled {
	cursor: default !important;
	pointer-events: none;
}



.ui-icon {
	display: inline-block;
	vertical-align: middle;
	margin-top: -.25em;
	position: relative;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
}

.ui-widget-icon-block {
	left: 50%;
	margin-left: -8px;
	display: block;
}




.ui-widget-overlay {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
}
.ui-accordion .ui-accordion-header {
	display: block;
	cursor: pointer;
	position: relative;
	margin: 2px 0 0 0;
	padding: .5em .5em .5em .7em;
	font-size: 100%;
}
.ui-accordion .ui-accordion-content {
	padding: 1em 2.2em;
	border-top: 0;
	overflow: auto;
}
.ui-autocomplete {
	position: absolute;
	top: 0;
	left: 0;
	cursor: default;
}
.ui-menu {
	list-style: none;
	padding: 0;
	margin: 0;
	display: block;
	outline: 0;
}
.ui-menu .ui-menu {
	position: absolute;
}
.ui-menu .ui-menu-item {
	margin: 0;
	cursor: pointer;
	
	list-style-image: url("data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7");
}
.ui-menu .ui-menu-item-wrapper {
	position: relative;
	padding: 3px 1em 3px .4em;
}
.ui-menu .ui-menu-divider {
	margin: 5px 0;
	height: 0;
	font-size: 0;
	line-height: 0;
	border-width: 1px 0 0 0;
}
.ui-menu .ui-state-focus,
.ui-menu .ui-state-active {
	margin: -1px;
}


.ui-menu-icons {
	position: relative;
}
.ui-menu-icons .ui-menu-item-wrapper {
	padding-left: 2em;
}


.ui-menu .ui-icon {
	position: absolute;
	top: 0;
	bottom: 0;
	left: .2em;
	margin: auto 0;
}


.ui-menu .ui-menu-icon {
	left: auto;
	right: 0;
}
.ui-button {
	padding: .4em 1em;
	display: inline-block;
	position: relative;
	line-height: normal;
	margin-right: .1em;
	cursor: pointer;
	vertical-align: middle;
	text-align: center;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;

	
	overflow: visible;
}

.ui-button,
.ui-button:link,
.ui-button:visited,
.ui-button:hover,
.ui-button:active {
	text-decoration: none;
}


.ui-button-icon-only {
	width: 2em;
	box-sizing: border-box;
	text-indent: -9999px;
	white-space: nowrap;
}


input.ui-button.ui-button-icon-only {
	text-indent: 0;
}


.ui-button-icon-only .ui-icon {
	position: absolute;
	top: 50%;
	left: 50%;
	margin-top: -8px;
	margin-left: -8px;
}

.ui-button.ui-icon-notext .ui-icon {
	padding: 0;
	width: 2.1em;
	height: 2.1em;
	text-indent: -9999px;
	white-space: nowrap;

}

input.ui-button.ui-icon-notext .ui-icon {
	width: auto;
	height: auto;
	text-indent: 0;
	white-space: normal;
	padding: .4em 1em;
}



input.ui-button::-moz-focus-inner,
button.ui-button::-moz-focus-inner {
	border: 0;
	padding: 0;
}
.ui-controlgroup {
	vertical-align: middle;
	display: inline-block;
}
.ui-controlgroup > .ui-controlgroup-item {
	float: left;
	margin-left: 0;
	margin-right: 0;
}
.ui-controlgroup > .ui-controlgroup-item:focus,
.ui-controlgroup > .ui-controlgroup-item.ui-visual-focus {
	z-index: 9999;
}
.ui-controlgroup-vertical > .ui-controlgroup-item {
	display: block;
	float: none;
	width: 100%;
	margin-top: 0;
	margin-bottom: 0;
	text-align: left;
}
.ui-controlgroup-vertical .ui-controlgroup-item {
	box-sizing: border-box;
}
.ui-controlgroup .ui-controlgroup-label {
	padding: .4em 1em;
}
.ui-controlgroup .ui-controlgroup-label span {
	font-size: 80%;
}
.ui-controlgroup-horizontal .ui-controlgroup-label + .ui-controlgroup-item {
	border-left: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label + .ui-controlgroup-item {
	border-top: none;
}
.ui-controlgroup-horizontal .ui-controlgroup-label.ui-widget-content {
	border-right: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label.ui-widget-content {
	border-bottom: none;
}


.ui-controlgroup-vertical .ui-spinner-input {

	
	width: 75%;
	width: calc( 100% - 2.4em );
}
.ui-controlgroup-vertical .ui-spinner .ui-spinner-up {
	border-top-style: solid;
}

.ui-checkboxradio-label .ui-icon-background {
	box-shadow: inset 1px 1px 1px #ccc;
	border-radius: .12em;
	border: none;
}
.ui-checkboxradio-radio-label .ui-icon-background {
	width: 16px;
	height: 16px;
	border-radius: 1em;
	overflow: visible;
	border: none;
}
.ui-checkboxradio-radio-label.ui-checkboxradio-checked .ui-icon,
.ui-checkboxradio-radio-label.ui-checkboxradio-checked:hover .ui-icon {
	background-image: none;
	width: 8px;
	height: 8px;
	border-width: 4px;
	border-style: solid;
}
.ui-checkboxradio-disabled {
	pointer-events: none;
}
.ui-datepicker {
	width: 17em;
	padding: .2em .2em 0;
	display: none;
}
.ui-datepicker .ui-datepicker-header {
	position: relative;
	padding: .2em 0;
}
.ui-datepicker .ui-datepicker-prev,
.ui-datepicker .ui-datepicker-next {
	position: absolute;
	top: 2px;
	width: 1.8em;
	height: 1.8em;
}
.ui-datepicker .ui-datepicker-prev-hover,
.ui-datepicker .ui-datepicker-next-hover {
	top: 1px;
}
.ui-datepicker .ui-datepicker-prev {
	left: 2px;
}
.ui-datepicker .ui-datepicker-next {
	right: 2px;
}
.ui-datepicker .ui-datepicker-prev-hover {
	left: 1px;
}
.ui-datepicker .ui-datepicker-next-hover {
	right: 1px;
}
.ui-datepicker .ui-datepicker-prev span,
.ui-datepicker .ui-datepicker-next span {
	display: block;
	position: absolute;
	left: 50%;
	margin-left: -8px;
	top: 50%;
	margin-top: -8px;
}
.ui-datepicker .ui-datepicker-title {
	margin: 0 2.3em;
	line-height: 1.8em;
	text-align: center;
}
.ui-datepicker .ui-datepicker-title select {
	font-size: 1em;
	margin: 1px 0;
}
.ui-datepicker select.ui-datepicker-month,
.ui-datepicker select.ui-datepicker-year {
	width: 45%;
}
.ui-datepicker table {
	width: 100%;
	font-size: .9em;
	border-collapse: collapse;
	margin: 0 0 .4em;
}
.ui-datepicker th {
	padding: .7em .3em;
	text-align: center;
	font-weight: bold;
	border: 0;
}
.ui-datepicker td {
	border: 0;
	padding: 1px;
}
.ui-datepicker td span,
.ui-datepicker td a {
	display: block;
	padding: .2em;
	text-align: right;
	text-decoration: none;
}
.ui-datepicker .ui-datepicker-buttonpane {
	background-image: none;
	margin: .7em 0 0 0;
	padding: 0 .2em;
	border-left: 0;
	border-right: 0;
	border-bottom: 0;
}
.ui-datepicker .ui-datepicker-buttonpane button {
	float: right;
	margin: .5em .2em .4em;
	cursor: pointer;
	padding: .2em .6em .3em .6em;
	width: auto;
	overflow: visible;
}
.ui-datepicker .ui-datepicker-buttonpane button.ui-datepicker-current {
	float: left;
}


.ui-datepicker.ui-datepicker-multi {
	width: auto;
}
.ui-datepicker-multi .ui-datepicker-group {
	float: left;
}
.ui-datepicker-multi .ui-datepicker-group table {
	width: 95%;
	margin: 0 auto .4em;
}
.ui-datepicker-multi-2 .ui-datepicker-group {
	width: 50%;
}
.ui-datepicker-multi-3 .ui-datepicker-group {
	width: 33.3%;
}
.ui-datepicker-multi-4 .ui-datepicker-group {
	width: 25%;
}
.ui-datepicker-multi .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-multi .ui-datepicker-group-middle .ui-datepicker-header {
	border-left-width: 0;
}
.ui-datepicker-multi .ui-datepicker-buttonpane {
	clear: left;
}
.ui-datepicker-row-break {
	clear: both;
	width: 100%;
	font-size: 0;
}


.ui-datepicker-rtl {
	direction: rtl;
}
.ui-datepicker-rtl .ui-datepicker-prev {
	right: 2px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next {
	left: 2px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-prev:hover {
	right: 1px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next:hover {
	left: 1px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane {
	clear: right;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button {
	float: left;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button.ui-datepicker-current,
.ui-datepicker-rtl .ui-datepicker-group {
	float: right;
}
.ui-datepicker-rtl .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-rtl .ui-datepicker-group-middle .ui-datepicker-header {
	border-right-width: 0;
	border-left-width: 1px;
}


.ui-datepicker .ui-icon {
	display: block;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
	left: .5em;
	top: .3em;
}
.ui-dialog {
	position: absolute;
	top: 0;
	left: 0;
	padding: .2em;
	outline: 0;
}
.ui-dialog .ui-dialog-titlebar {
	padding: .4em 1em;
	position: relative;
}
.ui-dialog .ui-dialog-title {
	float: left;
	margin: .1em 0;
	white-space: nowrap;
	width: 90%;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-dialog .ui-dialog-titlebar-close {
	position: absolute;
	right: .3em;
	top: 50%;
	width: 20px;
	margin: -10px 0 0 0;
	padding: 1px;
	height: 20px;
}
.ui-dialog .ui-dialog-content {
	position: relative;
	border: 0;
	padding: .5em 1em;
	background: none;
	overflow: auto;
}
.ui-dialog .ui-dialog-buttonpane {
	text-align: left;
	border-width: 1px 0 0 0;
	background-image: none;
	margin-top: .5em;
	padding: .3em 1em .5em .4em;
}
.ui-dialog .ui-dialog-buttonpane .ui-dialog-buttonset {
	float: right;
}
.ui-dialog .ui-dialog-buttonpane button {
	margin: .5em .4em .5em 0;
	cursor: pointer;
}
.ui-dialog .ui-resizable-n {
	height: 2px;
	top: 0;
}
.ui-dialog .ui-resizable-e {
	width: 2px;
	right: 0;
}
.ui-dialog .ui-resizable-s {
	height: 2px;
	bottom: 0;
}
.ui-dialog .ui-resizable-w {
	width: 2px;
	left: 0;
}
.ui-dialog .ui-resizable-se,
.ui-dialog .ui-resizable-sw,
.ui-dialog .ui-resizable-ne,
.ui-dialog .ui-resizable-nw {
	width: 7px;
	height: 7px;
}
.ui-dialog .ui-resizable-se {
	right: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-sw {
	left: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-ne {
	right: 0;
	top: 0;
}
.ui-dialog .ui-resizable-nw {
	left: 0;
	top: 0;
}
.ui-draggable .ui-dialog-titlebar {
	cursor: move;
}
.ui-draggable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable {
	position: relative;
}
.ui-resizable-handle {
	position: absolute;
	font-size: 0.1px;
	display: block;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable-disabled .ui-resizable-handle,
.ui-resizable-autohide .ui-resizable-handle {
	display: none;
}
.ui-resizable-n {
	cursor: n-resize;
	height: 7px;
	width: 100%;
	top: -5px;
	left: 0;
}
.ui-resizable-s {
	cursor: s-resize;
	height: 7px;
	width: 100%;
	bottom: -5px;
	left: 0;
}
.ui-resizable-e {
	cursor: e-resize;
	width: 7px;
	right: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-w {
	cursor: w-resize;
	width: 7px;
	left: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-se {
	cursor: se-resize;
	width: 12px;
	height: 12px;
	right: 1px;
	bottom: 1px;
}
.ui-resizable-sw {
	cursor: sw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	bottom: -5px;
}
.ui-resizable-nw {
	cursor: nw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	top: -5px;
}
.ui-resizable-ne {
	cursor: ne-resize;
	width: 9px;
	height: 9px;
	right: -5px;
	top: -5px;
}
.ui-progressbar {
	height: 2em;
	text-align: left;
	overflow: hidden;
}
.ui-progressbar .ui-progressbar-value {
	margin: -1px;
	height: 100%;
}
.ui-progressbar .ui-progressbar-overlay {
	background: url("data:image/gif;base64,R0lGODlhKAAoAIABAAAAAP///yH/C05FVFNDQVBFMi4wAwEAAAAh+QQJAQABACwAAAAAKAAoAAACkYwNqXrdC52DS06a7MFZI+4FHBCKoDeWKXqymPqGqxvJrXZbMx7Ttc+w9XgU2FB3lOyQRWET2IFGiU9m1frDVpxZZc6bfHwv4c1YXP6k1Vdy292Fb6UkuvFtXpvWSzA+HycXJHUXiGYIiMg2R6W459gnWGfHNdjIqDWVqemH2ekpObkpOlppWUqZiqr6edqqWQAAIfkECQEAAQAsAAAAACgAKAAAApSMgZnGfaqcg1E2uuzDmmHUBR8Qil95hiPKqWn3aqtLsS18y7G1SzNeowWBENtQd+T1JktP05nzPTdJZlR6vUxNWWjV+vUWhWNkWFwxl9VpZRedYcflIOLafaa28XdsH/ynlcc1uPVDZxQIR0K25+cICCmoqCe5mGhZOfeYSUh5yJcJyrkZWWpaR8doJ2o4NYq62lAAACH5BAkBAAEALAAAAAAoACgAAAKVDI4Yy22ZnINRNqosw0Bv7i1gyHUkFj7oSaWlu3ovC8GxNso5fluz3qLVhBVeT/Lz7ZTHyxL5dDalQWPVOsQWtRnuwXaFTj9jVVh8pma9JjZ4zYSj5ZOyma7uuolffh+IR5aW97cHuBUXKGKXlKjn+DiHWMcYJah4N0lYCMlJOXipGRr5qdgoSTrqWSq6WFl2ypoaUAAAIfkECQEAAQAsAAAAACgAKAAAApaEb6HLgd/iO7FNWtcFWe+ufODGjRfoiJ2akShbueb0wtI50zm02pbvwfWEMWBQ1zKGlLIhskiEPm9R6vRXxV4ZzWT2yHOGpWMyorblKlNp8HmHEb/lCXjcW7bmtXP8Xt229OVWR1fod2eWqNfHuMjXCPkIGNileOiImVmCOEmoSfn3yXlJWmoHGhqp6ilYuWYpmTqKUgAAIfkECQEAAQAsAAAAACgAKAAAApiEH6kb58biQ3FNWtMFWW3eNVcojuFGfqnZqSebuS06w5V80/X02pKe8zFwP6EFWOT1lDFk8rGERh1TTNOocQ61Hm4Xm2VexUHpzjymViHrFbiELsefVrn6XKfnt2Q9G/+Xdie499XHd2g4h7ioOGhXGJboGAnXSBnoBwKYyfioubZJ2Hn0RuRZaflZOil56Zp6iioKSXpUAAAh+QQJAQABACwAAAAAKAAoAAACkoQRqRvnxuI7kU1a1UU5bd5tnSeOZXhmn5lWK3qNTWvRdQxP8qvaC+/yaYQzXO7BMvaUEmJRd3TsiMAgswmNYrSgZdYrTX6tSHGZO73ezuAw2uxuQ+BbeZfMxsexY35+/Qe4J1inV0g4x3WHuMhIl2jXOKT2Q+VU5fgoSUI52VfZyfkJGkha6jmY+aaYdirq+lQAACH5BAkBAAEALAAAAAAoACgAAAKWBIKpYe0L3YNKToqswUlvznigd4wiR4KhZrKt9Upqip61i9E3vMvxRdHlbEFiEXfk9YARYxOZZD6VQ2pUunBmtRXo1Lf8hMVVcNl8JafV38aM2/Fu5V16Bn63r6xt97j09+MXSFi4BniGFae3hzbH9+hYBzkpuUh5aZmHuanZOZgIuvbGiNeomCnaxxap2upaCZsq+1kAACH5BAkBAAEALAAAAAAoACgAAAKXjI8By5zf4kOxTVrXNVlv1X0d8IGZGKLnNpYtm8Lr9cqVeuOSvfOW79D9aDHizNhDJidFZhNydEahOaDH6nomtJjp1tutKoNWkvA6JqfRVLHU/QUfau9l2x7G54d1fl995xcIGAdXqMfBNadoYrhH+Mg2KBlpVpbluCiXmMnZ2Sh4GBqJ+ckIOqqJ6LmKSllZmsoq6wpQAAAh+QQJAQABACwAAAAAKAAoAAAClYx/oLvoxuJDkU1a1YUZbJ59nSd2ZXhWqbRa2/gF8Gu2DY3iqs7yrq+xBYEkYvFSM8aSSObE+ZgRl1BHFZNr7pRCavZ5BW2142hY3AN/zWtsmf12p9XxxFl2lpLn1rseztfXZjdIWIf2s5dItwjYKBgo9yg5pHgzJXTEeGlZuenpyPmpGQoKOWkYmSpaSnqKileI2FAAACH5BAkBAAEALAAAAAAoACgAAAKVjB+gu+jG4kORTVrVhRlsnn2dJ3ZleFaptFrb+CXmO9OozeL5VfP99HvAWhpiUdcwkpBH3825AwYdU8xTqlLGhtCosArKMpvfa1mMRae9VvWZfeB2XfPkeLmm18lUcBj+p5dnN8jXZ3YIGEhYuOUn45aoCDkp16hl5IjYJvjWKcnoGQpqyPlpOhr3aElaqrq56Bq7VAAAOw==");
	height: 100%;
	filter: alpha(opacity=25); 
	opacity: 0.25;
}
.ui-progressbar-indeterminate .ui-progressbar-value {
	background-image: none;
}
.ui-selectable {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-selectable-helper {
	position: absolute;
	z-index: 100;
	border: 1px dotted black;
}
.ui-selectmenu-menu {
	padding: 0;
	margin: 0;
	position: absolute;
	top: 0;
	left: 0;
	display: none;
}
.ui-selectmenu-menu .ui-menu {
	overflow: auto;
	overflow-x: hidden;
	padding-bottom: 1px;
}
.ui-selectmenu-menu .ui-menu .ui-selectmenu-optgroup {
	font-size: 1em;
	font-weight: bold;
	line-height: 1.5;
	padding: 2px 0.4em;
	margin: 0.5em 0 0 0;
	height: auto;
	border: 0;
}
.ui-selectmenu-open {
	display: block;
}
.ui-selectmenu-text {
	display: block;
	margin-right: 20px;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-selectmenu-button.ui-button {
	text-align: left;
	white-space: nowrap;
	width: 14em;
}
.ui-selectmenu-icon.ui-icon {
	float: right;
	margin-top: 0;
}
.ui-slider {
	position: relative;
	text-align: left;
}
.ui-slider .ui-slider-handle {
	position: absolute;
	z-index: 2;
	width: 1.2em;
	height: 1.2em;
	cursor: default;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-slider .ui-slider-range {
	position: absolute;
	z-index: 1;
	font-size: .7em;
	display: block;
	border: 0;
	background-position: 0 0;
}


.ui-slider.ui-state-disabled .ui-slider-handle,
.ui-slider.ui-state-disabled .ui-slider-range {
	filter: inherit;
}

.ui-slider-horizontal {
	height: .8em;
}
.ui-slider-horizontal .ui-slider-handle {
	top: -.3em;
	margin-left: -.6em;
}
.ui-slider-horizontal .ui-slider-range {
	top: 0;
	height: 100%;
}
.ui-slider-horizontal .ui-slider-range-min {
	left: 0;
}
.ui-slider-horizontal .ui-slider-range-max {
	right: 0;
}

.ui-slider-vertical {
	width: .8em;
	height: 100px;
}
.ui-slider-vertical .ui-slider-handle {
	left: -.3em;
	margin-left: 0;
	margin-bottom: -.6em;
}
.ui-slider-vertical .ui-slider-range {
	left: 0;
	width: 100%;
}
.ui-slider-vertical .ui-slider-range-min {
	bottom: 0;
}
.ui-slider-vertical .ui-slider-range-max {
	top: 0;
}
.ui-sortable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-spinner {
	position: relative;
	display: inline-block;
	overflow: hidden;
	padding: 0;
	vertical-align: middle;
}
.ui-spinner-input {
	border: none;
	background: none;
	color: inherit;
	padding: .222em 0;
	margin: .2em 0;
	vertical-align: middle;
	margin-left: .4em;
	margin-right: 2em;
}
.ui-spinner-button {
	width: 1.6em;
	height: 50%;
	font-size: .5em;
	padding: 0;
	margin: 0;
	text-align: center;
	position: absolute;
	cursor: default;
	display: block;
	overflow: hidden;
	right: 0;
}

.ui-spinner a.ui-spinner-button {
	border-top-style: none;
	border-bottom-style: none;
	border-right-style: none;
}
.ui-spinner-up {
	top: 0;
}
.ui-spinner-down {
	bottom: 0;
}
.ui-tabs {
	position: relative;
	padding: .2em;
}
.ui-tabs .ui-tabs-nav {
	margin: 0;
	padding: .2em .2em 0;
}
.ui-tabs .ui-tabs-nav li {
	list-style: none;
	float: left;
	position: relative;
	top: 0;
	margin: 1px .2em 0 0;
	border-bottom-width: 0;
	padding: 0;
	white-space: nowrap;
}
.ui-tabs .ui-tabs-nav .ui-tabs-anchor {
	float: left;
	padding: .5em 1em;
	text-decoration: none;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active {
	margin-bottom: -1px;
	padding-bottom: 1px;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-state-disabled .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-tabs-loading .ui-tabs-anchor {
	cursor: text;
}
.ui-tabs-collapsible .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor {
	cursor: pointer;
}
.ui-tabs .ui-tabs-panel {
	display: block;
	border-width: 0;
	padding: 1em 1.4em;
	background: none;
}
.ui-tooltip {
	padding: 8px;
	position: absolute;
	z-index: 9999;
	max-width: 300px;
}
body .ui-tooltip {
	border-width: 2px;
}

.ui-widget {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget .ui-widget {
	font-size: 1em;
}
.ui-widget input,
.ui-widget select,
.ui-widget textarea,
.ui-widget button {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget.ui-widget-content {
	border: 1px solid #c5c5c5;
}
.ui-widget-content {
	border: 1px solid #dddddd;
	background: #ffffff;
	color: #333333;
}
.ui-widget-content a {
	color: #333333;
}
.ui-widget-header {
	border: 1px solid #dddddd;
	background: #e9e9e9;
	color: #333333;
	font-weight: bold;
}
.ui-widget-header a {
	color: #333333;
}


.ui-state-default,
.ui-widget-content .ui-state-default,
.ui-widget-header .ui-state-default,
.ui-button,


html .ui-button.ui-state-disabled:hover,
html .ui-button.ui-state-disabled:active {
	border: 1px solid #c5c5c5;
	background: #f6f6f6;
	font-weight: normal;
	color: #454545;
}
.ui-state-default a,
.ui-state-default a:link,
.ui-state-default a:visited,
a.ui-button,
a:link.ui-button,
a:visited.ui-button,
.ui-button {
	color: #454545;
	text-decoration: none;
}
.ui-state-hover,
.ui-widget-content .ui-state-hover,
.ui-widget-header .ui-state-hover,
.ui-state-focus,
.ui-widget-content .ui-state-focus,
.ui-widget-header .ui-state-focus,
.ui-button:hover,
.ui-button:focus {
	border: 1px solid #cccccc;
	background: #ededed;
	font-weight: normal;
	color: #2b2b2b;
}
.ui-state-hover a,
.ui-state-hover a:hover,
.ui-state-hover a:link,
.ui-state-hover a:visited,
.ui-state-focus a,
.ui-state-focus a:hover,
.ui-state-focus a:link,
.ui-state-focus a:visited,
a.ui-button:hover,
a.ui-button:focus {
	color: #2b2b2b;
	text-decoration: none;
}

.ui-visual-focus {
	box-shadow: 0 0 3px 1px rgb(94, 158, 214);
}
.ui-state-active,
.ui-widget-content .ui-state-active,
.ui-widget-header .ui-state-active,
a.ui-button:active,
.ui-button:active,
.ui-button.ui-state-active:hover {
	border: 1px solid #003eff;
	background: #007fff;
	font-weight: normal;
	color: #ffffff;
}
.ui-icon-background,
.ui-state-active .ui-icon-background {
	border: #003eff;
	background-color: #ffffff;
}
.ui-state-active a,
.ui-state-active a:link,
.ui-state-active a:visited {
	color: #ffffff;
	text-decoration: none;
}


.ui-state-highlight,
.ui-widget-content .ui-state-highlight,
.ui-widget-header .ui-state-highlight {
	border: 1px solid #dad55e;
	background: #fffa90;
	color: #777620;
}
.ui-state-checked {
	border: 1px solid #dad55e;
	background: #fffa90;
}
.ui-state-highlight a,
.ui-widget-content .ui-state-highlight a,
.ui-widget-header .ui-state-highlight a {
	color: #777620;
}
.ui-state-error,
.ui-widget-content .ui-state-error,
.ui-widget-header .ui-state-error {
	border: 1px solid #f1a899;
	background: #fddfdf;
	color: #5f3f3f;
}
.ui-state-error a,
.ui-widget-content .ui-state-error a,
.ui-widget-header .ui-state-error a {
	color: #5f3f3f;
}
.ui-state-error-text,
.ui-widget-content .ui-state-error-text,
.ui-widget-header .ui-state-error-text {
	color: #5f3f3f;
}
.ui-priority-primary,
.ui-widget-content .ui-priority-primary,
.ui-widget-header .ui-priority-primary {
	font-weight: bold;
}
.ui-priority-secondary,
.ui-widget-content .ui-priority-secondary,
.ui-widget-header .ui-priority-secondary {
	opacity: .7;
	filter:Alpha(Opacity=70); 
	font-weight: normal;
}
.ui-state-disabled,
.ui-widget-content .ui-state-disabled,
.ui-widget-header .ui-state-disabled {
	opacity: .35;
	filter:Alpha(Opacity=35); 
	background-image: none;
}
.ui-state-disabled .ui-icon {
	filter:Alpha(Opacity=35); 
}




.ui-icon {
	width: 16px;
	height: 16px;
}
.ui-icon,
.ui-widget-content .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-widget-header .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-state-hover .ui-icon,
.ui-state-focus .ui-icon,
.ui-button:hover .ui-icon,
.ui-button:focus .ui-icon {
	background-image: url("images/ui-icons_555555_256x240.png");
}
.ui-state-active .ui-icon,
.ui-button:active .ui-icon {
	background-image: url("images/ui-icons_ffffff_256x240.png");
}
.ui-state-highlight .ui-icon,
.ui-button .ui-state-highlight.ui-icon {
	background-image: url("images/ui-icons_777620_256x240.png");
}
.ui-state-error .ui-icon,
.ui-state-error-text .ui-icon {
	background-image: url("images/ui-icons_cc0000_256x240.png");
}
.ui-button .ui-icon {
	background-image: url("images/ui-icons_777777_256x240.png");
}


.ui-icon-blank { background-position: 16px 16px; }
.ui-icon-caret-1-n { background-position: 0 0; }
.ui-icon-caret-1-ne { background-position: -16px 0; }
.ui-icon-caret-1-e { background-position: -32px 0; }
.ui-icon-caret-1-se { background-position: -48px 0; }
.ui-icon-caret-1-s { background-position: -65px 0; }
.ui-icon-caret-1-sw { background-position: -80px 0; }
.ui-icon-caret-1-w { background-position: -96px 0; }
.ui-icon-caret-1-nw { background-position: -112px 0; }
.ui-icon-caret-2-n-s { background-position: -128px 0; }
.ui-icon-caret-2-e-w { background-position: -144px 0; }
.ui-icon-triangle-1-n { background-position: 0 -16px; }
.ui-icon-triangle-1-ne { background-position: -16px -16px; }
.ui-icon-triangle-1-e { background-position: -32px -16px; }
.ui-icon-triangle-1-se { background-position: -48px -16px; }
.ui-icon-triangle-1-s { background-position: -65px -16px; }
.ui-icon-triangle-1-sw { background-position: -80px -16px; }
.ui-icon-triangle-1-w { background-position: -96px -16px; }
.ui-icon-triangle-1-nw { background-position: -112px -16px; }
.ui-icon-triangle-2-n-s { background-position: -128px -16px; }
.ui-icon-triangle-2-e-w { background-position: -144px -16px; }
.ui-icon-arrow-1-n { background-position: 0 -32px; }
.ui-icon-arrow-1-ne { background-position: -16px -32px; }
.ui-icon-arrow-1-e { background-position: -32px -32px; }
.ui-icon-arrow-1-se { background-position: -48px -32px; }
.ui-icon-arrow-1-s { background-position: -65px -32px; }
.ui-icon-arrow-1-sw { background-position: -80px -32px; }
.ui-icon-arrow-1-w { background-position: -96px -32px; }
.ui-icon-arrow-1-nw { background-position: -112px -32px; }
.ui-icon-arrow-2-n-s { background-position: -128px -32px; }
.ui-icon-arrow-2-ne-sw { background-position: -144px -32px; }
.ui-icon-arrow-2-e-w { background-position: -160px -32px; }
.ui-icon-arrow-2-se-nw { background-position: -176px -32px; }
.ui-icon-arrowstop-1-n { background-position: -192px -32px; }
.ui-icon-arrowstop-1-e { background-position: -208px -32px; }
.ui-icon-arrowstop-1-s { background-position: -224px -32px; }
.ui-icon-arrowstop-1-w { background-position: -240px -32px; }
.ui-icon-arrowthick-1-n { background-position: 1px -48px; }
.ui-icon-arrowthick-1-ne { background-position: -16px -48px; }
.ui-icon-arrowthick-1-e { background-position: -32px -48px; }
.ui-icon-arrowthick-1-se { background-position: -48px -48px; }
.ui-icon-arrowthick-1-s { background-position: -64px -48px; }
.ui-icon-arrowthick-1-sw { background-position: -80px -48px; }
.ui-icon-arrowthick-1-w { background-position: -96px -48px; }
.ui-icon-arrowthick-1-nw { background-position: -112px -48px; }
.ui-icon-arrowthick-2-n-s { background-position: -128px -48px; }
.ui-icon-arrowthick-2-ne-sw { background-position: -144px -48px; }
.ui-icon-arrowthick-2-e-w { background-position: -160px -48px; }
.ui-icon-arrowthick-2-se-nw { background-position: -176px -48px; }
.ui-icon-arrowthickstop-1-n { background-position: -192px -48px; }
.ui-icon-arrowthickstop-1-e { background-position: -208px -48px; }
.ui-icon-arrowthickstop-1-s { background-position: -224px -48px; }
.ui-icon-arrowthickstop-1-w { background-position: -240px -48px; }
.ui-icon-arrowreturnthick-1-w { background-position: 0 -64px; }
.ui-icon-arrowreturnthick-1-n { background-position: -16px -64px; }
.ui-icon-arrowreturnthick-1-e { background-position: -32px -64px; }
.ui-icon-arrowreturnthick-1-s { background-position: -48px -64px; }
.ui-icon-arrowreturn-1-w { background-position: -64px -64px; }
.ui-icon-arrowreturn-1-n { background-position: -80px -64px; }
.ui-icon-arrowreturn-1-e { background-position: -96px -64px; }
.ui-icon-arrowreturn-1-s { background-position: -112px -64px; }
.ui-icon-arrowrefresh-1-w { background-position: -128px -64px; }
.ui-icon-arrowrefresh-1-n { background-position: -144px -64px; }
.ui-icon-arrowrefresh-1-e { background-position: -160px -64px; }
.ui-icon-arrowrefresh-1-s { background-position: -176px -64px; }
.ui-icon-arrow-4 { background-position: 0 -80px; }
.ui-icon-arrow-4-diag { background-position: -16px -80px; }
.ui-icon-extlink { background-position: -32px -80px; }
.ui-icon-newwin { background-position: -48px -80px; }
.ui-icon-refresh { background-position: -64px -80px; }
.ui-icon-shuffle { background-position: -80px -80px; }
.ui-icon-transfer-e-w { background-position: -96px -80px; }
.ui-icon-transferthick-e-w { background-position: -112px -80px; }
.ui-icon-folder-collapsed { background-position: 0 -96px; }
.ui-icon-folder-open { background-position: -16px -96px; }
.ui-icon-document { background-position: -32px -96px; }
.ui-icon-document-b { background-position: -48px -96px; }
.ui-icon-note { background-position: -64px -96px; }
.ui-icon-mail-closed { background-position: -80px -96px; }
.ui-icon-mail-open { background-position: -96px -96px; }
.ui-icon-suitcase { background-position: -112px -96px; }
.ui-icon-comment { background-position: -128px -96px; }
.ui-icon-person { background-position: -144px -96px; }
.ui-icon-print { background-position: -160px -96px; }
.ui-icon-trash { background-position: -176px -96px; }
.ui-icon-locked { background-position: -192px -96px; }
.ui-icon-unlocked { background-position: -208px -96px; }
.ui-icon-bookmark { background-position: -224px -96px; }
.ui-icon-tag { background-position: -240px -96px; }
.ui-icon-home { background-position: 0 -112px; }
.ui-icon-flag { background-position: -16px -112px; }
.ui-icon-calendar { background-position: -32px -112px; }
.ui-icon-cart { background-position: -48px -112px; }
.ui-icon-pencil { background-position: -64px -112px; }
.ui-icon-clock { background-position: -80px -112px; }
.ui-icon-disk { background-position: -96px -112px; }
.ui-icon-calculator { background-position: -112px -112px; }
.ui-icon-zoomin { background-position: -128px -112px; }
.ui-icon-zoomout { background-position: -144px -112px; }
.ui-icon-search { background-position: -160px -112px; }
.ui-icon-wrench { background-position: -176px -112px; }
.ui-icon-gear { background-position: -192px -112px; }
.ui-icon-heart { background-position: -208px -112px; }
.ui-icon-star { background-position: -224px -112px; }
.ui-icon-link { background-position: -240px -112px; }
.ui-icon-cancel { background-position: 0 -128px; }
.ui-icon-plus { background-position: -16px -128px; }
.ui-icon-plusthick { background-position: -32px -128px; }
.ui-icon-minus { background-position: -48px -128px; }
.ui-icon-minusthick { background-position: -64px -128px; }
.ui-icon-close { background-position: -80px -128px; }
.ui-icon-closethick { background-position: -96px -128px; }
.ui-icon-key { background-position: -112px -128px; }
.ui-icon-lightbulb { background-position: -128px -128px; }
.ui-icon-scissors { background-position: -144px -128px; }
.ui-icon-clipboard { background-position: -160px -128px; }
.ui-icon-copy { background-position: -176px -128px; }
.ui-icon-contact { background-position: -192px -128px; }
.ui-icon-image { background-position: -208px -128px; }
.ui-icon-video { background-position: -224px -128px; }
.ui-icon-script { background-position: -240px -128px; }
.ui-icon-alert { background-position: 0 -144px; }
.ui-icon-info { background-position: -16px -144px; }
.ui-icon-notice { background-position: -32px -144px; }
.ui-icon-help { background-position: -48px -144px; }
.ui-icon-check { background-position: -64px -144px; }
.ui-icon-bullet { background-position: -80px -144px; }
.ui-icon-radio-on { background-position: -96px -144px; }
.ui-icon-radio-off { background-position: -112px -144px; }
.ui-icon-pin-w { background-position: -128px -144px; }
.ui-icon-pin-s { background-position: -144px -144px; }
.ui-icon-play { background-position: 0 -160px; }
.ui-icon-pause { background-position: -16px -160px; }
.ui-icon-seek-next { background-position: -32px -160px; }
.ui-icon-seek-prev { background-position: -48px -160px; }
.ui-icon-seek-end { background-position: -64px -160px; }
.ui-icon-seek-start { background-position: -80px -160px; }

.ui-icon-seek-first { background-position: -80px -160px; }
.ui-icon-stop { background-position: -96px -160px; }
.ui-icon-eject { background-position: -112px -160px; }
.ui-icon-volume-off { background-position: -128px -160px; }
.ui-icon-volume-on { background-position: -144px -160px; }
.ui-icon-power { background-position: 0 -176px; }
.ui-icon-signal-diag { background-position: -16px -176px; }
.ui-icon-signal { background-position: -32px -176px; }
.ui-icon-battery-0 { background-position: -48px -176px; }
.ui-icon-battery-1 { background-position: -64px -176px; }
.ui-icon-battery-2 { background-position: -80px -176px; }
.ui-icon-battery-3 { background-position: -96px -176px; }
.ui-icon-circle-plus { background-position: 0 -192px; }
.ui-icon-circle-minus { background-position: -16px -192px; }
.ui-icon-circle-close { background-position: -32px -192px; }
.ui-icon-circle-triangle-e { background-position: -48px -192px; }
.ui-icon-circle-triangle-s { background-position: -64px -192px; }
.ui-icon-circle-triangle-w { background-position: -80px -192px; }
.ui-icon-circle-triangle-n { background-position: -96px -192px; }
.ui-icon-circle-arrow-e { background-position: -112px -192px; }
.ui-icon-circle-arrow-s { background-position: -128px -192px; }
.ui-icon-circle-arrow-w { background-position: -144px -192px; }
.ui-icon-circle-arrow-n { background-position: -160px -192px; }
.ui-icon-circle-zoomin { background-position: -176px -192px; }
.ui-icon-circle-zoomout { background-position: -192px -192px; }
.ui-icon-circle-check { background-position: -208px -192px; }
.ui-icon-circlesmall-plus { background-position: 0 -208px; }
.ui-icon-circlesmall-minus { background-position: -16px -208px; }
.ui-icon-circlesmall-close { background-position: -32px -208px; }
.ui-icon-squaresmall-plus { background-position: -48px -208px; }
.ui-icon-squaresmall-minus { background-position: -64px -208px; }
.ui-icon-squaresmall-close { background-position: -80px -208px; }
.ui-icon-grip-dotted-vertical { background-position: 0 -224px; }
.ui-icon-grip-dotted-horizontal { background-position: -16px -224px; }
.ui-icon-grip-solid-vertical { background-position: -32px -224px; }
.ui-icon-grip-solid-horizontal { background-position: -48px -224px; }
.ui-icon-gripsmall-diagonal-se { background-position: -64px -224px; }
.ui-icon-grip-diagonal-se { background-position: -80px -224px; }





.ui-corner-all,
.ui-corner-top,
.ui-corner-left,
.ui-corner-tl {
	border-top-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-top,
.ui-corner-right,
.ui-corner-tr {
	border-top-right-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-left,
.ui-corner-bl {
	border-bottom-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-right,
.ui-corner-br {
	border-bottom-right-radius: 3px;
}


.ui-widget-overlay {
	background: #aaaaaa;
	opacity: .3;
	filter: Alpha(Opacity=30); 
}
.ui-widget-shadow {
	-webkit-box-shadow: 0px 0px 5px #666666;
	box-shadow: 0px 0px 5px #666666;
} 
</style>
<script src='js/jquery-1.12.4.js'></script>
<script src='js/jquery-ui.js'></script>

<script>
$( function() {
	$( '#tabs-collect-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-consensus-heatmap'>
<ul>
<li><a href='#tab-collect-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-consensus-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-1-1.png" alt="plot of chunk tab-collect-consensus-heatmap-1"/></p>

</div>
<div id='tab-collect-consensus-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-2-1.png" alt="plot of chunk tab-collect-consensus-heatmap-2"/></p>

</div>
<div id='tab-collect-consensus-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-3-1.png" alt="plot of chunk tab-collect-consensus-heatmap-3"/></p>

</div>
<div id='tab-collect-consensus-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-4-1.png" alt="plot of chunk tab-collect-consensus-heatmap-4"/></p>

</div>
<div id='tab-collect-consensus-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-5-1.png" alt="plot of chunk tab-collect-consensus-heatmap-5"/></p>

</div>
</div>



### Membership heatmap

Membership heatmaps for all methods. ([What is a membership heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_12))


<script>
$( function() {
	$( '#tabs-collect-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-membership-heatmap'>
<ul>
<li><a href='#tab-collect-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-membership-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-1-1.png" alt="plot of chunk tab-collect-membership-heatmap-1"/></p>

</div>
<div id='tab-collect-membership-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-2-1.png" alt="plot of chunk tab-collect-membership-heatmap-2"/></p>

</div>
<div id='tab-collect-membership-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-3-1.png" alt="plot of chunk tab-collect-membership-heatmap-3"/></p>

</div>
<div id='tab-collect-membership-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-4-1.png" alt="plot of chunk tab-collect-membership-heatmap-4"/></p>

</div>
<div id='tab-collect-membership-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-5-1.png" alt="plot of chunk tab-collect-membership-heatmap-5"/></p>

</div>
</div>



### Signature heatmap

Signature heatmaps for all methods. ([What is a signature heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_22))


Note in following heatmaps, rows are scaled.



<script>
$( function() {
	$( '#tabs-collect-get-signatures' ).tabs();
} );
</script>
<div id='tabs-collect-get-signatures'>
<ul>
<li><a href='#tab-collect-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-collect-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-collect-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-collect-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-collect-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-collect-get-signatures-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-1-1.png" alt="plot of chunk tab-collect-get-signatures-1"/></p>

</div>
<div id='tab-collect-get-signatures-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-2-1.png" alt="plot of chunk tab-collect-get-signatures-2"/></p>

</div>
<div id='tab-collect-get-signatures-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-3-1.png" alt="plot of chunk tab-collect-get-signatures-3"/></p>

</div>
<div id='tab-collect-get-signatures-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-4-1.png" alt="plot of chunk tab-collect-get-signatures-4"/></p>

</div>
<div id='tab-collect-get-signatures-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-5-1.png" alt="plot of chunk tab-collect-get-signatures-5"/></p>

</div>
</div>



### Statistics table

The statistics used for measuring the stability of consensus partitioning.
([How are they
defined?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13))


<script>
$( function() {
	$( '#tabs-get-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-get-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-get-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-get-stats-from-consensus-partition-list-1'>
<pre><code class="r">get_stats(res_list, k = 2)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      2 1.000           0.944       0.978         0.4613 0.530   0.530
#&gt; CV:NMF      2 0.722           0.845       0.936         0.5039 0.490   0.490
#&gt; MAD:NMF     2 1.000           0.939       0.976         0.4660 0.530   0.530
#&gt; ATC:NMF     2 1.000           0.951       0.981         0.4307 0.571   0.571
#&gt; SD:skmeans  2 1.000           0.968       0.987         0.4850 0.510   0.510
#&gt; CV:skmeans  2 0.674           0.845       0.931         0.5086 0.490   0.490
#&gt; MAD:skmeans 2 0.916           0.960       0.980         0.4921 0.510   0.510
#&gt; ATC:skmeans 2 1.000           0.973       0.990         0.4856 0.519   0.519
#&gt; SD:mclust   2 0.711           0.864       0.927         0.3853 0.607   0.607
#&gt; CV:mclust   2 0.916           0.911       0.964         0.5030 0.493   0.493
#&gt; MAD:mclust  2 0.984           0.934       0.969         0.3704 0.628   0.628
#&gt; ATC:mclust  2 0.542           0.911       0.918         0.4484 0.542   0.542
#&gt; SD:kmeans   2 0.974           0.948       0.974         0.4498 0.542   0.542
#&gt; CV:kmeans   2 0.742           0.893       0.948         0.4724 0.542   0.542
#&gt; MAD:kmeans  2 1.000           0.944       0.977         0.4578 0.542   0.542
#&gt; ATC:kmeans  2 1.000           0.990       0.995         0.4478 0.556   0.556
#&gt; SD:pam      2 0.916           0.962       0.982         0.4548 0.542   0.542
#&gt; CV:pam      2 0.630           0.902       0.946         0.1156 0.960   0.960
#&gt; MAD:pam     2 0.999           0.956       0.981         0.4649 0.542   0.542
#&gt; ATC:pam     2 0.761           0.872       0.943         0.4812 0.497   0.497
#&gt; SD:hclust   2 0.613           0.863       0.934         0.3125 0.726   0.726
#&gt; CV:hclust   2 0.958           0.952       0.984         0.0582 0.960   0.960
#&gt; MAD:hclust  2 0.759           0.900       0.949         0.3101 0.726   0.726
#&gt; ATC:hclust  2 0.483           0.805       0.903         0.3220 0.754   0.754
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-2'>
<pre><code class="r">get_stats(res_list, k = 3)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      3 0.550           0.684       0.812         0.3717 0.776   0.602
#&gt; CV:NMF      3 0.661           0.776       0.896         0.3101 0.816   0.638
#&gt; MAD:NMF     3 0.545           0.652       0.794         0.3880 0.735   0.538
#&gt; ATC:NMF     3 0.475           0.650       0.795         0.4308 0.750   0.586
#&gt; SD:skmeans  3 0.837           0.917       0.948         0.3783 0.805   0.624
#&gt; CV:skmeans  3 0.364           0.603       0.785         0.3126 0.770   0.566
#&gt; MAD:skmeans 3 1.000           0.968       0.983         0.3604 0.805   0.624
#&gt; ATC:skmeans 3 0.819           0.826       0.907         0.3550 0.806   0.626
#&gt; SD:mclust   3 0.850           0.898       0.955         0.6485 0.634   0.455
#&gt; CV:mclust   3 0.958           0.928       0.966         0.1926 0.874   0.754
#&gt; MAD:mclust  3 0.907           0.898       0.951         0.7235 0.604   0.431
#&gt; ATC:mclust  3 0.414           0.697       0.808         0.2658 0.846   0.726
#&gt; SD:kmeans   3 0.469           0.534       0.667         0.3492 0.771   0.586
#&gt; CV:kmeans   3 0.557           0.664       0.811         0.2159 0.807   0.660
#&gt; MAD:kmeans  3 0.520           0.615       0.723         0.3647 0.791   0.620
#&gt; ATC:kmeans  3 0.488           0.649       0.707         0.3560 1.000   1.000
#&gt; SD:pam      3 0.652           0.772       0.833         0.2366 0.851   0.726
#&gt; CV:pam      3 0.512           0.799       0.913         0.4796 0.961   0.959
#&gt; MAD:pam     3 0.624           0.865       0.901         0.2984 0.868   0.756
#&gt; ATC:pam     3 0.920           0.891       0.950         0.2225 0.909   0.816
#&gt; SD:hclust   3 0.642           0.845       0.933         0.0716 0.990   0.987
#&gt; CV:hclust   3 0.636           0.828       0.936         2.2811 0.887   0.883
#&gt; MAD:hclust  3 0.779           0.886       0.949         0.0567 0.990   0.987
#&gt; ATC:hclust  3 0.489           0.769       0.806         0.6942 0.620   0.506
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-3'>
<pre><code class="r">get_stats(res_list, k = 4)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      4 0.784           0.799       0.909        0.15689 0.770   0.467
#&gt; CV:NMF      4 0.640           0.718       0.853        0.09609 0.881   0.685
#&gt; MAD:NMF     4 0.825           0.852       0.929        0.14081 0.747   0.415
#&gt; ATC:NMF     4 0.477           0.574       0.744        0.17255 0.784   0.499
#&gt; SD:skmeans  4 0.763           0.816       0.892        0.12912 0.892   0.687
#&gt; CV:skmeans  4 0.371           0.435       0.666        0.12172 0.902   0.732
#&gt; MAD:skmeans 4 0.774           0.830       0.907        0.12903 0.875   0.643
#&gt; ATC:skmeans 4 0.642           0.689       0.794        0.12127 0.847   0.585
#&gt; SD:mclust   4 0.682           0.756       0.888        0.03308 0.758   0.507
#&gt; CV:mclust   4 0.805           0.793       0.907        0.00479 0.724   0.496
#&gt; MAD:mclust  4 0.796           0.830       0.926        0.06395 0.766   0.512
#&gt; ATC:mclust  4 0.399           0.366       0.630        0.16197 0.860   0.709
#&gt; SD:kmeans   4 0.517           0.634       0.788        0.16148 0.740   0.411
#&gt; CV:kmeans   4 0.668           0.761       0.864        0.13069 0.865   0.681
#&gt; MAD:kmeans  4 0.608           0.739       0.842        0.14538 0.828   0.564
#&gt; ATC:kmeans  4 0.486           0.593       0.743        0.14822 0.684   0.463
#&gt; SD:pam      4 0.733           0.868       0.927        0.16890 0.901   0.764
#&gt; CV:pam      4 0.439           0.797       0.904        0.20818 0.962   0.958
#&gt; MAD:pam     4 0.796           0.874       0.934        0.12552 0.926   0.821
#&gt; ATC:pam     4 0.795           0.729       0.808        0.12139 0.957   0.894
#&gt; SD:hclust   4 0.582           0.742       0.876        0.83485 0.653   0.515
#&gt; CV:hclust   4 0.466           0.660       0.878        0.58475 0.928   0.915
#&gt; MAD:hclust  4 0.654           0.840       0.899        0.90941 0.647   0.507
#&gt; ATC:hclust  4 0.509           0.716       0.815        0.12586 0.993   0.984
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-4'>
<pre><code class="r">get_stats(res_list, k = 5)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      5 0.743           0.657       0.816         0.0793 0.909   0.676
#&gt; CV:NMF      5 0.644           0.558       0.768         0.0662 0.878   0.623
#&gt; MAD:NMF     5 0.736           0.696       0.831         0.0716 0.922   0.715
#&gt; ATC:NMF     5 0.707           0.647       0.827         0.0870 0.842   0.501
#&gt; SD:skmeans  5 0.671           0.556       0.720         0.0601 0.931   0.727
#&gt; CV:skmeans  5 0.414           0.303       0.587         0.0663 0.930   0.775
#&gt; MAD:skmeans 5 0.677           0.587       0.795         0.0605 0.969   0.873
#&gt; ATC:skmeans 5 0.649           0.576       0.758         0.0657 0.869   0.562
#&gt; SD:mclust   5 0.765           0.781       0.897         0.1070 0.907   0.755
#&gt; CV:mclust   5 0.838           0.783       0.846         0.1384 0.867   0.689
#&gt; MAD:mclust  5 0.792           0.751       0.882         0.0976 0.869   0.655
#&gt; ATC:mclust  5 0.482           0.455       0.692         0.1355 0.792   0.517
#&gt; SD:kmeans   5 0.606           0.619       0.780         0.0764 0.851   0.565
#&gt; CV:kmeans   5 0.673           0.645       0.788         0.0669 0.962   0.884
#&gt; MAD:kmeans  5 0.683           0.709       0.823         0.0691 0.892   0.654
#&gt; ATC:kmeans  5 0.572           0.702       0.780         0.0948 0.878   0.622
#&gt; SD:pam      5 0.739           0.781       0.874         0.0768 0.984   0.952
#&gt; CV:pam      5 0.488           0.840       0.922         0.1742 0.962   0.957
#&gt; MAD:pam     5 0.794           0.796       0.900         0.0610 0.811   0.529
#&gt; ATC:pam     5 0.946           0.892       0.933         0.1015 0.878   0.682
#&gt; SD:hclust   5 0.485           0.584       0.758         0.1257 0.962   0.898
#&gt; CV:hclust   5 0.355           0.659       0.863         0.2545 0.965   0.955
#&gt; MAD:hclust  5 0.525           0.676       0.807         0.1252 0.949   0.858
#&gt; ATC:hclust  5 0.557           0.705       0.801         0.1150 0.937   0.841
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-5'>
<pre><code class="r">get_stats(res_list, k = 6)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      6 0.801           0.777       0.858         0.0403 0.922   0.669
#&gt; CV:NMF      6 0.709           0.665       0.820         0.0462 0.932   0.727
#&gt; MAD:NMF     6 0.796           0.722       0.853         0.0409 0.945   0.756
#&gt; ATC:NMF     6 0.698           0.628       0.812         0.0449 0.885   0.557
#&gt; SD:skmeans  6 0.694           0.591       0.773         0.0430 0.900   0.567
#&gt; CV:skmeans  6 0.492           0.332       0.565         0.0406 0.909   0.680
#&gt; MAD:skmeans 6 0.700           0.592       0.765         0.0404 0.906   0.610
#&gt; ATC:skmeans 6 0.711           0.611       0.796         0.0451 0.936   0.704
#&gt; SD:mclust   6 0.760           0.772       0.884         0.0803 0.841   0.527
#&gt; CV:mclust   6 0.685           0.745       0.799         0.0842 0.951   0.851
#&gt; MAD:mclust  6 0.793           0.760       0.885         0.0803 0.844   0.499
#&gt; ATC:mclust  6 0.691           0.801       0.866         0.0590 0.845   0.501
#&gt; SD:kmeans   6 0.675           0.650       0.790         0.0454 0.959   0.848
#&gt; CV:kmeans   6 0.693           0.533       0.713         0.0572 0.896   0.695
#&gt; MAD:kmeans  6 0.692           0.578       0.759         0.0528 0.952   0.814
#&gt; ATC:kmeans  6 0.739           0.737       0.794         0.0573 1.000   1.000
#&gt; SD:pam      6 0.676           0.632       0.838         0.0668 0.961   0.880
#&gt; CV:pam      6 0.433           0.782       0.907         0.1871 0.963   0.957
#&gt; MAD:pam     6 0.759           0.719       0.850         0.0591 0.928   0.746
#&gt; ATC:pam     6 0.801           0.805       0.899         0.0734 0.945   0.805
#&gt; SD:hclust   6 0.543           0.563       0.745         0.1021 0.902   0.706
#&gt; CV:hclust   6 0.385           0.640       0.838         0.1009 0.999   0.999
#&gt; MAD:hclust  6 0.584           0.693       0.758         0.0956 0.907   0.706
#&gt; ATC:hclust  6 0.619           0.710       0.763         0.1321 0.860   0.588
</code></pre>

</div>
</div>

Following heatmap plots the partition for each combination of methods and the
lightness correspond to the silhouette scores for samples in each method. On
top the consensus subgroup is inferred from all methods by taking the mean
silhouette scores as weight.


<script>
$( function() {
	$( '#tabs-collect-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-stats-from-consensus-partition-list-1'>
<pre><code class="r">collect_stats(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-2'>
<pre><code class="r">collect_stats(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-3'>
<pre><code class="r">collect_stats(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-4'>
<pre><code class="r">collect_stats(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-5'>
<pre><code class="r">collect_stats(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-5"/></p>

</div>
</div>

### Partition from all methods



Collect partitions from all methods:


<script>
$( function() {
	$( '#tabs-collect-classes-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-classes-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-classes-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-classes-from-consensus-partition-list-1'>
<pre><code class="r">collect_classes(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-2'>
<pre><code class="r">collect_classes(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-3'>
<pre><code class="r">collect_classes(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-4'>
<pre><code class="r">collect_classes(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-5'>
<pre><code class="r">collect_classes(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-5"/></p>

</div>
</div>



### Top rows overlap


Overlap of top rows from different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-euler' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-euler'>
<ul>
<li><a href='#tab-top-rows-overlap-by-euler-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-euler-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-5"/></p>

</div>
</div>

Also visualize the correspondance of rankings between different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-correspondance' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-correspondance'>
<ul>
<li><a href='#tab-top-rows-overlap-by-correspondance-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-correspondance-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-5"/></p>

</div>
</div>


Heatmaps of the top rows:



<script>
$( function() {
	$( '#tabs-top-rows-heatmap' ).tabs();
} );
</script>
<div id='tabs-top-rows-heatmap'>
<ul>
<li><a href='#tab-top-rows-heatmap-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-heatmap-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-heatmap-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-heatmap-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-heatmap-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-heatmap-1'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 1000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-1-1.png" alt="plot of chunk tab-top-rows-heatmap-1"/></p>

</div>
<div id='tab-top-rows-heatmap-2'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 2000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-2-1.png" alt="plot of chunk tab-top-rows-heatmap-2"/></p>

</div>
<div id='tab-top-rows-heatmap-3'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 3000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-3-1.png" alt="plot of chunk tab-top-rows-heatmap-3"/></p>

</div>
<div id='tab-top-rows-heatmap-4'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 4000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-4-1.png" alt="plot of chunk tab-top-rows-heatmap-4"/></p>

</div>
<div id='tab-top-rows-heatmap-5'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 5000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-5-1.png" alt="plot of chunk tab-top-rows-heatmap-5"/></p>

</div>
</div>




### Test to known annotations



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.


<script>
$( function() {
	$( '#tabs-test-to-known-factors-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-test-to-known-factors-from-consensus-partition-list'>
<ul>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-test-to-known-factors-from-consensus-partition-list-1'>
<pre><code class="r">test_to_known_factors(res_list, k = 2)
</code></pre>

<pre><code>#&gt;              n disease.state(p) k
#&gt; SD:NMF      48            0.719 2
#&gt; CV:NMF      45            0.805 2
#&gt; MAD:NMF     48            0.696 2
#&gt; ATC:NMF     49            0.869 2
#&gt; SD:skmeans  49            0.553 2
#&gt; CV:skmeans  46            0.763 2
#&gt; MAD:skmeans 50            0.448 2
#&gt; ATC:skmeans 49            0.553 2
#&gt; SD:mclust   47            0.710 2
#&gt; CV:mclust   48            0.929 2
#&gt; MAD:mclust  48            0.694 2
#&gt; ATC:mclust  50            0.714 2
#&gt; SD:kmeans   49            0.706 2
#&gt; CV:kmeans   50            0.394 2
#&gt; MAD:kmeans  48            0.719 2
#&gt; ATC:kmeans  50            0.878 2
#&gt; SD:pam      50            0.438 2
#&gt; CV:pam      49               NA 2
#&gt; MAD:pam     50            0.438 2
#&gt; ATC:pam     46            0.639 2
#&gt; SD:hclust   47            0.561 2
#&gt; CV:hclust   49               NA 2
#&gt; MAD:hclust  48            0.528 2
#&gt; ATC:hclust  46            0.826 2
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-2'>
<pre><code class="r">test_to_known_factors(res_list, k = 3)
</code></pre>

<pre><code>#&gt;              n disease.state(p) k
#&gt; SD:NMF      45            0.625 3
#&gt; CV:NMF      44            0.966 3
#&gt; MAD:NMF     42            0.612 3
#&gt; ATC:NMF     42            0.568 3
#&gt; SD:skmeans  50            0.689 3
#&gt; CV:skmeans  38            0.838 3
#&gt; MAD:skmeans 50            0.689 3
#&gt; ATC:skmeans 48            0.632 3
#&gt; SD:mclust   48            0.750 3
#&gt; CV:mclust   50            0.447 3
#&gt; MAD:mclust  49            0.593 3
#&gt; ATC:mclust  45            0.687 3
#&gt; SD:kmeans   34            0.527 3
#&gt; CV:kmeans   37            0.733 3
#&gt; MAD:kmeans  44            0.823 3
#&gt; ATC:kmeans  45            0.806 3
#&gt; SD:pam      47            0.569 3
#&gt; CV:pam      46               NA 3
#&gt; MAD:pam     49            0.470 3
#&gt; ATC:pam     48            0.953 3
#&gt; SD:hclust   47            0.566 3
#&gt; CV:hclust   47            0.572 3
#&gt; MAD:hclust  48            0.539 3
#&gt; ATC:hclust  47            0.570 3
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-3'>
<pre><code class="r">test_to_known_factors(res_list, k = 4)
</code></pre>

<pre><code>#&gt;              n disease.state(p) k
#&gt; SD:NMF      47            0.901 4
#&gt; CV:NMF      44            0.880 4
#&gt; MAD:NMF     48            0.902 4
#&gt; ATC:NMF     38            0.798 4
#&gt; SD:skmeans  48            0.738 4
#&gt; CV:skmeans  24            0.506 4
#&gt; MAD:skmeans 48            0.738 4
#&gt; ATC:skmeans 46            0.931 4
#&gt; SD:mclust   43            0.918 4
#&gt; CV:mclust   44            0.519 4
#&gt; MAD:mclust  45            0.877 4
#&gt; ATC:mclust  19               NA 4
#&gt; SD:kmeans   42            0.928 4
#&gt; CV:kmeans   40            0.519 4
#&gt; MAD:kmeans  45            0.878 4
#&gt; ATC:kmeans  37            0.924 4
#&gt; SD:pam      50            0.602 4
#&gt; CV:pam      46               NA 4
#&gt; MAD:pam     48            0.671 4
#&gt; ATC:pam     41            0.977 4
#&gt; SD:hclust   45            0.417 4
#&gt; CV:hclust   40            0.224 4
#&gt; MAD:hclust  48            0.584 4
#&gt; ATC:hclust  43            0.406 4
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-4'>
<pre><code class="r">test_to_known_factors(res_list, k = 5)
</code></pre>

<pre><code>#&gt;              n disease.state(p) k
#&gt; SD:NMF      39           0.7748 5
#&gt; CV:NMF      30           0.4768 5
#&gt; MAD:NMF     39           0.8458 5
#&gt; ATC:NMF     39           0.7052 5
#&gt; SD:skmeans  36           0.8138 5
#&gt; CV:skmeans   9           0.0842 5
#&gt; MAD:skmeans 36           0.8565 5
#&gt; ATC:skmeans 32           0.8479 5
#&gt; SD:mclust   46           0.8550 5
#&gt; CV:mclust   45           0.6888 5
#&gt; MAD:mclust  41           0.7390 5
#&gt; ATC:mclust  24           0.8270 5
#&gt; SD:kmeans   35           0.9431 5
#&gt; CV:kmeans   42           0.7116 5
#&gt; MAD:kmeans  40           0.4737 5
#&gt; ATC:kmeans  44           0.9073 5
#&gt; SD:pam      43           0.7128 5
#&gt; CV:pam      46               NA 5
#&gt; MAD:pam     47           0.6629 5
#&gt; ATC:pam     49           0.8744 5
#&gt; SD:hclust   38           0.6820 5
#&gt; CV:hclust   42           0.1964 5
#&gt; MAD:hclust  45           0.5525 5
#&gt; ATC:hclust  43           0.7721 5
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-5'>
<pre><code class="r">test_to_known_factors(res_list, k = 6)
</code></pre>

<pre><code>#&gt;              n disease.state(p) k
#&gt; SD:NMF      42           0.5815 6
#&gt; CV:NMF      40           0.3800 6
#&gt; MAD:NMF     41           0.6382 6
#&gt; ATC:NMF     35           0.7628 6
#&gt; SD:skmeans  39           0.6798 6
#&gt; CV:skmeans  11           0.0601 6
#&gt; MAD:skmeans 35           0.7834 6
#&gt; ATC:skmeans 37           0.7651 6
#&gt; SD:mclust   46           0.7006 6
#&gt; CV:mclust   40           0.5705 6
#&gt; MAD:mclust  45           0.4268 6
#&gt; ATC:mclust  50           0.6564 6
#&gt; SD:kmeans   40           0.8272 6
#&gt; CV:kmeans   30           0.6118 6
#&gt; MAD:kmeans  35           0.8689 6
#&gt; ATC:kmeans  48           0.8439 6
#&gt; SD:pam      37           0.6702 6
#&gt; CV:pam      45               NA 6
#&gt; MAD:pam     41           0.7202 6
#&gt; ATC:pam     49           0.4539 6
#&gt; SD:hclust   35           0.4084 6
#&gt; CV:hclust   39               NA 6
#&gt; MAD:hclust  43           0.4561 6
#&gt; ATC:hclust  45           0.7071 6
</code></pre>

</div>
</div>


 
## Results for each method


---------------------------------------------------




### SD:hclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "hclust"]
# you can also extract it by
# res = res_list["SD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-hclust-collect-plots](figure_cola/SD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-hclust-select-partition-number](figure_cola/SD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.613           0.863       0.934         0.3125 0.726   0.726
#> 3 3 0.642           0.845       0.933         0.0716 0.990   0.987
#> 4 4 0.582           0.742       0.876         0.8349 0.653   0.515
#> 5 5 0.485           0.584       0.758         0.1257 0.962   0.898
#> 6 6 0.543           0.563       0.745         0.1021 0.902   0.706
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-classes'>
<ul>
<li><a href='#tab-SD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-hclust-get-classes-1'>
<p><a id='tab-SD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     1  0.0000      0.930 1.000 0.000
#&gt; GSM317652     1  0.0000      0.930 1.000 0.000
#&gt; GSM317666     1  0.4939      0.875 0.892 0.108
#&gt; GSM317672     1  0.9491      0.471 0.632 0.368
#&gt; GSM317679     1  0.0000      0.930 1.000 0.000
#&gt; GSM317681     1  0.5519      0.860 0.872 0.128
#&gt; GSM317682     1  0.0000      0.930 1.000 0.000
#&gt; GSM317683     2  0.0938      0.907 0.012 0.988
#&gt; GSM317689     1  0.9922      0.243 0.552 0.448
#&gt; GSM317691     1  0.2603      0.914 0.956 0.044
#&gt; GSM317692     1  0.5737      0.831 0.864 0.136
#&gt; GSM317693     1  0.1414      0.925 0.980 0.020
#&gt; GSM317696     1  0.0000      0.930 1.000 0.000
#&gt; GSM317697     1  0.0938      0.927 0.988 0.012
#&gt; GSM317698     1  0.0000      0.930 1.000 0.000
#&gt; GSM317650     2  0.0938      0.907 0.012 0.988
#&gt; GSM317651     1  0.0000      0.930 1.000 0.000
#&gt; GSM317657     1  0.5842      0.848 0.860 0.140
#&gt; GSM317667     2  0.0000      0.901 0.000 1.000
#&gt; GSM317670     1  0.7219      0.783 0.800 0.200
#&gt; GSM317674     1  0.0000      0.930 1.000 0.000
#&gt; GSM317675     1  0.0000      0.930 1.000 0.000
#&gt; GSM317677     1  0.4161      0.890 0.916 0.084
#&gt; GSM317678     2  0.6148      0.782 0.152 0.848
#&gt; GSM317687     1  0.1184      0.927 0.984 0.016
#&gt; GSM317695     1  0.0000      0.930 1.000 0.000
#&gt; GSM317653     1  0.2778      0.914 0.952 0.048
#&gt; GSM317656     1  0.0000      0.930 1.000 0.000
#&gt; GSM317658     1  0.8661      0.647 0.712 0.288
#&gt; GSM317660     1  0.0938      0.926 0.988 0.012
#&gt; GSM317663     1  0.5842      0.848 0.860 0.140
#&gt; GSM317664     1  0.0000      0.930 1.000 0.000
#&gt; GSM317665     1  0.0376      0.929 0.996 0.004
#&gt; GSM317673     1  0.0000      0.930 1.000 0.000
#&gt; GSM317686     2  0.0000      0.901 0.000 1.000
#&gt; GSM317688     1  0.0000      0.930 1.000 0.000
#&gt; GSM317690     2  0.9710      0.239 0.400 0.600
#&gt; GSM317654     1  0.0376      0.929 0.996 0.004
#&gt; GSM317655     1  0.7883      0.738 0.764 0.236
#&gt; GSM317659     1  0.4690      0.880 0.900 0.100
#&gt; GSM317661     2  0.1184      0.906 0.016 0.984
#&gt; GSM317662     2  0.0938      0.907 0.012 0.988
#&gt; GSM317668     1  0.0000      0.930 1.000 0.000
#&gt; GSM317669     1  0.0000      0.930 1.000 0.000
#&gt; GSM317671     1  0.0000      0.930 1.000 0.000
#&gt; GSM317676     1  0.4690      0.880 0.900 0.100
#&gt; GSM317680     1  0.0000      0.930 1.000 0.000
#&gt; GSM317684     1  0.1184      0.927 0.984 0.016
#&gt; GSM317685     1  0.0000      0.930 1.000 0.000
#&gt; GSM317694     1  0.1414      0.925 0.980 0.020
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-1-a').click(function(){
  $('#tab-SD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-2'>
<p><a id='tab-SD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM317649     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317652     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317666     1  0.3116      0.876 0.892 0.000 0.108
#&gt; GSM317672     1  0.6008      0.430 0.628 0.372 0.000
#&gt; GSM317679     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317681     1  0.3851      0.842 0.860 0.136 0.004
#&gt; GSM317682     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317683     2  0.0000      0.723 0.000 1.000 0.000
#&gt; GSM317689     1  0.7013      0.201 0.548 0.432 0.020
#&gt; GSM317691     1  0.1950      0.911 0.952 0.040 0.008
#&gt; GSM317692     1  0.3851      0.824 0.860 0.136 0.004
#&gt; GSM317693     1  0.1129      0.923 0.976 0.020 0.004
#&gt; GSM317696     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317697     1  0.0747      0.924 0.984 0.016 0.000
#&gt; GSM317698     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317650     2  0.0000      0.723 0.000 1.000 0.000
#&gt; GSM317651     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317657     1  0.4565      0.852 0.860 0.064 0.076
#&gt; GSM317667     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM317670     1  0.5744      0.788 0.800 0.128 0.072
#&gt; GSM317674     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317675     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317677     1  0.2625      0.892 0.916 0.000 0.084
#&gt; GSM317678     2  0.3752      0.588 0.144 0.856 0.000
#&gt; GSM317687     1  0.0747      0.926 0.984 0.000 0.016
#&gt; GSM317695     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317653     1  0.2434      0.909 0.940 0.024 0.036
#&gt; GSM317656     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317658     1  0.5497      0.618 0.708 0.292 0.000
#&gt; GSM317660     1  0.1031      0.921 0.976 0.024 0.000
#&gt; GSM317663     1  0.4544      0.853 0.860 0.056 0.084
#&gt; GSM317664     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317665     1  0.0237      0.929 0.996 0.000 0.004
#&gt; GSM317673     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317686     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM317688     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317690     2  0.8097      0.228 0.388 0.540 0.072
#&gt; GSM317654     1  0.0237      0.929 0.996 0.000 0.004
#&gt; GSM317655     1  0.6510      0.736 0.756 0.156 0.088
#&gt; GSM317659     1  0.2959      0.882 0.900 0.000 0.100
#&gt; GSM317661     2  0.0237      0.722 0.004 0.996 0.000
#&gt; GSM317662     2  0.0000      0.723 0.000 1.000 0.000
#&gt; GSM317668     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317669     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317671     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317676     1  0.2959      0.882 0.900 0.000 0.100
#&gt; GSM317680     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317684     1  0.0747      0.926 0.984 0.000 0.016
#&gt; GSM317685     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317694     1  0.0892      0.925 0.980 0.000 0.020
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-2-a').click(function(){
  $('#tab-SD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-3'>
<p><a id='tab-SD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     1  0.0000     0.9359 1.000 0.000 0.000 0.000
#&gt; GSM317652     1  0.0000     0.9359 1.000 0.000 0.000 0.000
#&gt; GSM317666     4  0.0336     0.6383 0.000 0.000 0.008 0.992
#&gt; GSM317672     4  0.7235     0.3036 0.148 0.372 0.000 0.480
#&gt; GSM317679     1  0.0000     0.9359 1.000 0.000 0.000 0.000
#&gt; GSM317681     1  0.4586     0.7268 0.796 0.136 0.000 0.068
#&gt; GSM317682     1  0.0188     0.9347 0.996 0.000 0.000 0.004
#&gt; GSM317683     2  0.0000     0.8150 0.000 1.000 0.000 0.000
#&gt; GSM317689     4  0.6187     0.1822 0.052 0.432 0.000 0.516
#&gt; GSM317691     4  0.5512     0.6119 0.300 0.040 0.000 0.660
#&gt; GSM317692     4  0.7033     0.5066 0.336 0.136 0.000 0.528
#&gt; GSM317693     1  0.5581    -0.1490 0.532 0.020 0.000 0.448
#&gt; GSM317696     1  0.0469     0.9335 0.988 0.000 0.000 0.012
#&gt; GSM317697     1  0.4175     0.6586 0.784 0.016 0.000 0.200
#&gt; GSM317698     1  0.0592     0.9311 0.984 0.000 0.000 0.016
#&gt; GSM317650     2  0.0000     0.8150 0.000 1.000 0.000 0.000
#&gt; GSM317651     1  0.0188     0.9347 0.996 0.000 0.000 0.004
#&gt; GSM317657     4  0.2179     0.6436 0.012 0.064 0.000 0.924
#&gt; GSM317667     3  0.0000     1.0000 0.000 0.000 1.000 0.000
#&gt; GSM317670     4  0.3749     0.6173 0.032 0.128 0.000 0.840
#&gt; GSM317674     1  0.0592     0.9311 0.984 0.000 0.000 0.016
#&gt; GSM317675     1  0.0592     0.9311 0.984 0.000 0.000 0.016
#&gt; GSM317677     4  0.4008     0.6219 0.244 0.000 0.000 0.756
#&gt; GSM317678     2  0.3667     0.6917 0.088 0.856 0.000 0.056
#&gt; GSM317687     4  0.3528     0.6773 0.192 0.000 0.000 0.808
#&gt; GSM317695     1  0.0000     0.9359 1.000 0.000 0.000 0.000
#&gt; GSM317653     1  0.4095     0.7477 0.804 0.024 0.000 0.172
#&gt; GSM317656     1  0.0000     0.9359 1.000 0.000 0.000 0.000
#&gt; GSM317658     4  0.7397     0.4324 0.200 0.292 0.000 0.508
#&gt; GSM317660     1  0.1629     0.9077 0.952 0.024 0.000 0.024
#&gt; GSM317663     4  0.2076     0.6389 0.004 0.056 0.008 0.932
#&gt; GSM317664     1  0.0592     0.9311 0.984 0.000 0.000 0.016
#&gt; GSM317665     1  0.0921     0.9219 0.972 0.000 0.000 0.028
#&gt; GSM317673     1  0.0469     0.9335 0.988 0.000 0.000 0.012
#&gt; GSM317686     3  0.0000     1.0000 0.000 0.000 1.000 0.000
#&gt; GSM317688     1  0.0000     0.9359 1.000 0.000 0.000 0.000
#&gt; GSM317690     2  0.4977     0.0679 0.000 0.540 0.000 0.460
#&gt; GSM317654     1  0.1022     0.9193 0.968 0.000 0.000 0.032
#&gt; GSM317655     4  0.3450     0.5842 0.000 0.156 0.008 0.836
#&gt; GSM317659     4  0.1557     0.6677 0.056 0.000 0.000 0.944
#&gt; GSM317661     2  0.0188     0.8136 0.000 0.996 0.000 0.004
#&gt; GSM317662     2  0.0000     0.8150 0.000 1.000 0.000 0.000
#&gt; GSM317668     4  0.4661     0.5743 0.348 0.000 0.000 0.652
#&gt; GSM317669     1  0.0000     0.9359 1.000 0.000 0.000 0.000
#&gt; GSM317671     1  0.0000     0.9359 1.000 0.000 0.000 0.000
#&gt; GSM317676     4  0.1557     0.6677 0.056 0.000 0.000 0.944
#&gt; GSM317680     1  0.0000     0.9359 1.000 0.000 0.000 0.000
#&gt; GSM317684     4  0.3764     0.6686 0.216 0.000 0.000 0.784
#&gt; GSM317685     1  0.0188     0.9347 0.996 0.000 0.000 0.004
#&gt; GSM317694     4  0.4790     0.5047 0.380 0.000 0.000 0.620
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-3-a').click(function(){
  $('#tab-SD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-4'>
<p><a id='tab-SD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     1  0.3074     0.6876 0.804 0.000 0.000 0.000 0.196
#&gt; GSM317652     1  0.3177     0.6810 0.792 0.000 0.000 0.000 0.208
#&gt; GSM317666     4  0.1282     0.6411 0.000 0.000 0.004 0.952 0.044
#&gt; GSM317672     4  0.7455     0.2666 0.104 0.332 0.000 0.456 0.108
#&gt; GSM317679     1  0.3534     0.6369 0.744 0.000 0.000 0.000 0.256
#&gt; GSM317681     5  0.6387     0.8310 0.396 0.080 0.000 0.032 0.492
#&gt; GSM317682     1  0.1410     0.6781 0.940 0.000 0.000 0.000 0.060
#&gt; GSM317683     2  0.0000     0.7943 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317689     4  0.6476     0.1702 0.020 0.384 0.000 0.484 0.112
#&gt; GSM317691     4  0.5449     0.4895 0.264 0.000 0.000 0.632 0.104
#&gt; GSM317692     4  0.7270     0.3710 0.292 0.096 0.000 0.504 0.108
#&gt; GSM317693     1  0.5631    -0.2493 0.500 0.000 0.000 0.424 0.076
#&gt; GSM317696     1  0.0693     0.6987 0.980 0.000 0.000 0.008 0.012
#&gt; GSM317697     1  0.4409     0.2909 0.752 0.000 0.000 0.176 0.072
#&gt; GSM317698     1  0.0798     0.7010 0.976 0.000 0.000 0.016 0.008
#&gt; GSM317650     2  0.0000     0.7943 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317651     1  0.1544     0.6705 0.932 0.000 0.000 0.000 0.068
#&gt; GSM317657     4  0.2438     0.6397 0.008 0.040 0.000 0.908 0.044
#&gt; GSM317667     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317670     4  0.4245     0.5857 0.024 0.008 0.000 0.744 0.224
#&gt; GSM317674     1  0.0798     0.7010 0.976 0.000 0.000 0.016 0.008
#&gt; GSM317675     1  0.0798     0.7010 0.976 0.000 0.000 0.016 0.008
#&gt; GSM317677     4  0.4873     0.4967 0.244 0.000 0.000 0.688 0.068
#&gt; GSM317678     2  0.3930     0.6919 0.044 0.824 0.000 0.028 0.104
#&gt; GSM317687     4  0.4170     0.6015 0.192 0.000 0.000 0.760 0.048
#&gt; GSM317695     1  0.3534     0.6369 0.744 0.000 0.000 0.000 0.256
#&gt; GSM317653     5  0.6059     0.8264 0.412 0.000 0.000 0.120 0.468
#&gt; GSM317656     1  0.3074     0.6876 0.804 0.000 0.000 0.000 0.196
#&gt; GSM317658     4  0.7613     0.4015 0.164 0.252 0.000 0.484 0.100
#&gt; GSM317660     1  0.4161    -0.3573 0.608 0.000 0.000 0.000 0.392
#&gt; GSM317663     4  0.2074     0.6392 0.004 0.032 0.004 0.928 0.032
#&gt; GSM317664     1  0.0798     0.7010 0.976 0.000 0.000 0.016 0.008
#&gt; GSM317665     1  0.3724     0.6581 0.788 0.000 0.000 0.028 0.184
#&gt; GSM317673     1  0.0992     0.6979 0.968 0.000 0.000 0.008 0.024
#&gt; GSM317686     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317688     1  0.0404     0.7042 0.988 0.000 0.000 0.000 0.012
#&gt; GSM317690     2  0.6646     0.0721 0.000 0.416 0.000 0.356 0.228
#&gt; GSM317654     1  0.3805     0.6548 0.784 0.000 0.000 0.032 0.184
#&gt; GSM317655     4  0.3817     0.5636 0.000 0.004 0.004 0.740 0.252
#&gt; GSM317659     4  0.2859     0.6342 0.056 0.000 0.000 0.876 0.068
#&gt; GSM317661     2  0.1965     0.7563 0.000 0.904 0.000 0.000 0.096
#&gt; GSM317662     2  0.0000     0.7943 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317668     4  0.5049     0.4127 0.296 0.000 0.000 0.644 0.060
#&gt; GSM317669     1  0.3534     0.6369 0.744 0.000 0.000 0.000 0.256
#&gt; GSM317671     1  0.3534     0.6369 0.744 0.000 0.000 0.000 0.256
#&gt; GSM317676     4  0.2859     0.6342 0.056 0.000 0.000 0.876 0.068
#&gt; GSM317680     1  0.3534     0.6369 0.744 0.000 0.000 0.000 0.256
#&gt; GSM317684     4  0.4364     0.5801 0.216 0.000 0.000 0.736 0.048
#&gt; GSM317685     1  0.1410     0.6781 0.940 0.000 0.000 0.000 0.060
#&gt; GSM317694     4  0.5037     0.3198 0.376 0.000 0.000 0.584 0.040
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-4-a').click(function(){
  $('#tab-SD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-5'>
<p><a id='tab-SD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM317649     1  0.4739     -0.157 0.516 0.000 0.436 0.000 0.048  0
#&gt; GSM317652     3  0.4756      0.186 0.464 0.000 0.488 0.000 0.048  0
#&gt; GSM317666     4  0.1082      0.626 0.000 0.000 0.004 0.956 0.040  0
#&gt; GSM317672     4  0.7551      0.214 0.088 0.324 0.040 0.404 0.144  0
#&gt; GSM317679     3  0.2793      0.891 0.200 0.000 0.800 0.000 0.000  0
#&gt; GSM317681     5  0.4864      0.698 0.224 0.076 0.000 0.020 0.680  0
#&gt; GSM317682     1  0.2009      0.653 0.908 0.000 0.024 0.000 0.068  0
#&gt; GSM317683     2  0.0000      0.782 0.000 1.000 0.000 0.000 0.000  0
#&gt; GSM317689     4  0.6816      0.144 0.024 0.376 0.040 0.424 0.136  0
#&gt; GSM317691     4  0.5718      0.492 0.276 0.000 0.036 0.584 0.104  0
#&gt; GSM317692     4  0.7357      0.381 0.300 0.092 0.040 0.452 0.116  0
#&gt; GSM317693     1  0.5708     -0.176 0.512 0.000 0.032 0.376 0.080  0
#&gt; GSM317696     1  0.0436      0.688 0.988 0.000 0.004 0.004 0.004  0
#&gt; GSM317697     1  0.4262      0.402 0.760 0.000 0.024 0.148 0.068  0
#&gt; GSM317698     1  0.0603      0.690 0.980 0.000 0.004 0.016 0.000  0
#&gt; GSM317650     2  0.0790      0.784 0.000 0.968 0.032 0.000 0.000  0
#&gt; GSM317651     1  0.2201      0.644 0.896 0.000 0.028 0.000 0.076  0
#&gt; GSM317657     4  0.2860      0.626 0.008 0.040 0.012 0.876 0.064  0
#&gt; GSM317667     6  0.0000      1.000 0.000 0.000 0.000 0.000 0.000  1
#&gt; GSM317670     4  0.4928      0.540 0.012 0.004 0.076 0.668 0.240  0
#&gt; GSM317674     1  0.0603      0.690 0.980 0.000 0.004 0.016 0.000  0
#&gt; GSM317675     1  0.0603      0.690 0.980 0.000 0.004 0.016 0.000  0
#&gt; GSM317677     4  0.4586      0.508 0.240 0.000 0.012 0.688 0.060  0
#&gt; GSM317678     2  0.3662      0.676 0.024 0.816 0.040 0.004 0.116  0
#&gt; GSM317687     4  0.4113      0.594 0.192 0.000 0.008 0.744 0.056  0
#&gt; GSM317695     3  0.2793      0.891 0.200 0.000 0.800 0.000 0.000  0
#&gt; GSM317653     5  0.5009      0.722 0.256 0.000 0.000 0.120 0.624  0
#&gt; GSM317656     1  0.4735     -0.129 0.520 0.000 0.432 0.000 0.048  0
#&gt; GSM317658     4  0.7711      0.355 0.176 0.244 0.040 0.432 0.108  0
#&gt; GSM317660     5  0.5471      0.580 0.336 0.000 0.140 0.000 0.524  0
#&gt; GSM317663     4  0.2307      0.627 0.000 0.032 0.004 0.896 0.068  0
#&gt; GSM317664     1  0.0603      0.690 0.980 0.000 0.004 0.016 0.000  0
#&gt; GSM317665     1  0.5403      0.307 0.600 0.000 0.292 0.028 0.080  0
#&gt; GSM317673     1  0.1080      0.682 0.960 0.000 0.004 0.004 0.032  0
#&gt; GSM317686     6  0.0000      1.000 0.000 0.000 0.000 0.000 0.000  1
#&gt; GSM317688     1  0.0837      0.682 0.972 0.000 0.020 0.004 0.004  0
#&gt; GSM317690     2  0.7130      0.123 0.000 0.412 0.096 0.272 0.220  0
#&gt; GSM317654     1  0.5470      0.304 0.596 0.000 0.292 0.032 0.080  0
#&gt; GSM317655     4  0.4663      0.526 0.000 0.000 0.092 0.664 0.244  0
#&gt; GSM317659     4  0.2822      0.615 0.056 0.000 0.008 0.868 0.068  0
#&gt; GSM317661     2  0.2672      0.742 0.000 0.868 0.080 0.000 0.052  0
#&gt; GSM317662     2  0.0790      0.784 0.000 0.968 0.032 0.000 0.000  0
#&gt; GSM317668     4  0.5414      0.468 0.140 0.000 0.208 0.632 0.020  0
#&gt; GSM317669     3  0.2793      0.891 0.200 0.000 0.800 0.000 0.000  0
#&gt; GSM317671     3  0.2793      0.891 0.200 0.000 0.800 0.000 0.000  0
#&gt; GSM317676     4  0.2822      0.615 0.056 0.000 0.008 0.868 0.068  0
#&gt; GSM317680     3  0.2793      0.891 0.200 0.000 0.800 0.000 0.000  0
#&gt; GSM317684     4  0.4284      0.578 0.216 0.000 0.008 0.720 0.056  0
#&gt; GSM317685     1  0.2009      0.653 0.908 0.000 0.024 0.000 0.068  0
#&gt; GSM317694     4  0.4819      0.404 0.376 0.000 0.008 0.572 0.044  0
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-5-a').click(function(){
  $('#tab-SD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-hclust-signature_compare](figure_cola/SD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-hclust-collect-classes](figure_cola/SD-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) k
#> SD:hclust 47            0.561 2
#> SD:hclust 47            0.566 3
#> SD:hclust 45            0.417 4
#> SD:hclust 38            0.682 5
#> SD:hclust 35            0.408 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "kmeans"]
# you can also extract it by
# res = res_list["SD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-kmeans-collect-plots](figure_cola/SD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-kmeans-select-partition-number](figure_cola/SD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.974           0.948       0.974         0.4498 0.542   0.542
#> 3 3 0.469           0.534       0.667         0.3492 0.771   0.586
#> 4 4 0.517           0.634       0.788         0.1615 0.740   0.411
#> 5 5 0.606           0.619       0.780         0.0764 0.851   0.565
#> 6 6 0.675           0.650       0.790         0.0454 0.959   0.848
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-classes'>
<ul>
<li><a href='#tab-SD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-kmeans-get-classes-1'>
<p><a id='tab-SD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     1  0.0938      0.980 0.988 0.012
#&gt; GSM317652     1  0.0000      0.988 1.000 0.000
#&gt; GSM317666     2  0.1414      0.949 0.020 0.980
#&gt; GSM317672     2  0.2778      0.930 0.048 0.952
#&gt; GSM317679     1  0.0938      0.980 0.988 0.012
#&gt; GSM317681     2  0.6801      0.788 0.180 0.820
#&gt; GSM317682     1  0.0000      0.988 1.000 0.000
#&gt; GSM317683     2  0.0938      0.953 0.012 0.988
#&gt; GSM317689     2  0.0938      0.953 0.012 0.988
#&gt; GSM317691     1  0.0000      0.988 1.000 0.000
#&gt; GSM317692     1  0.0000      0.988 1.000 0.000
#&gt; GSM317693     1  0.0000      0.988 1.000 0.000
#&gt; GSM317696     1  0.0000      0.988 1.000 0.000
#&gt; GSM317697     1  0.0000      0.988 1.000 0.000
#&gt; GSM317698     1  0.0000      0.988 1.000 0.000
#&gt; GSM317650     2  0.0938      0.953 0.012 0.988
#&gt; GSM317651     1  0.0000      0.988 1.000 0.000
#&gt; GSM317657     2  0.0938      0.953 0.012 0.988
#&gt; GSM317667     2  0.0938      0.953 0.012 0.988
#&gt; GSM317670     2  0.4815      0.879 0.104 0.896
#&gt; GSM317674     1  0.0000      0.988 1.000 0.000
#&gt; GSM317675     1  0.0000      0.988 1.000 0.000
#&gt; GSM317677     1  0.0000      0.988 1.000 0.000
#&gt; GSM317678     2  0.0938      0.953 0.012 0.988
#&gt; GSM317687     1  0.0000      0.988 1.000 0.000
#&gt; GSM317695     1  0.0938      0.980 0.988 0.012
#&gt; GSM317653     1  0.8713      0.553 0.708 0.292
#&gt; GSM317656     1  0.0000      0.988 1.000 0.000
#&gt; GSM317658     2  0.9850      0.300 0.428 0.572
#&gt; GSM317660     1  0.0000      0.988 1.000 0.000
#&gt; GSM317663     2  0.0938      0.953 0.012 0.988
#&gt; GSM317664     1  0.0000      0.988 1.000 0.000
#&gt; GSM317665     1  0.0000      0.988 1.000 0.000
#&gt; GSM317673     1  0.0000      0.988 1.000 0.000
#&gt; GSM317686     2  0.0938      0.953 0.012 0.988
#&gt; GSM317688     1  0.0000      0.988 1.000 0.000
#&gt; GSM317690     2  0.0938      0.953 0.012 0.988
#&gt; GSM317654     1  0.0000      0.988 1.000 0.000
#&gt; GSM317655     2  0.0938      0.953 0.012 0.988
#&gt; GSM317659     1  0.0000      0.988 1.000 0.000
#&gt; GSM317661     2  0.0938      0.953 0.012 0.988
#&gt; GSM317662     2  0.0938      0.953 0.012 0.988
#&gt; GSM317668     1  0.0000      0.988 1.000 0.000
#&gt; GSM317669     1  0.0938      0.980 0.988 0.012
#&gt; GSM317671     1  0.0938      0.980 0.988 0.012
#&gt; GSM317676     1  0.0000      0.988 1.000 0.000
#&gt; GSM317680     1  0.0938      0.980 0.988 0.012
#&gt; GSM317684     1  0.0000      0.988 1.000 0.000
#&gt; GSM317685     1  0.0000      0.988 1.000 0.000
#&gt; GSM317694     1  0.0000      0.988 1.000 0.000
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-1-a').click(function(){
  $('#tab-SD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-2'>
<p><a id='tab-SD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM317649     1  0.0000     0.6139 1.000 0.000 0.000
#&gt; GSM317652     1  0.2878     0.6301 0.904 0.000 0.096
#&gt; GSM317666     3  0.6062    -0.5815 0.000 0.384 0.616
#&gt; GSM317672     2  0.4921     0.7585 0.020 0.816 0.164
#&gt; GSM317679     1  0.0237     0.6114 0.996 0.000 0.004
#&gt; GSM317681     2  0.6644     0.6910 0.108 0.752 0.140
#&gt; GSM317682     1  0.6154     0.3394 0.592 0.000 0.408
#&gt; GSM317683     2  0.0000     0.8008 0.000 1.000 0.000
#&gt; GSM317689     2  0.3116     0.7888 0.000 0.892 0.108
#&gt; GSM317691     3  0.5760     0.6202 0.328 0.000 0.672
#&gt; GSM317692     3  0.5956     0.6234 0.324 0.004 0.672
#&gt; GSM317693     3  0.5706     0.6284 0.320 0.000 0.680
#&gt; GSM317696     1  0.6192     0.3206 0.580 0.000 0.420
#&gt; GSM317697     3  0.6095     0.4736 0.392 0.000 0.608
#&gt; GSM317698     3  0.6309     0.0287 0.496 0.000 0.504
#&gt; GSM317650     2  0.0000     0.8008 0.000 1.000 0.000
#&gt; GSM317651     1  0.6126     0.3507 0.600 0.000 0.400
#&gt; GSM317657     2  0.6286     0.6798 0.000 0.536 0.464
#&gt; GSM317667     2  0.6154     0.6854 0.000 0.592 0.408
#&gt; GSM317670     2  0.6090     0.7175 0.020 0.716 0.264
#&gt; GSM317674     1  0.6192     0.3206 0.580 0.000 0.420
#&gt; GSM317675     1  0.6192     0.3206 0.580 0.000 0.420
#&gt; GSM317677     3  0.5678     0.6302 0.316 0.000 0.684
#&gt; GSM317678     2  0.0237     0.8000 0.000 0.996 0.004
#&gt; GSM317687     3  0.5529     0.6187 0.296 0.000 0.704
#&gt; GSM317695     1  0.0592     0.6154 0.988 0.000 0.012
#&gt; GSM317653     3  0.9463     0.3562 0.244 0.256 0.500
#&gt; GSM317656     1  0.3412     0.6222 0.876 0.000 0.124
#&gt; GSM317658     2  0.8405    -0.0102 0.084 0.460 0.456
#&gt; GSM317660     1  0.3456     0.6078 0.904 0.036 0.060
#&gt; GSM317663     2  0.6252     0.6947 0.000 0.556 0.444
#&gt; GSM317664     1  0.6192     0.3206 0.580 0.000 0.420
#&gt; GSM317665     1  0.2261     0.6302 0.932 0.000 0.068
#&gt; GSM317673     1  0.6180     0.3311 0.584 0.000 0.416
#&gt; GSM317686     2  0.5650     0.7320 0.000 0.688 0.312
#&gt; GSM317688     1  0.6192     0.3258 0.580 0.000 0.420
#&gt; GSM317690     2  0.1860     0.8002 0.000 0.948 0.052
#&gt; GSM317654     1  0.3038     0.6287 0.896 0.000 0.104
#&gt; GSM317655     2  0.6192     0.7094 0.000 0.580 0.420
#&gt; GSM317659     3  0.5678     0.6302 0.316 0.000 0.684
#&gt; GSM317661     2  0.0000     0.8008 0.000 1.000 0.000
#&gt; GSM317662     2  0.0000     0.8008 0.000 1.000 0.000
#&gt; GSM317668     1  0.4974     0.5428 0.764 0.000 0.236
#&gt; GSM317669     1  0.0000     0.6139 1.000 0.000 0.000
#&gt; GSM317671     1  0.0000     0.6139 1.000 0.000 0.000
#&gt; GSM317676     3  0.1964     0.4187 0.056 0.000 0.944
#&gt; GSM317680     1  0.0000     0.6139 1.000 0.000 0.000
#&gt; GSM317684     3  0.5733     0.6267 0.324 0.000 0.676
#&gt; GSM317685     1  0.6180     0.3311 0.584 0.000 0.416
#&gt; GSM317694     3  0.6305     0.0873 0.484 0.000 0.516
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-2-a').click(function(){
  $('#tab-SD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-3'>
<p><a id='tab-SD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     3  0.0921     0.7841 0.028 0.000 0.972 0.000
#&gt; GSM317652     3  0.6215     0.6687 0.192 0.000 0.668 0.140
#&gt; GSM317666     4  0.6252     0.6704 0.192 0.128 0.004 0.676
#&gt; GSM317672     2  0.5985     0.5405 0.036 0.660 0.020 0.284
#&gt; GSM317679     3  0.1109     0.7824 0.028 0.000 0.968 0.004
#&gt; GSM317681     2  0.7810     0.3587 0.148 0.524 0.028 0.300
#&gt; GSM317682     1  0.6357     0.5454 0.656 0.000 0.184 0.160
#&gt; GSM317683     2  0.0000     0.7564 0.000 1.000 0.000 0.000
#&gt; GSM317689     2  0.3908     0.6619 0.032 0.844 0.008 0.116
#&gt; GSM317691     1  0.2281     0.7102 0.904 0.000 0.000 0.096
#&gt; GSM317692     1  0.3108     0.6943 0.872 0.000 0.016 0.112
#&gt; GSM317693     1  0.0707     0.7380 0.980 0.000 0.000 0.020
#&gt; GSM317696     1  0.3649     0.6886 0.796 0.000 0.204 0.000
#&gt; GSM317697     1  0.0937     0.7395 0.976 0.000 0.012 0.012
#&gt; GSM317698     1  0.2216     0.7338 0.908 0.000 0.092 0.000
#&gt; GSM317650     2  0.0000     0.7564 0.000 1.000 0.000 0.000
#&gt; GSM317651     1  0.6816     0.4641 0.604 0.000 0.212 0.184
#&gt; GSM317657     4  0.6617     0.7356 0.120 0.280 0.000 0.600
#&gt; GSM317667     4  0.5281     0.6448 0.012 0.300 0.012 0.676
#&gt; GSM317670     2  0.7278     0.2078 0.220 0.576 0.008 0.196
#&gt; GSM317674     1  0.3649     0.6886 0.796 0.000 0.204 0.000
#&gt; GSM317675     1  0.3649     0.6886 0.796 0.000 0.204 0.000
#&gt; GSM317677     1  0.1398     0.7336 0.956 0.000 0.004 0.040
#&gt; GSM317678     2  0.0921     0.7519 0.000 0.972 0.000 0.028
#&gt; GSM317687     1  0.2589     0.6976 0.884 0.000 0.000 0.116
#&gt; GSM317695     3  0.1022     0.7831 0.032 0.000 0.968 0.000
#&gt; GSM317653     1  0.7458     0.3360 0.540 0.124 0.020 0.316
#&gt; GSM317656     3  0.5750     0.2348 0.440 0.000 0.532 0.028
#&gt; GSM317658     1  0.7349    -0.0581 0.488 0.392 0.016 0.104
#&gt; GSM317660     3  0.6133     0.7095 0.084 0.020 0.704 0.192
#&gt; GSM317663     4  0.6426     0.7432 0.108 0.272 0.000 0.620
#&gt; GSM317664     1  0.3649     0.6886 0.796 0.000 0.204 0.000
#&gt; GSM317665     3  0.5889     0.7117 0.116 0.000 0.696 0.188
#&gt; GSM317673     1  0.4361     0.6714 0.772 0.000 0.208 0.020
#&gt; GSM317686     4  0.5093     0.6220 0.000 0.348 0.012 0.640
#&gt; GSM317688     1  0.4919     0.6609 0.752 0.000 0.200 0.048
#&gt; GSM317690     2  0.2216     0.6966 0.000 0.908 0.000 0.092
#&gt; GSM317654     3  0.6978     0.6088 0.208 0.000 0.584 0.208
#&gt; GSM317655     4  0.6430     0.7242 0.092 0.312 0.000 0.596
#&gt; GSM317659     1  0.1940     0.7201 0.924 0.000 0.000 0.076
#&gt; GSM317661     2  0.0000     0.7564 0.000 1.000 0.000 0.000
#&gt; GSM317662     2  0.0000     0.7564 0.000 1.000 0.000 0.000
#&gt; GSM317668     3  0.5724     0.2477 0.424 0.000 0.548 0.028
#&gt; GSM317669     3  0.0921     0.7841 0.028 0.000 0.972 0.000
#&gt; GSM317671     3  0.1109     0.7824 0.028 0.000 0.968 0.004
#&gt; GSM317676     1  0.5028     0.1631 0.596 0.000 0.004 0.400
#&gt; GSM317680     3  0.0921     0.7841 0.028 0.000 0.972 0.000
#&gt; GSM317684     1  0.1118     0.7336 0.964 0.000 0.000 0.036
#&gt; GSM317685     1  0.4399     0.6678 0.768 0.000 0.212 0.020
#&gt; GSM317694     1  0.2469     0.7296 0.892 0.000 0.108 0.000
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-3-a').click(function(){
  $('#tab-SD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-4'>
<p><a id='tab-SD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     3  0.0566    0.99250 0.012 0.000 0.984 0.000 0.004
#&gt; GSM317652     5  0.7069    0.31995 0.256 0.000 0.364 0.012 0.368
#&gt; GSM317666     4  0.3078    0.64707 0.056 0.008 0.000 0.872 0.064
#&gt; GSM317672     5  0.6999    0.02998 0.044 0.372 0.000 0.128 0.456
#&gt; GSM317679     3  0.0404    0.99557 0.012 0.000 0.988 0.000 0.000
#&gt; GSM317681     5  0.6088    0.45564 0.052 0.188 0.000 0.104 0.656
#&gt; GSM317682     1  0.5206    0.02859 0.572 0.000 0.040 0.004 0.384
#&gt; GSM317683     2  0.0162    0.82517 0.000 0.996 0.000 0.000 0.004
#&gt; GSM317689     2  0.4295    0.63224 0.004 0.732 0.000 0.236 0.028
#&gt; GSM317691     1  0.4974    0.58583 0.696 0.000 0.000 0.212 0.092
#&gt; GSM317692     1  0.5565    0.51072 0.632 0.000 0.000 0.240 0.128
#&gt; GSM317693     1  0.2770    0.72152 0.880 0.000 0.000 0.044 0.076
#&gt; GSM317696     1  0.1956    0.73914 0.916 0.000 0.076 0.000 0.008
#&gt; GSM317697     1  0.1300    0.73850 0.956 0.000 0.000 0.016 0.028
#&gt; GSM317698     1  0.0566    0.74347 0.984 0.000 0.012 0.000 0.004
#&gt; GSM317650     2  0.0162    0.82517 0.000 0.996 0.000 0.000 0.004
#&gt; GSM317651     5  0.5322    0.32534 0.408 0.000 0.044 0.004 0.544
#&gt; GSM317657     4  0.3716    0.63406 0.048 0.072 0.000 0.844 0.036
#&gt; GSM317667     4  0.6157    0.49129 0.004 0.120 0.012 0.596 0.268
#&gt; GSM317670     2  0.6636    0.03246 0.188 0.420 0.000 0.388 0.004
#&gt; GSM317674     1  0.2069    0.73915 0.912 0.000 0.076 0.000 0.012
#&gt; GSM317675     1  0.2069    0.73915 0.912 0.000 0.076 0.000 0.012
#&gt; GSM317677     1  0.3622    0.69680 0.816 0.000 0.000 0.048 0.136
#&gt; GSM317678     2  0.1117    0.81297 0.000 0.964 0.000 0.016 0.020
#&gt; GSM317687     1  0.5774    0.48924 0.612 0.000 0.000 0.232 0.156
#&gt; GSM317695     3  0.0510    0.99173 0.016 0.000 0.984 0.000 0.000
#&gt; GSM317653     5  0.5283    0.45188 0.112 0.024 0.000 0.144 0.720
#&gt; GSM317656     1  0.4501    0.58777 0.740 0.000 0.212 0.012 0.036
#&gt; GSM317658     1  0.6994    0.26242 0.528 0.268 0.000 0.156 0.048
#&gt; GSM317660     5  0.5458    0.44483 0.040 0.004 0.380 0.008 0.568
#&gt; GSM317663     4  0.2846    0.64832 0.028 0.076 0.000 0.884 0.012
#&gt; GSM317664     1  0.2069    0.73915 0.912 0.000 0.076 0.000 0.012
#&gt; GSM317665     5  0.5489    0.47840 0.064 0.000 0.356 0.004 0.576
#&gt; GSM317673     1  0.2429    0.73223 0.900 0.000 0.076 0.004 0.020
#&gt; GSM317686     4  0.6142    0.47568 0.000 0.148 0.012 0.596 0.244
#&gt; GSM317688     1  0.3386    0.70732 0.856 0.000 0.068 0.012 0.064
#&gt; GSM317690     2  0.2970    0.72990 0.000 0.828 0.000 0.168 0.004
#&gt; GSM317654     5  0.5389    0.55620 0.088 0.000 0.260 0.004 0.648
#&gt; GSM317655     4  0.2921    0.62479 0.020 0.124 0.000 0.856 0.000
#&gt; GSM317659     1  0.5125    0.60384 0.696 0.000 0.000 0.156 0.148
#&gt; GSM317661     2  0.0000    0.82480 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317662     2  0.0000    0.82480 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317668     1  0.5746    0.36648 0.576 0.000 0.348 0.020 0.056
#&gt; GSM317669     3  0.0451    0.98997 0.008 0.000 0.988 0.000 0.004
#&gt; GSM317671     3  0.0404    0.99557 0.012 0.000 0.988 0.000 0.000
#&gt; GSM317676     4  0.6253   -0.00199 0.388 0.000 0.000 0.464 0.148
#&gt; GSM317680     3  0.0404    0.99557 0.012 0.000 0.988 0.000 0.000
#&gt; GSM317684     1  0.3506    0.70085 0.824 0.000 0.000 0.044 0.132
#&gt; GSM317685     1  0.2913    0.73029 0.876 0.000 0.080 0.004 0.040
#&gt; GSM317694     1  0.2932    0.72901 0.864 0.000 0.020 0.004 0.112
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-4-a').click(function(){
  $('#tab-SD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-5'>
<p><a id='tab-SD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM317649     3  0.0508     0.9802 0.000 0.000 0.984 0.000 0.012 0.004
#&gt; GSM317652     1  0.6653    -0.1354 0.408 0.000 0.128 0.000 0.388 0.076
#&gt; GSM317666     4  0.5047     0.5301 0.028 0.000 0.000 0.692 0.128 0.152
#&gt; GSM317672     5  0.6861     0.4787 0.056 0.156 0.000 0.156 0.572 0.060
#&gt; GSM317679     3  0.0146     0.9924 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM317681     5  0.4880     0.6517 0.040 0.056 0.000 0.104 0.756 0.044
#&gt; GSM317682     1  0.5478    -0.0543 0.504 0.000 0.004 0.004 0.392 0.096
#&gt; GSM317683     2  0.0000     0.7922 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317689     2  0.5059     0.0727 0.000 0.480 0.000 0.464 0.036 0.020
#&gt; GSM317691     1  0.5619     0.5526 0.608 0.000 0.000 0.244 0.116 0.032
#&gt; GSM317692     1  0.6440     0.3954 0.496 0.000 0.000 0.304 0.140 0.060
#&gt; GSM317693     1  0.4220     0.6941 0.776 0.000 0.000 0.104 0.088 0.032
#&gt; GSM317696     1  0.1116     0.7307 0.960 0.000 0.028 0.004 0.000 0.008
#&gt; GSM317697     1  0.2216     0.7267 0.908 0.000 0.000 0.052 0.016 0.024
#&gt; GSM317698     1  0.0291     0.7346 0.992 0.000 0.000 0.004 0.000 0.004
#&gt; GSM317650     2  0.0000     0.7922 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317651     5  0.4750     0.5493 0.264 0.000 0.016 0.000 0.664 0.056
#&gt; GSM317657     4  0.2271     0.6689 0.000 0.024 0.000 0.908 0.036 0.032
#&gt; GSM317667     6  0.3243     0.9732 0.000 0.028 0.000 0.156 0.004 0.812
#&gt; GSM317670     4  0.4807     0.4348 0.016 0.224 0.000 0.700 0.032 0.028
#&gt; GSM317674     1  0.1116     0.7324 0.960 0.000 0.028 0.000 0.008 0.004
#&gt; GSM317675     1  0.1332     0.7320 0.952 0.000 0.028 0.000 0.008 0.012
#&gt; GSM317677     1  0.4560     0.6731 0.744 0.000 0.000 0.092 0.132 0.032
#&gt; GSM317678     2  0.1148     0.7744 0.000 0.960 0.000 0.020 0.004 0.016
#&gt; GSM317687     1  0.6203     0.4562 0.536 0.000 0.000 0.256 0.168 0.040
#&gt; GSM317695     3  0.0146     0.9900 0.004 0.000 0.996 0.000 0.000 0.000
#&gt; GSM317653     5  0.1821     0.6666 0.008 0.000 0.000 0.040 0.928 0.024
#&gt; GSM317656     1  0.3775     0.6717 0.816 0.000 0.076 0.000 0.048 0.060
#&gt; GSM317658     1  0.6520     0.4030 0.552 0.128 0.000 0.252 0.036 0.032
#&gt; GSM317660     5  0.3546     0.6933 0.016 0.000 0.196 0.000 0.776 0.012
#&gt; GSM317663     4  0.3007     0.6575 0.000 0.020 0.000 0.860 0.040 0.080
#&gt; GSM317664     1  0.1116     0.7324 0.960 0.000 0.028 0.000 0.008 0.004
#&gt; GSM317665     5  0.3269     0.7052 0.024 0.000 0.184 0.000 0.792 0.000
#&gt; GSM317673     1  0.2245     0.7158 0.908 0.000 0.028 0.004 0.008 0.052
#&gt; GSM317686     6  0.3240     0.9731 0.000 0.040 0.000 0.148 0.000 0.812
#&gt; GSM317688     1  0.3668     0.6816 0.816 0.000 0.020 0.004 0.048 0.112
#&gt; GSM317690     2  0.4627     0.2328 0.000 0.532 0.000 0.436 0.020 0.012
#&gt; GSM317654     5  0.2412     0.7215 0.028 0.000 0.092 0.000 0.880 0.000
#&gt; GSM317655     4  0.3337     0.6176 0.000 0.044 0.000 0.840 0.028 0.088
#&gt; GSM317659     1  0.5538     0.5826 0.636 0.000 0.000 0.184 0.148 0.032
#&gt; GSM317661     2  0.0146     0.7917 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; GSM317662     2  0.0146     0.7917 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; GSM317668     1  0.6569     0.5024 0.604 0.000 0.140 0.040 0.144 0.072
#&gt; GSM317669     3  0.0146     0.9924 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM317671     3  0.0000     0.9930 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317676     4  0.5638     0.3937 0.180 0.000 0.000 0.632 0.148 0.040
#&gt; GSM317680     3  0.0000     0.9930 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317684     1  0.4874     0.6583 0.716 0.000 0.000 0.104 0.144 0.036
#&gt; GSM317685     1  0.2839     0.7113 0.876 0.000 0.032 0.000 0.040 0.052
#&gt; GSM317694     1  0.4105     0.6903 0.780 0.000 0.000 0.068 0.124 0.028
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-5-a').click(function(){
  $('#tab-SD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-kmeans-signature_compare](figure_cola/SD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-kmeans-collect-classes](figure_cola/SD-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) k
#> SD:kmeans 49            0.706 2
#> SD:kmeans 34            0.527 3
#> SD:kmeans 42            0.928 4
#> SD:kmeans 35            0.943 5
#> SD:kmeans 40            0.827 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "skmeans"]
# you can also extract it by
# res = res_list["SD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-skmeans-collect-plots](figure_cola/SD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-skmeans-select-partition-number](figure_cola/SD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.968       0.987         0.4850 0.510   0.510
#> 3 3 0.837           0.917       0.948         0.3783 0.805   0.624
#> 4 4 0.763           0.816       0.892         0.1291 0.892   0.687
#> 5 5 0.671           0.556       0.720         0.0601 0.931   0.727
#> 6 6 0.694           0.591       0.773         0.0430 0.900   0.567
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-classes'>
<ul>
<li><a href='#tab-SD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-skmeans-get-classes-1'>
<p><a id='tab-SD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     1  0.0000      0.999 1.000 0.000
#&gt; GSM317652     1  0.0000      0.999 1.000 0.000
#&gt; GSM317666     2  0.0000      0.968 0.000 1.000
#&gt; GSM317672     2  0.0000      0.968 0.000 1.000
#&gt; GSM317679     1  0.0000      0.999 1.000 0.000
#&gt; GSM317681     2  0.0000      0.968 0.000 1.000
#&gt; GSM317682     1  0.0000      0.999 1.000 0.000
#&gt; GSM317683     2  0.0000      0.968 0.000 1.000
#&gt; GSM317689     2  0.0000      0.968 0.000 1.000
#&gt; GSM317691     1  0.1184      0.984 0.984 0.016
#&gt; GSM317692     2  0.0000      0.968 0.000 1.000
#&gt; GSM317693     1  0.0000      0.999 1.000 0.000
#&gt; GSM317696     1  0.0000      0.999 1.000 0.000
#&gt; GSM317697     1  0.0938      0.988 0.988 0.012
#&gt; GSM317698     1  0.0000      0.999 1.000 0.000
#&gt; GSM317650     2  0.0000      0.968 0.000 1.000
#&gt; GSM317651     1  0.0000      0.999 1.000 0.000
#&gt; GSM317657     2  0.0000      0.968 0.000 1.000
#&gt; GSM317667     2  0.0000      0.968 0.000 1.000
#&gt; GSM317670     2  0.0000      0.968 0.000 1.000
#&gt; GSM317674     1  0.0000      0.999 1.000 0.000
#&gt; GSM317675     1  0.0000      0.999 1.000 0.000
#&gt; GSM317677     1  0.0000      0.999 1.000 0.000
#&gt; GSM317678     2  0.0000      0.968 0.000 1.000
#&gt; GSM317687     1  0.0000      0.999 1.000 0.000
#&gt; GSM317695     1  0.0000      0.999 1.000 0.000
#&gt; GSM317653     2  0.6148      0.811 0.152 0.848
#&gt; GSM317656     1  0.0000      0.999 1.000 0.000
#&gt; GSM317658     2  0.0000      0.968 0.000 1.000
#&gt; GSM317660     2  0.9922      0.211 0.448 0.552
#&gt; GSM317663     2  0.0000      0.968 0.000 1.000
#&gt; GSM317664     1  0.0000      0.999 1.000 0.000
#&gt; GSM317665     1  0.0000      0.999 1.000 0.000
#&gt; GSM317673     1  0.0000      0.999 1.000 0.000
#&gt; GSM317686     2  0.0000      0.968 0.000 1.000
#&gt; GSM317688     1  0.0000      0.999 1.000 0.000
#&gt; GSM317690     2  0.0000      0.968 0.000 1.000
#&gt; GSM317654     1  0.0000      0.999 1.000 0.000
#&gt; GSM317655     2  0.0000      0.968 0.000 1.000
#&gt; GSM317659     1  0.0000      0.999 1.000 0.000
#&gt; GSM317661     2  0.0000      0.968 0.000 1.000
#&gt; GSM317662     2  0.0000      0.968 0.000 1.000
#&gt; GSM317668     1  0.0000      0.999 1.000 0.000
#&gt; GSM317669     1  0.0000      0.999 1.000 0.000
#&gt; GSM317671     1  0.0000      0.999 1.000 0.000
#&gt; GSM317676     1  0.0376      0.995 0.996 0.004
#&gt; GSM317680     1  0.0000      0.999 1.000 0.000
#&gt; GSM317684     1  0.0000      0.999 1.000 0.000
#&gt; GSM317685     1  0.0000      0.999 1.000 0.000
#&gt; GSM317694     1  0.0000      0.999 1.000 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-1-a').click(function(){
  $('#tab-SD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-2'>
<p><a id='tab-SD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM317649     3  0.0000      0.995 0.000 0.000 1.000
#&gt; GSM317652     3  0.0000      0.995 0.000 0.000 1.000
#&gt; GSM317666     2  0.4555      0.792 0.200 0.800 0.000
#&gt; GSM317672     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM317679     3  0.0000      0.995 0.000 0.000 1.000
#&gt; GSM317681     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM317682     1  0.4605      0.834 0.796 0.000 0.204
#&gt; GSM317683     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM317689     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM317691     1  0.0000      0.880 1.000 0.000 0.000
#&gt; GSM317692     2  0.1753      0.936 0.048 0.952 0.000
#&gt; GSM317693     1  0.0000      0.880 1.000 0.000 0.000
#&gt; GSM317696     1  0.4399      0.845 0.812 0.000 0.188
#&gt; GSM317697     1  0.0237      0.880 0.996 0.000 0.004
#&gt; GSM317698     1  0.0237      0.880 0.996 0.000 0.004
#&gt; GSM317650     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM317651     1  0.4842      0.815 0.776 0.000 0.224
#&gt; GSM317657     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM317667     2  0.0424      0.965 0.008 0.992 0.000
#&gt; GSM317670     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM317674     1  0.4346      0.847 0.816 0.000 0.184
#&gt; GSM317675     1  0.4346      0.847 0.816 0.000 0.184
#&gt; GSM317677     1  0.0000      0.880 1.000 0.000 0.000
#&gt; GSM317678     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM317687     1  0.0000      0.880 1.000 0.000 0.000
#&gt; GSM317695     3  0.0000      0.995 0.000 0.000 1.000
#&gt; GSM317653     2  0.7772      0.651 0.196 0.672 0.132
#&gt; GSM317656     3  0.0000      0.995 0.000 0.000 1.000
#&gt; GSM317658     2  0.0424      0.965 0.008 0.992 0.000
#&gt; GSM317660     3  0.0000      0.995 0.000 0.000 1.000
#&gt; GSM317663     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM317664     1  0.4346      0.847 0.816 0.000 0.184
#&gt; GSM317665     3  0.0000      0.995 0.000 0.000 1.000
#&gt; GSM317673     1  0.4555      0.837 0.800 0.000 0.200
#&gt; GSM317686     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM317688     1  0.6111      0.537 0.604 0.000 0.396
#&gt; GSM317690     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM317654     3  0.0237      0.991 0.004 0.000 0.996
#&gt; GSM317655     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM317659     1  0.0000      0.880 1.000 0.000 0.000
#&gt; GSM317661     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM317662     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM317668     3  0.1529      0.951 0.040 0.000 0.960
#&gt; GSM317669     3  0.0000      0.995 0.000 0.000 1.000
#&gt; GSM317671     3  0.0000      0.995 0.000 0.000 1.000
#&gt; GSM317676     1  0.0000      0.880 1.000 0.000 0.000
#&gt; GSM317680     3  0.0000      0.995 0.000 0.000 1.000
#&gt; GSM317684     1  0.0000      0.880 1.000 0.000 0.000
#&gt; GSM317685     1  0.4555      0.837 0.800 0.000 0.200
#&gt; GSM317694     1  0.0000      0.880 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-2-a').click(function(){
  $('#tab-SD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-3'>
<p><a id='tab-SD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     3  0.0000      0.947 0.000 0.000 1.000 0.000
#&gt; GSM317652     3  0.0927      0.945 0.008 0.000 0.976 0.016
#&gt; GSM317666     4  0.1489      0.750 0.004 0.044 0.000 0.952
#&gt; GSM317672     2  0.0592      0.918 0.000 0.984 0.000 0.016
#&gt; GSM317679     3  0.0000      0.947 0.000 0.000 1.000 0.000
#&gt; GSM317681     2  0.1557      0.891 0.000 0.944 0.000 0.056
#&gt; GSM317682     1  0.1854      0.832 0.940 0.000 0.012 0.048
#&gt; GSM317683     2  0.0000      0.927 0.000 1.000 0.000 0.000
#&gt; GSM317689     2  0.0188      0.926 0.000 0.996 0.000 0.004
#&gt; GSM317691     1  0.4967      0.435 0.548 0.000 0.000 0.452
#&gt; GSM317692     2  0.4049      0.682 0.008 0.780 0.000 0.212
#&gt; GSM317693     1  0.3356      0.789 0.824 0.000 0.000 0.176
#&gt; GSM317696     1  0.0188      0.855 0.996 0.000 0.004 0.000
#&gt; GSM317697     1  0.0000      0.855 1.000 0.000 0.000 0.000
#&gt; GSM317698     1  0.0000      0.855 1.000 0.000 0.000 0.000
#&gt; GSM317650     2  0.0000      0.927 0.000 1.000 0.000 0.000
#&gt; GSM317651     1  0.5312      0.635 0.712 0.000 0.236 0.052
#&gt; GSM317657     4  0.4356      0.714 0.000 0.292 0.000 0.708
#&gt; GSM317667     4  0.3311      0.762 0.000 0.172 0.000 0.828
#&gt; GSM317670     2  0.4194      0.635 0.000 0.764 0.008 0.228
#&gt; GSM317674     1  0.0188      0.855 0.996 0.000 0.004 0.000
#&gt; GSM317675     1  0.0188      0.855 0.996 0.000 0.004 0.000
#&gt; GSM317677     1  0.3975      0.745 0.760 0.000 0.000 0.240
#&gt; GSM317678     2  0.0000      0.927 0.000 1.000 0.000 0.000
#&gt; GSM317687     4  0.1940      0.705 0.076 0.000 0.000 0.924
#&gt; GSM317695     3  0.0707      0.941 0.020 0.000 0.980 0.000
#&gt; GSM317653     4  0.4284      0.648 0.000 0.200 0.020 0.780
#&gt; GSM317656     3  0.3688      0.753 0.208 0.000 0.792 0.000
#&gt; GSM317658     2  0.1820      0.893 0.036 0.944 0.000 0.020
#&gt; GSM317660     3  0.1854      0.930 0.000 0.012 0.940 0.048
#&gt; GSM317663     4  0.4500      0.700 0.000 0.316 0.000 0.684
#&gt; GSM317664     1  0.0336      0.854 0.992 0.000 0.008 0.000
#&gt; GSM317665     3  0.1389      0.935 0.000 0.000 0.952 0.048
#&gt; GSM317673     1  0.0657      0.852 0.984 0.000 0.012 0.004
#&gt; GSM317686     4  0.4697      0.646 0.000 0.356 0.000 0.644
#&gt; GSM317688     1  0.3708      0.747 0.832 0.000 0.148 0.020
#&gt; GSM317690     2  0.1557      0.891 0.000 0.944 0.000 0.056
#&gt; GSM317654     3  0.2053      0.923 0.000 0.004 0.924 0.072
#&gt; GSM317655     4  0.4477      0.701 0.000 0.312 0.000 0.688
#&gt; GSM317659     1  0.4992      0.393 0.524 0.000 0.000 0.476
#&gt; GSM317661     2  0.0188      0.926 0.000 0.996 0.000 0.004
#&gt; GSM317662     2  0.0000      0.927 0.000 1.000 0.000 0.000
#&gt; GSM317668     3  0.3032      0.842 0.124 0.000 0.868 0.008
#&gt; GSM317669     3  0.0000      0.947 0.000 0.000 1.000 0.000
#&gt; GSM317671     3  0.0000      0.947 0.000 0.000 1.000 0.000
#&gt; GSM317676     4  0.1474      0.726 0.052 0.000 0.000 0.948
#&gt; GSM317680     3  0.0000      0.947 0.000 0.000 1.000 0.000
#&gt; GSM317684     1  0.4103      0.732 0.744 0.000 0.000 0.256
#&gt; GSM317685     1  0.0779      0.852 0.980 0.000 0.016 0.004
#&gt; GSM317694     1  0.2814      0.813 0.868 0.000 0.000 0.132
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-3-a').click(function(){
  $('#tab-SD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-4'>
<p><a id='tab-SD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     3  0.0794     0.7711 0.028 0.000 0.972 0.000 0.000
#&gt; GSM317652     3  0.5415     0.7076 0.212 0.000 0.688 0.076 0.024
#&gt; GSM317666     4  0.3724     0.5275 0.000 0.020 0.000 0.776 0.204
#&gt; GSM317672     2  0.1830     0.8066 0.028 0.932 0.000 0.040 0.000
#&gt; GSM317679     3  0.0000     0.7710 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317681     2  0.5726     0.5050 0.188 0.640 0.004 0.168 0.000
#&gt; GSM317682     1  0.4026     0.5428 0.736 0.000 0.000 0.020 0.244
#&gt; GSM317683     2  0.0000     0.8308 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317689     2  0.0703     0.8280 0.000 0.976 0.000 0.024 0.000
#&gt; GSM317691     5  0.4337     0.4802 0.056 0.000 0.000 0.196 0.748
#&gt; GSM317692     2  0.7181     0.3487 0.084 0.552 0.000 0.172 0.192
#&gt; GSM317693     5  0.3690     0.1236 0.224 0.000 0.000 0.012 0.764
#&gt; GSM317696     1  0.4278     0.6982 0.548 0.000 0.000 0.000 0.452
#&gt; GSM317697     5  0.4747    -0.6822 0.488 0.000 0.000 0.016 0.496
#&gt; GSM317698     1  0.4297     0.6912 0.528 0.000 0.000 0.000 0.472
#&gt; GSM317650     2  0.0290     0.8309 0.000 0.992 0.000 0.008 0.000
#&gt; GSM317651     1  0.7140     0.0125 0.564 0.000 0.116 0.120 0.200
#&gt; GSM317657     4  0.4285     0.6728 0.008 0.208 0.000 0.752 0.032
#&gt; GSM317667     4  0.3821     0.6743 0.000 0.216 0.000 0.764 0.020
#&gt; GSM317670     2  0.5467     0.4490 0.040 0.656 0.016 0.276 0.012
#&gt; GSM317674     1  0.4291     0.6997 0.536 0.000 0.000 0.000 0.464
#&gt; GSM317675     1  0.4287     0.7018 0.540 0.000 0.000 0.000 0.460
#&gt; GSM317677     5  0.3003     0.4398 0.092 0.000 0.000 0.044 0.864
#&gt; GSM317678     2  0.0324     0.8298 0.004 0.992 0.000 0.004 0.000
#&gt; GSM317687     5  0.4648    -0.2030 0.012 0.000 0.000 0.464 0.524
#&gt; GSM317695     3  0.1478     0.7482 0.064 0.000 0.936 0.000 0.000
#&gt; GSM317653     4  0.8205     0.0829 0.340 0.108 0.004 0.344 0.204
#&gt; GSM317656     3  0.5729     0.3873 0.352 0.000 0.572 0.016 0.060
#&gt; GSM317658     2  0.4397     0.7214 0.080 0.804 0.000 0.056 0.060
#&gt; GSM317660     3  0.6422     0.5954 0.340 0.008 0.504 0.148 0.000
#&gt; GSM317663     4  0.3700     0.6679 0.000 0.240 0.000 0.752 0.008
#&gt; GSM317664     1  0.4291     0.6997 0.536 0.000 0.000 0.000 0.464
#&gt; GSM317665     3  0.6121     0.6087 0.324 0.000 0.528 0.148 0.000
#&gt; GSM317673     1  0.4219     0.6972 0.584 0.000 0.000 0.000 0.416
#&gt; GSM317686     4  0.3857     0.5891 0.000 0.312 0.000 0.688 0.000
#&gt; GSM317688     1  0.5760     0.4883 0.648 0.000 0.088 0.024 0.240
#&gt; GSM317690     2  0.2707     0.7393 0.008 0.860 0.000 0.132 0.000
#&gt; GSM317654     3  0.7458     0.5191 0.352 0.000 0.420 0.168 0.060
#&gt; GSM317655     4  0.3844     0.6448 0.004 0.256 0.000 0.736 0.004
#&gt; GSM317659     5  0.3586     0.5065 0.020 0.000 0.000 0.188 0.792
#&gt; GSM317661     2  0.0609     0.8294 0.000 0.980 0.000 0.020 0.000
#&gt; GSM317662     2  0.0290     0.8309 0.000 0.992 0.000 0.008 0.000
#&gt; GSM317668     3  0.6039     0.6074 0.156 0.000 0.668 0.052 0.124
#&gt; GSM317669     3  0.0162     0.7715 0.004 0.000 0.996 0.000 0.000
#&gt; GSM317671     3  0.0000     0.7710 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317676     4  0.4650     0.1323 0.012 0.000 0.000 0.520 0.468
#&gt; GSM317680     3  0.0000     0.7710 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317684     5  0.2795     0.4977 0.056 0.000 0.000 0.064 0.880
#&gt; GSM317685     1  0.4801     0.6542 0.584 0.000 0.012 0.008 0.396
#&gt; GSM317694     5  0.3300     0.2004 0.204 0.000 0.000 0.004 0.792
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-4-a').click(function(){
  $('#tab-SD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-5'>
<p><a id='tab-SD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM317649     3  0.0858     0.7668 0.000 0.000 0.968 0.000 0.028 0.004
#&gt; GSM317652     3  0.6637    -0.2707 0.140 0.000 0.412 0.012 0.396 0.040
#&gt; GSM317666     4  0.2615     0.7277 0.000 0.008 0.000 0.852 0.004 0.136
#&gt; GSM317672     2  0.1995     0.7641 0.000 0.912 0.000 0.000 0.052 0.036
#&gt; GSM317679     3  0.0000     0.7860 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317681     2  0.5451     0.3518 0.000 0.564 0.008 0.044 0.352 0.032
#&gt; GSM317682     1  0.5531     0.5625 0.632 0.008 0.000 0.016 0.212 0.132
#&gt; GSM317683     2  0.0837     0.7872 0.000 0.972 0.000 0.020 0.004 0.004
#&gt; GSM317689     2  0.1841     0.7782 0.000 0.920 0.000 0.064 0.008 0.008
#&gt; GSM317691     6  0.5340     0.5756 0.144 0.000 0.004 0.064 0.096 0.692
#&gt; GSM317692     2  0.7539     0.2677 0.012 0.420 0.000 0.152 0.172 0.244
#&gt; GSM317693     1  0.5419     0.0553 0.476 0.000 0.000 0.016 0.072 0.436
#&gt; GSM317696     1  0.0820     0.7095 0.972 0.000 0.000 0.000 0.012 0.016
#&gt; GSM317697     1  0.4482     0.5802 0.736 0.000 0.000 0.016 0.096 0.152
#&gt; GSM317698     1  0.0653     0.7069 0.980 0.000 0.000 0.004 0.004 0.012
#&gt; GSM317650     2  0.0891     0.7870 0.000 0.968 0.000 0.024 0.008 0.000
#&gt; GSM317651     5  0.6277     0.5032 0.172 0.000 0.064 0.012 0.600 0.152
#&gt; GSM317657     4  0.2803     0.8644 0.000 0.064 0.000 0.872 0.012 0.052
#&gt; GSM317667     4  0.2169     0.8763 0.000 0.080 0.000 0.900 0.008 0.012
#&gt; GSM317670     2  0.6448     0.2916 0.004 0.500 0.000 0.324 0.092 0.080
#&gt; GSM317674     1  0.0291     0.7085 0.992 0.000 0.000 0.004 0.000 0.004
#&gt; GSM317675     1  0.0146     0.7090 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; GSM317677     6  0.4524     0.2698 0.452 0.000 0.000 0.024 0.004 0.520
#&gt; GSM317678     2  0.0767     0.7825 0.000 0.976 0.000 0.004 0.008 0.012
#&gt; GSM317687     6  0.4203     0.5557 0.016 0.000 0.000 0.288 0.016 0.680
#&gt; GSM317695     3  0.0937     0.7553 0.040 0.000 0.960 0.000 0.000 0.000
#&gt; GSM317653     5  0.5428     0.5032 0.000 0.080 0.000 0.112 0.680 0.128
#&gt; GSM317656     1  0.6474     0.0723 0.480 0.000 0.352 0.016 0.112 0.040
#&gt; GSM317658     2  0.5728     0.6406 0.044 0.692 0.000 0.060 0.096 0.108
#&gt; GSM317660     5  0.4353     0.6226 0.000 0.032 0.288 0.004 0.672 0.004
#&gt; GSM317663     4  0.2476     0.8858 0.000 0.092 0.000 0.880 0.004 0.024
#&gt; GSM317664     1  0.0260     0.7090 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; GSM317665     5  0.4022     0.5485 0.004 0.008 0.360 0.000 0.628 0.000
#&gt; GSM317673     1  0.2883     0.6938 0.864 0.000 0.000 0.008 0.060 0.068
#&gt; GSM317686     4  0.2544     0.8567 0.000 0.140 0.000 0.852 0.004 0.004
#&gt; GSM317688     1  0.6871     0.4795 0.556 0.000 0.076 0.036 0.200 0.132
#&gt; GSM317690     2  0.4090     0.6397 0.000 0.748 0.000 0.192 0.048 0.012
#&gt; GSM317654     5  0.4592     0.6556 0.000 0.000 0.240 0.004 0.680 0.076
#&gt; GSM317655     4  0.3394     0.8429 0.000 0.104 0.000 0.832 0.028 0.036
#&gt; GSM317659     6  0.5220     0.6729 0.156 0.000 0.000 0.152 0.024 0.668
#&gt; GSM317661     2  0.1524     0.7790 0.000 0.932 0.000 0.060 0.008 0.000
#&gt; GSM317662     2  0.0972     0.7867 0.000 0.964 0.000 0.028 0.008 0.000
#&gt; GSM317668     3  0.7673     0.1317 0.196 0.004 0.452 0.032 0.212 0.104
#&gt; GSM317669     3  0.0000     0.7860 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317671     3  0.0000     0.7860 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317676     6  0.4262     0.3041 0.004 0.000 0.000 0.424 0.012 0.560
#&gt; GSM317680     3  0.0000     0.7860 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317684     6  0.4228     0.6111 0.184 0.000 0.000 0.028 0.040 0.748
#&gt; GSM317685     1  0.4303     0.6426 0.752 0.000 0.000 0.012 0.108 0.128
#&gt; GSM317694     1  0.3966    -0.1444 0.552 0.000 0.000 0.004 0.000 0.444
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-5-a').click(function(){
  $('#tab-SD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-skmeans-signature_compare](figure_cola/SD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-skmeans-collect-classes](figure_cola/SD-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> SD:skmeans 49            0.553 2
#> SD:skmeans 50            0.689 3
#> SD:skmeans 48            0.738 4
#> SD:skmeans 36            0.814 5
#> SD:skmeans 39            0.680 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:pam*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "pam"]
# you can also extract it by
# res = res_list["SD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-pam-collect-plots](figure_cola/SD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-pam-select-partition-number](figure_cola/SD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.916           0.962       0.982         0.4548 0.542   0.542
#> 3 3 0.652           0.772       0.833         0.2366 0.851   0.726
#> 4 4 0.733           0.868       0.927         0.1689 0.901   0.764
#> 5 5 0.739           0.781       0.874         0.0768 0.984   0.952
#> 6 6 0.676           0.632       0.838         0.0668 0.961   0.880
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-classes'>
<ul>
<li><a href='#tab-SD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-pam-get-classes-1'>
<p><a id='tab-SD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     1  0.0000      0.989 1.000 0.000
#&gt; GSM317652     1  0.0000      0.989 1.000 0.000
#&gt; GSM317666     2  0.0672      0.960 0.008 0.992
#&gt; GSM317672     1  0.0000      0.989 1.000 0.000
#&gt; GSM317679     1  0.0376      0.986 0.996 0.004
#&gt; GSM317681     1  0.0000      0.989 1.000 0.000
#&gt; GSM317682     1  0.0000      0.989 1.000 0.000
#&gt; GSM317683     2  0.0000      0.964 0.000 1.000
#&gt; GSM317689     2  0.0000      0.964 0.000 1.000
#&gt; GSM317691     2  0.5519      0.860 0.128 0.872
#&gt; GSM317692     1  0.4939      0.876 0.892 0.108
#&gt; GSM317693     1  0.0000      0.989 1.000 0.000
#&gt; GSM317696     1  0.0000      0.989 1.000 0.000
#&gt; GSM317697     1  0.0000      0.989 1.000 0.000
#&gt; GSM317698     1  0.0000      0.989 1.000 0.000
#&gt; GSM317650     2  0.0000      0.964 0.000 1.000
#&gt; GSM317651     1  0.0000      0.989 1.000 0.000
#&gt; GSM317657     2  0.0000      0.964 0.000 1.000
#&gt; GSM317667     2  0.0000      0.964 0.000 1.000
#&gt; GSM317670     2  0.0000      0.964 0.000 1.000
#&gt; GSM317674     1  0.0000      0.989 1.000 0.000
#&gt; GSM317675     1  0.0000      0.989 1.000 0.000
#&gt; GSM317677     1  0.0000      0.989 1.000 0.000
#&gt; GSM317678     2  0.0672      0.960 0.008 0.992
#&gt; GSM317687     2  0.8081      0.704 0.248 0.752
#&gt; GSM317695     1  0.0000      0.989 1.000 0.000
#&gt; GSM317653     1  0.0000      0.989 1.000 0.000
#&gt; GSM317656     1  0.0000      0.989 1.000 0.000
#&gt; GSM317658     1  0.7528      0.723 0.784 0.216
#&gt; GSM317660     1  0.0672      0.982 0.992 0.008
#&gt; GSM317663     2  0.0000      0.964 0.000 1.000
#&gt; GSM317664     1  0.0000      0.989 1.000 0.000
#&gt; GSM317665     1  0.0000      0.989 1.000 0.000
#&gt; GSM317673     1  0.0000      0.989 1.000 0.000
#&gt; GSM317686     2  0.0000      0.964 0.000 1.000
#&gt; GSM317688     1  0.0000      0.989 1.000 0.000
#&gt; GSM317690     2  0.0000      0.964 0.000 1.000
#&gt; GSM317654     1  0.0000      0.989 1.000 0.000
#&gt; GSM317655     2  0.0000      0.964 0.000 1.000
#&gt; GSM317659     1  0.0000      0.989 1.000 0.000
#&gt; GSM317661     2  0.0000      0.964 0.000 1.000
#&gt; GSM317662     2  0.0000      0.964 0.000 1.000
#&gt; GSM317668     1  0.0000      0.989 1.000 0.000
#&gt; GSM317669     1  0.0000      0.989 1.000 0.000
#&gt; GSM317671     1  0.0000      0.989 1.000 0.000
#&gt; GSM317676     2  0.6801      0.802 0.180 0.820
#&gt; GSM317680     1  0.0000      0.989 1.000 0.000
#&gt; GSM317684     1  0.0000      0.989 1.000 0.000
#&gt; GSM317685     1  0.0000      0.989 1.000 0.000
#&gt; GSM317694     1  0.0000      0.989 1.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-1-a').click(function(){
  $('#tab-SD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-2'>
<p><a id='tab-SD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM317649     3  0.0237      0.814 0.004 0.000 0.996
#&gt; GSM317652     1  0.6308      0.898 0.508 0.000 0.492
#&gt; GSM317666     2  0.6307      0.828 0.488 0.512 0.000
#&gt; GSM317672     1  0.8646      0.628 0.556 0.124 0.320
#&gt; GSM317679     3  0.2096      0.743 0.052 0.004 0.944
#&gt; GSM317681     1  0.8953      0.327 0.560 0.260 0.180
#&gt; GSM317682     1  0.6307      0.895 0.512 0.000 0.488
#&gt; GSM317683     2  0.0000      0.713 0.000 1.000 0.000
#&gt; GSM317689     2  0.6154      0.829 0.408 0.592 0.000
#&gt; GSM317691     2  0.8708      0.744 0.404 0.488 0.108
#&gt; GSM317692     1  0.6416      0.715 0.616 0.008 0.376
#&gt; GSM317693     1  0.6244      0.840 0.560 0.000 0.440
#&gt; GSM317696     1  0.6308      0.898 0.508 0.000 0.492
#&gt; GSM317697     1  0.6244      0.840 0.560 0.000 0.440
#&gt; GSM317698     1  0.6308      0.898 0.508 0.000 0.492
#&gt; GSM317650     2  0.0000      0.713 0.000 1.000 0.000
#&gt; GSM317651     1  0.6308      0.898 0.508 0.000 0.492
#&gt; GSM317657     2  0.6302      0.829 0.480 0.520 0.000
#&gt; GSM317667     2  0.6244      0.830 0.440 0.560 0.000
#&gt; GSM317670     2  0.6204      0.832 0.424 0.576 0.000
#&gt; GSM317674     1  0.6308      0.898 0.508 0.000 0.492
#&gt; GSM317675     1  0.6308      0.898 0.508 0.000 0.492
#&gt; GSM317677     1  0.6308      0.898 0.508 0.000 0.492
#&gt; GSM317678     2  0.1289      0.709 0.032 0.968 0.000
#&gt; GSM317687     2  0.9356      0.620 0.368 0.460 0.172
#&gt; GSM317695     3  0.0000      0.818 0.000 0.000 1.000
#&gt; GSM317653     1  0.6244      0.840 0.560 0.000 0.440
#&gt; GSM317656     1  0.6308      0.898 0.508 0.000 0.492
#&gt; GSM317658     1  0.9014      0.326 0.560 0.208 0.232
#&gt; GSM317660     3  0.6680     -0.872 0.484 0.008 0.508
#&gt; GSM317663     2  0.6302      0.829 0.480 0.520 0.000
#&gt; GSM317664     1  0.6308      0.898 0.508 0.000 0.492
#&gt; GSM317665     1  0.6308      0.898 0.508 0.000 0.492
#&gt; GSM317673     1  0.6308      0.898 0.508 0.000 0.492
#&gt; GSM317686     2  0.6244      0.830 0.440 0.560 0.000
#&gt; GSM317688     1  0.6307      0.895 0.512 0.000 0.488
#&gt; GSM317690     2  0.6126      0.832 0.400 0.600 0.000
#&gt; GSM317654     1  0.6302      0.887 0.520 0.000 0.480
#&gt; GSM317655     2  0.6291      0.831 0.468 0.532 0.000
#&gt; GSM317659     1  0.6308      0.898 0.508 0.000 0.492
#&gt; GSM317661     2  0.0000      0.713 0.000 1.000 0.000
#&gt; GSM317662     2  0.0000      0.713 0.000 1.000 0.000
#&gt; GSM317668     1  0.6308      0.898 0.508 0.000 0.492
#&gt; GSM317669     3  0.0000      0.818 0.000 0.000 1.000
#&gt; GSM317671     3  0.0000      0.818 0.000 0.000 1.000
#&gt; GSM317676     2  0.9050      0.706 0.376 0.484 0.140
#&gt; GSM317680     3  0.0000      0.818 0.000 0.000 1.000
#&gt; GSM317684     1  0.6267      0.855 0.548 0.000 0.452
#&gt; GSM317685     1  0.6308      0.898 0.508 0.000 0.492
#&gt; GSM317694     1  0.6308      0.898 0.508 0.000 0.492
</code></pre>

<script>
$('#tab-SD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-2-a').click(function(){
  $('#tab-SD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-3'>
<p><a id='tab-SD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     3  0.2704      0.909 0.124 0.000 0.876 0.000
#&gt; GSM317652     1  0.0188      0.931 0.996 0.000 0.004 0.000
#&gt; GSM317666     4  0.0000      0.871 0.000 0.000 0.000 1.000
#&gt; GSM317672     1  0.4888      0.776 0.780 0.124 0.000 0.096
#&gt; GSM317679     3  0.2081      0.831 0.000 0.000 0.916 0.084
#&gt; GSM317681     2  0.6020      0.536 0.220 0.684 0.004 0.092
#&gt; GSM317682     1  0.0188      0.931 0.996 0.000 0.004 0.000
#&gt; GSM317683     2  0.0000      0.903 0.000 1.000 0.000 0.000
#&gt; GSM317689     4  0.2281      0.839 0.000 0.096 0.000 0.904
#&gt; GSM317691     4  0.2814      0.788 0.132 0.000 0.000 0.868
#&gt; GSM317692     1  0.3649      0.793 0.796 0.000 0.000 0.204
#&gt; GSM317693     1  0.2944      0.856 0.868 0.000 0.004 0.128
#&gt; GSM317696     1  0.0188      0.931 0.996 0.000 0.004 0.000
#&gt; GSM317697     1  0.2944      0.856 0.868 0.000 0.004 0.128
#&gt; GSM317698     1  0.0188      0.931 0.996 0.000 0.004 0.000
#&gt; GSM317650     2  0.0000      0.903 0.000 1.000 0.000 0.000
#&gt; GSM317651     1  0.0000      0.931 1.000 0.000 0.000 0.000
#&gt; GSM317657     4  0.0336      0.872 0.000 0.008 0.000 0.992
#&gt; GSM317667     4  0.3354      0.818 0.000 0.044 0.084 0.872
#&gt; GSM317670     4  0.1637      0.863 0.000 0.060 0.000 0.940
#&gt; GSM317674     1  0.0188      0.931 0.996 0.000 0.004 0.000
#&gt; GSM317675     1  0.0000      0.931 1.000 0.000 0.000 0.000
#&gt; GSM317677     1  0.0000      0.931 1.000 0.000 0.000 0.000
#&gt; GSM317678     2  0.0817      0.888 0.000 0.976 0.000 0.024
#&gt; GSM317687     4  0.3837      0.669 0.224 0.000 0.000 0.776
#&gt; GSM317695     3  0.2081      0.955 0.084 0.000 0.916 0.000
#&gt; GSM317653     1  0.2760      0.855 0.872 0.000 0.000 0.128
#&gt; GSM317656     1  0.0188      0.931 0.996 0.000 0.004 0.000
#&gt; GSM317658     1  0.6840      0.511 0.600 0.180 0.000 0.220
#&gt; GSM317660     1  0.3791      0.716 0.796 0.004 0.200 0.000
#&gt; GSM317663     4  0.0336      0.872 0.000 0.008 0.000 0.992
#&gt; GSM317664     1  0.0188      0.931 0.996 0.000 0.004 0.000
#&gt; GSM317665     1  0.0188      0.931 0.996 0.000 0.004 0.000
#&gt; GSM317673     1  0.0188      0.931 0.996 0.000 0.004 0.000
#&gt; GSM317686     4  0.3354      0.818 0.000 0.044 0.084 0.872
#&gt; GSM317688     1  0.0188      0.931 0.996 0.000 0.004 0.000
#&gt; GSM317690     4  0.3172      0.795 0.000 0.160 0.000 0.840
#&gt; GSM317654     1  0.1488      0.914 0.956 0.000 0.012 0.032
#&gt; GSM317655     4  0.0707      0.872 0.000 0.020 0.000 0.980
#&gt; GSM317659     1  0.0000      0.931 1.000 0.000 0.000 0.000
#&gt; GSM317661     2  0.0336      0.900 0.000 0.992 0.000 0.008
#&gt; GSM317662     2  0.0000      0.903 0.000 1.000 0.000 0.000
#&gt; GSM317668     1  0.0188      0.931 0.996 0.000 0.004 0.000
#&gt; GSM317669     3  0.2081      0.955 0.084 0.000 0.916 0.000
#&gt; GSM317671     3  0.2081      0.955 0.084 0.000 0.916 0.000
#&gt; GSM317676     4  0.3219      0.752 0.164 0.000 0.000 0.836
#&gt; GSM317680     3  0.2081      0.955 0.084 0.000 0.916 0.000
#&gt; GSM317684     1  0.2647      0.861 0.880 0.000 0.000 0.120
#&gt; GSM317685     1  0.0000      0.931 1.000 0.000 0.000 0.000
#&gt; GSM317694     1  0.0000      0.931 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-3-a').click(function(){
  $('#tab-SD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-4'>
<p><a id='tab-SD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     3  0.1043      0.931 0.040 0.000 0.960 0.000 0.000
#&gt; GSM317652     1  0.0000      0.828 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317666     4  0.1965      0.843 0.000 0.000 0.000 0.904 0.096
#&gt; GSM317672     1  0.5373      0.581 0.632 0.092 0.000 0.276 0.000
#&gt; GSM317679     3  0.0000      0.987 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317681     2  0.5107      0.528 0.108 0.688 0.000 0.204 0.000
#&gt; GSM317682     1  0.0880      0.822 0.968 0.000 0.000 0.032 0.000
#&gt; GSM317683     2  0.0000      0.905 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317689     4  0.4360      0.721 0.000 0.184 0.000 0.752 0.064
#&gt; GSM317691     4  0.1082      0.800 0.028 0.000 0.000 0.964 0.008
#&gt; GSM317692     1  0.4872      0.396 0.540 0.000 0.000 0.436 0.024
#&gt; GSM317693     1  0.4210      0.467 0.588 0.000 0.000 0.412 0.000
#&gt; GSM317696     1  0.0794      0.824 0.972 0.000 0.000 0.028 0.000
#&gt; GSM317697     1  0.4219      0.460 0.584 0.000 0.000 0.416 0.000
#&gt; GSM317698     1  0.0703      0.825 0.976 0.000 0.000 0.024 0.000
#&gt; GSM317650     2  0.0000      0.905 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317651     1  0.1671      0.812 0.924 0.000 0.000 0.076 0.000
#&gt; GSM317657     4  0.2304      0.845 0.000 0.008 0.000 0.892 0.100
#&gt; GSM317667     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM317670     4  0.2795      0.842 0.000 0.028 0.000 0.872 0.100
#&gt; GSM317674     1  0.0000      0.828 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317675     1  0.0290      0.828 0.992 0.000 0.000 0.008 0.000
#&gt; GSM317677     1  0.1671      0.812 0.924 0.000 0.000 0.076 0.000
#&gt; GSM317678     2  0.0703      0.890 0.000 0.976 0.000 0.024 0.000
#&gt; GSM317687     4  0.1544      0.776 0.068 0.000 0.000 0.932 0.000
#&gt; GSM317695     3  0.0000      0.987 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317653     1  0.4291      0.418 0.536 0.000 0.000 0.464 0.000
#&gt; GSM317656     1  0.0162      0.828 0.996 0.000 0.000 0.004 0.000
#&gt; GSM317658     1  0.6328      0.262 0.448 0.136 0.000 0.412 0.004
#&gt; GSM317660     1  0.3300      0.698 0.792 0.004 0.204 0.000 0.000
#&gt; GSM317663     4  0.2304      0.845 0.000 0.008 0.000 0.892 0.100
#&gt; GSM317664     1  0.0000      0.828 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317665     1  0.0000      0.828 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317673     1  0.0609      0.826 0.980 0.000 0.000 0.020 0.000
#&gt; GSM317686     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM317688     1  0.0963      0.822 0.964 0.000 0.000 0.036 0.000
#&gt; GSM317690     4  0.5700      0.311 0.000 0.380 0.000 0.532 0.088
#&gt; GSM317654     1  0.2488      0.797 0.872 0.000 0.004 0.124 0.000
#&gt; GSM317655     4  0.2795      0.842 0.000 0.028 0.000 0.872 0.100
#&gt; GSM317659     1  0.1671      0.812 0.924 0.000 0.000 0.076 0.000
#&gt; GSM317661     2  0.0162      0.904 0.000 0.996 0.000 0.004 0.000
#&gt; GSM317662     2  0.0000      0.905 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317668     1  0.0162      0.828 0.996 0.000 0.004 0.000 0.000
#&gt; GSM317669     3  0.0000      0.987 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317671     3  0.0000      0.987 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317676     4  0.1478      0.785 0.064 0.000 0.000 0.936 0.000
#&gt; GSM317680     3  0.0000      0.987 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317684     1  0.4256      0.466 0.564 0.000 0.000 0.436 0.000
#&gt; GSM317685     1  0.1544      0.815 0.932 0.000 0.000 0.068 0.000
#&gt; GSM317694     1  0.1608      0.813 0.928 0.000 0.000 0.072 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-4-a').click(function(){
  $('#tab-SD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-5'>
<p><a id='tab-SD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM317649     3  0.0937     0.9344 0.040 0.000 0.960 0.000 0.000  0
#&gt; GSM317652     1  0.0000     0.6707 1.000 0.000 0.000 0.000 0.000  0
#&gt; GSM317666     4  0.2118     0.7355 0.008 0.000 0.000 0.888 0.104  0
#&gt; GSM317672     1  0.5657     0.4288 0.652 0.100 0.000 0.164 0.084  0
#&gt; GSM317679     3  0.0000     0.9871 0.000 0.000 1.000 0.000 0.000  0
#&gt; GSM317681     2  0.6250     0.4605 0.108 0.592 0.000 0.152 0.148  0
#&gt; GSM317682     1  0.2006     0.6510 0.904 0.000 0.000 0.016 0.080  0
#&gt; GSM317683     2  0.0000     0.8723 0.000 1.000 0.000 0.000 0.000  0
#&gt; GSM317689     4  0.1753     0.7547 0.000 0.084 0.000 0.912 0.004  0
#&gt; GSM317691     4  0.2883     0.6466 0.000 0.000 0.000 0.788 0.212  0
#&gt; GSM317692     1  0.4986     0.3530 0.612 0.000 0.000 0.284 0.104  0
#&gt; GSM317693     1  0.4723     0.4135 0.664 0.000 0.000 0.232 0.104  0
#&gt; GSM317696     1  0.1367     0.6672 0.944 0.000 0.000 0.012 0.044  0
#&gt; GSM317697     1  0.4503     0.4351 0.684 0.000 0.000 0.232 0.084  0
#&gt; GSM317698     1  0.0806     0.6713 0.972 0.000 0.000 0.008 0.020  0
#&gt; GSM317650     2  0.0000     0.8723 0.000 1.000 0.000 0.000 0.000  0
#&gt; GSM317651     1  0.2854     0.4827 0.792 0.000 0.000 0.000 0.208  0
#&gt; GSM317657     4  0.0260     0.7700 0.000 0.008 0.000 0.992 0.000  0
#&gt; GSM317667     6  0.0000     1.0000 0.000 0.000 0.000 0.000 0.000  1
#&gt; GSM317670     4  0.3217     0.6752 0.000 0.008 0.000 0.768 0.224  0
#&gt; GSM317674     1  0.0000     0.6707 1.000 0.000 0.000 0.000 0.000  0
#&gt; GSM317675     1  0.0363     0.6679 0.988 0.000 0.000 0.000 0.012  0
#&gt; GSM317677     1  0.2883     0.4749 0.788 0.000 0.000 0.000 0.212  0
#&gt; GSM317678     2  0.0935     0.8548 0.000 0.964 0.000 0.004 0.032  0
#&gt; GSM317687     4  0.4065     0.5380 0.028 0.000 0.000 0.672 0.300  0
#&gt; GSM317695     3  0.0000     0.9871 0.000 0.000 1.000 0.000 0.000  0
#&gt; GSM317653     5  0.5348     0.5007 0.192 0.000 0.000 0.216 0.592  0
#&gt; GSM317656     1  0.0000     0.6707 1.000 0.000 0.000 0.000 0.000  0
#&gt; GSM317658     1  0.6641     0.1813 0.504 0.144 0.000 0.264 0.088  0
#&gt; GSM317660     1  0.5980    -0.3780 0.460 0.004 0.208 0.000 0.328  0
#&gt; GSM317663     4  0.0260     0.7700 0.000 0.008 0.000 0.992 0.000  0
#&gt; GSM317664     1  0.0000     0.6707 1.000 0.000 0.000 0.000 0.000  0
#&gt; GSM317665     1  0.3482    -0.0597 0.684 0.000 0.000 0.000 0.316  0
#&gt; GSM317673     1  0.0692     0.6718 0.976 0.000 0.000 0.004 0.020  0
#&gt; GSM317686     6  0.0000     1.0000 0.000 0.000 0.000 0.000 0.000  1
#&gt; GSM317688     1  0.1895     0.6561 0.912 0.000 0.000 0.016 0.072  0
#&gt; GSM317690     4  0.3314     0.6744 0.000 0.012 0.000 0.764 0.224  0
#&gt; GSM317654     5  0.4642     0.3763 0.452 0.000 0.000 0.040 0.508  0
#&gt; GSM317655     4  0.3217     0.6752 0.000 0.008 0.000 0.768 0.224  0
#&gt; GSM317659     1  0.2996     0.4508 0.772 0.000 0.000 0.000 0.228  0
#&gt; GSM317661     2  0.2006     0.8154 0.000 0.892 0.000 0.004 0.104  0
#&gt; GSM317662     2  0.0000     0.8723 0.000 1.000 0.000 0.000 0.000  0
#&gt; GSM317668     1  0.2595     0.5676 0.836 0.000 0.004 0.000 0.160  0
#&gt; GSM317669     3  0.0000     0.9871 0.000 0.000 1.000 0.000 0.000  0
#&gt; GSM317671     3  0.0000     0.9871 0.000 0.000 1.000 0.000 0.000  0
#&gt; GSM317676     4  0.3885     0.5878 0.044 0.000 0.000 0.736 0.220  0
#&gt; GSM317680     3  0.0000     0.9871 0.000 0.000 1.000 0.000 0.000  0
#&gt; GSM317684     1  0.5524     0.0791 0.560 0.000 0.000 0.204 0.236  0
#&gt; GSM317685     1  0.2178     0.5857 0.868 0.000 0.000 0.000 0.132  0
#&gt; GSM317694     1  0.2003     0.5962 0.884 0.000 0.000 0.000 0.116  0
</code></pre>

<script>
$('#tab-SD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-5-a').click(function(){
  $('#tab-SD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-SD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-pam-signature_compare](figure_cola/SD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-SD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-pam-collect-classes](figure_cola/SD-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n disease.state(p) k
#> SD:pam 50            0.438 2
#> SD:pam 47            0.569 3
#> SD:pam 50            0.602 4
#> SD:pam 43            0.713 5
#> SD:pam 37            0.670 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "mclust"]
# you can also extract it by
# res = res_list["SD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-mclust-collect-plots](figure_cola/SD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-mclust-select-partition-number](figure_cola/SD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.711           0.864       0.927         0.3853 0.607   0.607
#> 3 3 0.850           0.898       0.955         0.6485 0.634   0.455
#> 4 4 0.682           0.756       0.888         0.0331 0.758   0.507
#> 5 5 0.765           0.781       0.897         0.1070 0.907   0.755
#> 6 6 0.760           0.772       0.884         0.0803 0.841   0.527
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-classes'>
<ul>
<li><a href='#tab-SD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-mclust-get-classes-1'>
<p><a id='tab-SD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     1  0.3431      0.919 0.936 0.064
#&gt; GSM317652     1  0.3431      0.919 0.936 0.064
#&gt; GSM317666     1  0.7602      0.705 0.780 0.220
#&gt; GSM317672     1  0.5408      0.898 0.876 0.124
#&gt; GSM317679     1  0.3431      0.919 0.936 0.064
#&gt; GSM317681     1  0.8499      0.679 0.724 0.276
#&gt; GSM317682     1  0.1843      0.932 0.972 0.028
#&gt; GSM317683     2  0.0000      0.850 0.000 1.000
#&gt; GSM317689     2  0.7376      0.702 0.208 0.792
#&gt; GSM317691     1  0.2603      0.927 0.956 0.044
#&gt; GSM317692     1  0.2603      0.927 0.956 0.044
#&gt; GSM317693     1  0.0000      0.936 1.000 0.000
#&gt; GSM317696     1  0.0000      0.936 1.000 0.000
#&gt; GSM317697     1  0.0000      0.936 1.000 0.000
#&gt; GSM317698     1  0.0000      0.936 1.000 0.000
#&gt; GSM317650     2  0.0000      0.850 0.000 1.000
#&gt; GSM317651     1  0.0938      0.936 0.988 0.012
#&gt; GSM317657     2  0.9815      0.425 0.420 0.580
#&gt; GSM317667     2  0.3431      0.838 0.064 0.936
#&gt; GSM317670     2  0.9909      0.356 0.444 0.556
#&gt; GSM317674     1  0.0000      0.936 1.000 0.000
#&gt; GSM317675     1  0.0938      0.935 0.988 0.012
#&gt; GSM317677     1  0.2603      0.927 0.956 0.044
#&gt; GSM317678     2  0.0000      0.850 0.000 1.000
#&gt; GSM317687     1  0.2603      0.927 0.956 0.044
#&gt; GSM317695     1  0.3431      0.919 0.936 0.064
#&gt; GSM317653     1  0.7602      0.731 0.780 0.220
#&gt; GSM317656     1  0.0672      0.936 0.992 0.008
#&gt; GSM317658     1  0.3879      0.904 0.924 0.076
#&gt; GSM317660     1  0.3879      0.915 0.924 0.076
#&gt; GSM317663     2  0.9686      0.479 0.396 0.604
#&gt; GSM317664     1  0.0000      0.936 1.000 0.000
#&gt; GSM317665     1  0.3431      0.919 0.936 0.064
#&gt; GSM317673     1  0.0000      0.936 1.000 0.000
#&gt; GSM317686     2  0.3431      0.838 0.064 0.936
#&gt; GSM317688     1  0.0000      0.936 1.000 0.000
#&gt; GSM317690     2  0.0376      0.850 0.004 0.996
#&gt; GSM317654     1  0.3431      0.919 0.936 0.064
#&gt; GSM317655     2  0.3431      0.838 0.064 0.936
#&gt; GSM317659     1  0.2603      0.927 0.956 0.044
#&gt; GSM317661     2  0.0000      0.850 0.000 1.000
#&gt; GSM317662     2  0.0000      0.850 0.000 1.000
#&gt; GSM317668     1  0.0672      0.936 0.992 0.008
#&gt; GSM317669     1  0.3431      0.919 0.936 0.064
#&gt; GSM317671     1  0.3431      0.919 0.936 0.064
#&gt; GSM317676     1  0.3274      0.917 0.940 0.060
#&gt; GSM317680     1  0.3431      0.919 0.936 0.064
#&gt; GSM317684     1  0.2603      0.927 0.956 0.044
#&gt; GSM317685     1  0.0000      0.936 1.000 0.000
#&gt; GSM317694     1  0.2603      0.927 0.956 0.044
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-1-a').click(function(){
  $('#tab-SD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-2'>
<p><a id='tab-SD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM317649     3  0.0000      0.926 0.000 0.000 1.000
#&gt; GSM317652     1  0.6062      0.416 0.616 0.000 0.384
#&gt; GSM317666     2  0.0829      0.976 0.004 0.984 0.012
#&gt; GSM317672     2  0.0237      0.978 0.004 0.996 0.000
#&gt; GSM317679     3  0.0000      0.926 0.000 0.000 1.000
#&gt; GSM317681     2  0.0747      0.974 0.000 0.984 0.016
#&gt; GSM317682     1  0.3816      0.812 0.852 0.000 0.148
#&gt; GSM317683     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM317689     2  0.0237      0.978 0.004 0.996 0.000
#&gt; GSM317691     1  0.0000      0.929 1.000 0.000 0.000
#&gt; GSM317692     1  0.3851      0.801 0.860 0.136 0.004
#&gt; GSM317693     1  0.0237      0.930 0.996 0.000 0.004
#&gt; GSM317696     1  0.0424      0.931 0.992 0.000 0.008
#&gt; GSM317697     1  0.0424      0.931 0.992 0.000 0.008
#&gt; GSM317698     1  0.0424      0.931 0.992 0.000 0.008
#&gt; GSM317650     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM317651     1  0.5254      0.662 0.736 0.000 0.264
#&gt; GSM317657     2  0.0237      0.978 0.004 0.996 0.000
#&gt; GSM317667     2  0.0592      0.975 0.000 0.988 0.012
#&gt; GSM317670     2  0.1753      0.940 0.048 0.952 0.000
#&gt; GSM317674     1  0.0424      0.931 0.992 0.000 0.008
#&gt; GSM317675     1  0.0237      0.930 0.996 0.000 0.004
#&gt; GSM317677     1  0.0000      0.929 1.000 0.000 0.000
#&gt; GSM317678     2  0.0237      0.978 0.004 0.996 0.000
#&gt; GSM317687     1  0.3116      0.833 0.892 0.108 0.000
#&gt; GSM317695     1  0.0592      0.929 0.988 0.000 0.012
#&gt; GSM317653     2  0.1751      0.959 0.028 0.960 0.012
#&gt; GSM317656     1  0.0424      0.931 0.992 0.000 0.008
#&gt; GSM317658     2  0.3983      0.805 0.144 0.852 0.004
#&gt; GSM317660     3  0.0424      0.923 0.000 0.008 0.992
#&gt; GSM317663     2  0.0829      0.976 0.004 0.984 0.012
#&gt; GSM317664     1  0.0424      0.931 0.992 0.000 0.008
#&gt; GSM317665     3  0.0424      0.923 0.008 0.000 0.992
#&gt; GSM317673     1  0.0237      0.930 0.996 0.000 0.004
#&gt; GSM317686     2  0.0592      0.975 0.000 0.988 0.012
#&gt; GSM317688     1  0.0424      0.931 0.992 0.000 0.008
#&gt; GSM317690     2  0.0237      0.978 0.004 0.996 0.000
#&gt; GSM317654     3  0.6192      0.148 0.420 0.000 0.580
#&gt; GSM317655     2  0.0829      0.976 0.004 0.984 0.012
#&gt; GSM317659     1  0.0000      0.929 1.000 0.000 0.000
#&gt; GSM317661     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM317662     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM317668     1  0.5363      0.621 0.724 0.000 0.276
#&gt; GSM317669     3  0.0000      0.926 0.000 0.000 1.000
#&gt; GSM317671     3  0.0892      0.914 0.020 0.000 0.980
#&gt; GSM317676     2  0.1411      0.957 0.036 0.964 0.000
#&gt; GSM317680     3  0.0000      0.926 0.000 0.000 1.000
#&gt; GSM317684     1  0.0000      0.929 1.000 0.000 0.000
#&gt; GSM317685     1  0.0424      0.931 0.992 0.000 0.008
#&gt; GSM317694     1  0.0000      0.929 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-2-a').click(function(){
  $('#tab-SD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-3'>
<p><a id='tab-SD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     3  0.0188      0.801 0.004 0.000 0.996 0.000
#&gt; GSM317652     3  0.4624      0.505 0.340 0.000 0.660 0.000
#&gt; GSM317666     4  0.2021      0.947 0.012 0.056 0.000 0.932
#&gt; GSM317672     1  0.3726      0.713 0.788 0.212 0.000 0.000
#&gt; GSM317679     3  0.0188      0.801 0.004 0.000 0.996 0.000
#&gt; GSM317681     3  0.6985      0.465 0.312 0.140 0.548 0.000
#&gt; GSM317682     1  0.3074      0.746 0.848 0.000 0.152 0.000
#&gt; GSM317683     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; GSM317689     2  0.3266      0.700 0.168 0.832 0.000 0.000
#&gt; GSM317691     1  0.1557      0.840 0.944 0.000 0.000 0.056
#&gt; GSM317692     1  0.1557      0.828 0.944 0.056 0.000 0.000
#&gt; GSM317693     1  0.1557      0.840 0.944 0.000 0.000 0.056
#&gt; GSM317696     1  0.0000      0.845 1.000 0.000 0.000 0.000
#&gt; GSM317697     1  0.0000      0.845 1.000 0.000 0.000 0.000
#&gt; GSM317698     1  0.0000      0.845 1.000 0.000 0.000 0.000
#&gt; GSM317650     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; GSM317651     1  0.3688      0.675 0.792 0.000 0.208 0.000
#&gt; GSM317657     1  0.7028      0.387 0.568 0.260 0.000 0.172
#&gt; GSM317667     4  0.1557      0.943 0.000 0.056 0.000 0.944
#&gt; GSM317670     1  0.4972      0.220 0.544 0.456 0.000 0.000
#&gt; GSM317674     1  0.0000      0.845 1.000 0.000 0.000 0.000
#&gt; GSM317675     1  0.0000      0.845 1.000 0.000 0.000 0.000
#&gt; GSM317677     1  0.1557      0.840 0.944 0.000 0.000 0.056
#&gt; GSM317678     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; GSM317687     1  0.2921      0.794 0.860 0.000 0.000 0.140
#&gt; GSM317695     1  0.4331      0.485 0.712 0.000 0.288 0.000
#&gt; GSM317653     1  0.7525     -0.171 0.460 0.056 0.428 0.056
#&gt; GSM317656     1  0.0000      0.845 1.000 0.000 0.000 0.000
#&gt; GSM317658     1  0.4103      0.653 0.744 0.256 0.000 0.000
#&gt; GSM317660     3  0.0336      0.797 0.000 0.008 0.992 0.000
#&gt; GSM317663     4  0.3601      0.879 0.084 0.056 0.000 0.860
#&gt; GSM317664     1  0.0000      0.845 1.000 0.000 0.000 0.000
#&gt; GSM317665     3  0.1118      0.789 0.036 0.000 0.964 0.000
#&gt; GSM317673     1  0.0000      0.845 1.000 0.000 0.000 0.000
#&gt; GSM317686     4  0.1557      0.943 0.000 0.056 0.000 0.944
#&gt; GSM317688     1  0.0000      0.845 1.000 0.000 0.000 0.000
#&gt; GSM317690     2  0.0188      0.947 0.004 0.996 0.000 0.000
#&gt; GSM317654     3  0.4855      0.380 0.400 0.000 0.600 0.000
#&gt; GSM317655     4  0.2751      0.935 0.040 0.056 0.000 0.904
#&gt; GSM317659     1  0.1557      0.840 0.944 0.000 0.000 0.056
#&gt; GSM317661     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; GSM317662     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; GSM317668     1  0.4040      0.609 0.752 0.000 0.248 0.000
#&gt; GSM317669     3  0.0188      0.801 0.004 0.000 0.996 0.000
#&gt; GSM317671     3  0.0817      0.792 0.024 0.000 0.976 0.000
#&gt; GSM317676     1  0.4907      0.373 0.580 0.000 0.000 0.420
#&gt; GSM317680     3  0.0188      0.801 0.004 0.000 0.996 0.000
#&gt; GSM317684     1  0.1557      0.840 0.944 0.000 0.000 0.056
#&gt; GSM317685     1  0.0469      0.844 0.988 0.000 0.012 0.000
#&gt; GSM317694     1  0.1474      0.841 0.948 0.000 0.000 0.052
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-3-a').click(function(){
  $('#tab-SD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-4'>
<p><a id='tab-SD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     3  0.0404     0.8409 0.012 0.000 0.988 0.000 0.000
#&gt; GSM317652     3  0.3949     0.4558 0.332 0.000 0.668 0.000 0.000
#&gt; GSM317666     4  0.2629     0.7805 0.000 0.004 0.000 0.860 0.136
#&gt; GSM317672     1  0.7141     0.1796 0.440 0.368 0.148 0.044 0.000
#&gt; GSM317679     3  0.0290     0.8423 0.008 0.000 0.992 0.000 0.000
#&gt; GSM317681     3  0.8168     0.0827 0.192 0.148 0.404 0.256 0.000
#&gt; GSM317682     1  0.2719     0.7561 0.852 0.000 0.144 0.000 0.004
#&gt; GSM317683     2  0.0000     0.9910 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317689     2  0.0865     0.9709 0.004 0.972 0.000 0.024 0.000
#&gt; GSM317691     1  0.0510     0.8600 0.984 0.000 0.000 0.016 0.000
#&gt; GSM317692     1  0.1430     0.8448 0.944 0.004 0.000 0.052 0.000
#&gt; GSM317693     1  0.0510     0.8600 0.984 0.000 0.000 0.016 0.000
#&gt; GSM317696     1  0.0000     0.8638 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317697     1  0.0162     0.8630 0.996 0.000 0.000 0.004 0.000
#&gt; GSM317698     1  0.0000     0.8638 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317650     2  0.0000     0.9910 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317651     1  0.3398     0.6853 0.780 0.000 0.216 0.000 0.004
#&gt; GSM317657     4  0.3266     0.6691 0.000 0.200 0.000 0.796 0.004
#&gt; GSM317667     5  0.0162     1.0000 0.000 0.004 0.000 0.000 0.996
#&gt; GSM317670     1  0.6219     0.0655 0.440 0.420 0.000 0.140 0.000
#&gt; GSM317674     1  0.0000     0.8638 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317675     1  0.0000     0.8638 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317677     1  0.3274     0.7346 0.780 0.000 0.000 0.220 0.000
#&gt; GSM317678     2  0.0162     0.9897 0.000 0.996 0.000 0.004 0.000
#&gt; GSM317687     4  0.1608     0.7219 0.072 0.000 0.000 0.928 0.000
#&gt; GSM317695     1  0.3274     0.6636 0.780 0.000 0.220 0.000 0.000
#&gt; GSM317653     4  0.3088     0.6344 0.004 0.000 0.164 0.828 0.004
#&gt; GSM317656     1  0.0000     0.8638 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317658     1  0.3882     0.6862 0.756 0.224 0.000 0.020 0.000
#&gt; GSM317660     3  0.0324     0.8362 0.000 0.000 0.992 0.004 0.004
#&gt; GSM317663     4  0.3732     0.7605 0.000 0.032 0.000 0.792 0.176
#&gt; GSM317664     1  0.0000     0.8638 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317665     3  0.0162     0.8372 0.000 0.000 0.996 0.000 0.004
#&gt; GSM317673     1  0.0000     0.8638 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317686     5  0.0162     1.0000 0.000 0.004 0.000 0.000 0.996
#&gt; GSM317688     1  0.0000     0.8638 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317690     2  0.0566     0.9829 0.004 0.984 0.000 0.012 0.000
#&gt; GSM317654     3  0.2068     0.7741 0.092 0.000 0.904 0.000 0.004
#&gt; GSM317655     4  0.3562     0.7534 0.000 0.016 0.000 0.788 0.196
#&gt; GSM317659     1  0.3508     0.7074 0.748 0.000 0.000 0.252 0.000
#&gt; GSM317661     2  0.0000     0.9910 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317662     2  0.0000     0.9910 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317668     1  0.3913     0.5222 0.676 0.000 0.324 0.000 0.000
#&gt; GSM317669     3  0.0290     0.8423 0.008 0.000 0.992 0.000 0.000
#&gt; GSM317671     3  0.1121     0.8182 0.044 0.000 0.956 0.000 0.000
#&gt; GSM317676     4  0.0404     0.7667 0.012 0.000 0.000 0.988 0.000
#&gt; GSM317680     3  0.0290     0.8423 0.008 0.000 0.992 0.000 0.000
#&gt; GSM317684     1  0.1965     0.8240 0.904 0.000 0.000 0.096 0.000
#&gt; GSM317685     1  0.0000     0.8638 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317694     1  0.0290     0.8625 0.992 0.000 0.000 0.008 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-4-a').click(function(){
  $('#tab-SD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-5'>
<p><a id='tab-SD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM317649     3  0.0363      0.936 0.012 0.000 0.988 0.000 0.000 0.000
#&gt; GSM317652     3  0.4151      0.690 0.176 0.000 0.744 0.004 0.076 0.000
#&gt; GSM317666     4  0.1151      0.698 0.000 0.000 0.000 0.956 0.012 0.032
#&gt; GSM317672     5  0.2952      0.658 0.016 0.068 0.052 0.000 0.864 0.000
#&gt; GSM317679     3  0.0508      0.936 0.012 0.004 0.984 0.000 0.000 0.000
#&gt; GSM317681     5  0.4833      0.574 0.004 0.044 0.232 0.032 0.688 0.000
#&gt; GSM317682     1  0.2558      0.765 0.840 0.000 0.156 0.004 0.000 0.000
#&gt; GSM317683     2  0.0146      0.968 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM317689     5  0.1814      0.645 0.000 0.100 0.000 0.000 0.900 0.000
#&gt; GSM317691     1  0.0603      0.895 0.980 0.000 0.000 0.016 0.004 0.000
#&gt; GSM317692     5  0.4704      0.494 0.236 0.000 0.000 0.100 0.664 0.000
#&gt; GSM317693     1  0.0935      0.884 0.964 0.000 0.000 0.032 0.004 0.000
#&gt; GSM317696     1  0.0000      0.904 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317697     1  0.0291      0.901 0.992 0.000 0.000 0.004 0.004 0.000
#&gt; GSM317698     1  0.0146      0.903 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; GSM317650     2  0.0146      0.968 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM317651     1  0.3421      0.647 0.736 0.000 0.256 0.000 0.008 0.000
#&gt; GSM317657     5  0.3728      0.579 0.000 0.000 0.000 0.344 0.652 0.004
#&gt; GSM317667     6  0.0000      1.000 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM317670     5  0.2159      0.657 0.012 0.072 0.000 0.012 0.904 0.000
#&gt; GSM317674     1  0.0000      0.904 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317675     1  0.0000      0.904 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317677     4  0.3684      0.507 0.332 0.000 0.000 0.664 0.004 0.000
#&gt; GSM317678     2  0.1814      0.881 0.000 0.900 0.000 0.000 0.100 0.000
#&gt; GSM317687     4  0.0725      0.724 0.012 0.000 0.000 0.976 0.012 0.000
#&gt; GSM317695     1  0.4284      0.626 0.728 0.000 0.200 0.008 0.064 0.000
#&gt; GSM317653     5  0.5748      0.365 0.000 0.000 0.176 0.360 0.464 0.000
#&gt; GSM317656     1  0.0000      0.904 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317658     5  0.3189      0.562 0.184 0.020 0.000 0.000 0.796 0.000
#&gt; GSM317660     3  0.0291      0.928 0.000 0.004 0.992 0.000 0.004 0.000
#&gt; GSM317663     5  0.4026      0.570 0.000 0.000 0.000 0.348 0.636 0.016
#&gt; GSM317664     1  0.0000      0.904 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317665     3  0.0937      0.920 0.000 0.000 0.960 0.000 0.040 0.000
#&gt; GSM317673     1  0.0000      0.904 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317686     6  0.0000      1.000 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM317688     1  0.0000      0.904 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317690     5  0.3867     -0.108 0.000 0.488 0.000 0.000 0.512 0.000
#&gt; GSM317654     3  0.2006      0.890 0.016 0.000 0.904 0.000 0.080 0.000
#&gt; GSM317655     5  0.4938      0.538 0.000 0.000 0.000 0.340 0.580 0.080
#&gt; GSM317659     4  0.3109      0.626 0.224 0.000 0.000 0.772 0.004 0.000
#&gt; GSM317661     2  0.0363      0.964 0.000 0.988 0.000 0.000 0.012 0.000
#&gt; GSM317662     2  0.0146      0.968 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM317668     1  0.4535      0.485 0.644 0.000 0.296 0.000 0.060 0.000
#&gt; GSM317669     3  0.0508      0.936 0.012 0.004 0.984 0.000 0.000 0.000
#&gt; GSM317671     3  0.1082      0.914 0.040 0.000 0.956 0.004 0.000 0.000
#&gt; GSM317676     4  0.0363      0.716 0.000 0.000 0.000 0.988 0.012 0.000
#&gt; GSM317680     3  0.0363      0.936 0.012 0.000 0.988 0.000 0.000 0.000
#&gt; GSM317684     1  0.2871      0.707 0.804 0.000 0.000 0.192 0.004 0.000
#&gt; GSM317685     1  0.0000      0.904 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317694     1  0.0603      0.896 0.980 0.000 0.000 0.016 0.004 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-5-a').click(function(){
  $('#tab-SD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-mclust-signature_compare](figure_cola/SD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-mclust-collect-classes](figure_cola/SD-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) k
#> SD:mclust 47            0.710 2
#> SD:mclust 48            0.750 3
#> SD:mclust 43            0.918 4
#> SD:mclust 46            0.855 5
#> SD:mclust 46            0.701 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "NMF"]
# you can also extract it by
# res = res_list["SD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-NMF-collect-plots](figure_cola/SD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-NMF-select-partition-number](figure_cola/SD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.944       0.978         0.4613 0.530   0.530
#> 3 3 0.550           0.684       0.812         0.3717 0.776   0.602
#> 4 4 0.784           0.799       0.909         0.1569 0.770   0.467
#> 5 5 0.743           0.657       0.816         0.0793 0.909   0.676
#> 6 6 0.801           0.777       0.858         0.0403 0.922   0.669
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-classes'>
<ul>
<li><a href='#tab-SD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-NMF-get-classes-1'>
<p><a id='tab-SD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     1  0.0000      0.994 1.000 0.000
#&gt; GSM317652     1  0.0000      0.994 1.000 0.000
#&gt; GSM317666     2  0.0000      0.945 0.000 1.000
#&gt; GSM317672     2  0.2236      0.920 0.036 0.964
#&gt; GSM317679     1  0.0376      0.990 0.996 0.004
#&gt; GSM317681     2  0.0376      0.942 0.004 0.996
#&gt; GSM317682     1  0.0000      0.994 1.000 0.000
#&gt; GSM317683     2  0.0000      0.945 0.000 1.000
#&gt; GSM317689     2  0.0000      0.945 0.000 1.000
#&gt; GSM317691     1  0.0000      0.994 1.000 0.000
#&gt; GSM317692     1  0.6623      0.777 0.828 0.172
#&gt; GSM317693     1  0.0000      0.994 1.000 0.000
#&gt; GSM317696     1  0.0000      0.994 1.000 0.000
#&gt; GSM317697     1  0.0000      0.994 1.000 0.000
#&gt; GSM317698     1  0.0000      0.994 1.000 0.000
#&gt; GSM317650     2  0.0000      0.945 0.000 1.000
#&gt; GSM317651     1  0.0000      0.994 1.000 0.000
#&gt; GSM317657     2  0.0000      0.945 0.000 1.000
#&gt; GSM317667     2  0.0000      0.945 0.000 1.000
#&gt; GSM317670     2  0.2603      0.913 0.044 0.956
#&gt; GSM317674     1  0.0000      0.994 1.000 0.000
#&gt; GSM317675     1  0.0000      0.994 1.000 0.000
#&gt; GSM317677     1  0.0000      0.994 1.000 0.000
#&gt; GSM317678     2  0.0000      0.945 0.000 1.000
#&gt; GSM317687     1  0.0000      0.994 1.000 0.000
#&gt; GSM317695     1  0.0000      0.994 1.000 0.000
#&gt; GSM317653     2  0.9608      0.398 0.384 0.616
#&gt; GSM317656     1  0.0000      0.994 1.000 0.000
#&gt; GSM317658     2  0.9933      0.198 0.452 0.548
#&gt; GSM317660     1  0.0376      0.990 0.996 0.004
#&gt; GSM317663     2  0.0000      0.945 0.000 1.000
#&gt; GSM317664     1  0.0000      0.994 1.000 0.000
#&gt; GSM317665     1  0.0000      0.994 1.000 0.000
#&gt; GSM317673     1  0.0000      0.994 1.000 0.000
#&gt; GSM317686     2  0.0000      0.945 0.000 1.000
#&gt; GSM317688     1  0.0000      0.994 1.000 0.000
#&gt; GSM317690     2  0.0000      0.945 0.000 1.000
#&gt; GSM317654     1  0.0000      0.994 1.000 0.000
#&gt; GSM317655     2  0.0000      0.945 0.000 1.000
#&gt; GSM317659     1  0.0000      0.994 1.000 0.000
#&gt; GSM317661     2  0.0000      0.945 0.000 1.000
#&gt; GSM317662     2  0.0000      0.945 0.000 1.000
#&gt; GSM317668     1  0.0000      0.994 1.000 0.000
#&gt; GSM317669     1  0.0000      0.994 1.000 0.000
#&gt; GSM317671     1  0.0376      0.990 0.996 0.004
#&gt; GSM317676     1  0.0000      0.994 1.000 0.000
#&gt; GSM317680     1  0.0000      0.994 1.000 0.000
#&gt; GSM317684     1  0.0000      0.994 1.000 0.000
#&gt; GSM317685     1  0.0000      0.994 1.000 0.000
#&gt; GSM317694     1  0.0000      0.994 1.000 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-1-a').click(function(){
  $('#tab-SD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-2'>
<p><a id='tab-SD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM317649     1  0.0592      0.783 0.988 0.012 0.000
#&gt; GSM317652     1  0.0892      0.795 0.980 0.000 0.020
#&gt; GSM317666     3  0.4504      0.635 0.000 0.196 0.804
#&gt; GSM317672     2  0.3551      0.771 0.132 0.868 0.000
#&gt; GSM317679     1  0.1529      0.763 0.960 0.040 0.000
#&gt; GSM317681     2  0.4452      0.713 0.192 0.808 0.000
#&gt; GSM317682     1  0.0237      0.787 0.996 0.004 0.000
#&gt; GSM317683     2  0.0424      0.841 0.000 0.992 0.008
#&gt; GSM317689     2  0.0592      0.839 0.000 0.988 0.012
#&gt; GSM317691     1  0.6244      0.569 0.560 0.000 0.440
#&gt; GSM317692     1  0.7718      0.663 0.612 0.068 0.320
#&gt; GSM317693     3  0.6079     -0.163 0.388 0.000 0.612
#&gt; GSM317696     1  0.5138      0.793 0.748 0.000 0.252
#&gt; GSM317697     1  0.5465      0.773 0.712 0.000 0.288
#&gt; GSM317698     1  0.5650      0.752 0.688 0.000 0.312
#&gt; GSM317650     2  0.0747      0.843 0.016 0.984 0.000
#&gt; GSM317651     1  0.5058      0.796 0.756 0.000 0.244
#&gt; GSM317657     3  0.5327      0.591 0.000 0.272 0.728
#&gt; GSM317667     3  0.5621      0.576 0.000 0.308 0.692
#&gt; GSM317670     2  0.7226      0.530 0.076 0.688 0.236
#&gt; GSM317674     1  0.5291      0.785 0.732 0.000 0.268
#&gt; GSM317675     1  0.5465      0.773 0.712 0.000 0.288
#&gt; GSM317677     3  0.2711      0.621 0.088 0.000 0.912
#&gt; GSM317678     2  0.0892      0.842 0.020 0.980 0.000
#&gt; GSM317687     3  0.0747      0.654 0.016 0.000 0.984
#&gt; GSM317695     1  0.1643      0.799 0.956 0.000 0.044
#&gt; GSM317653     3  0.6675      0.428 0.012 0.404 0.584
#&gt; GSM317656     1  0.1289      0.798 0.968 0.000 0.032
#&gt; GSM317658     2  0.8562      0.349 0.208 0.608 0.184
#&gt; GSM317660     1  0.5988      0.167 0.632 0.368 0.000
#&gt; GSM317663     3  0.5733      0.560 0.000 0.324 0.676
#&gt; GSM317664     1  0.5138      0.793 0.748 0.000 0.252
#&gt; GSM317665     1  0.1031      0.775 0.976 0.024 0.000
#&gt; GSM317673     1  0.4750      0.800 0.784 0.000 0.216
#&gt; GSM317686     3  0.5926      0.517 0.000 0.356 0.644
#&gt; GSM317688     1  0.3879      0.804 0.848 0.000 0.152
#&gt; GSM317690     2  0.1163      0.828 0.000 0.972 0.028
#&gt; GSM317654     1  0.0592      0.791 0.988 0.000 0.012
#&gt; GSM317655     3  0.5591      0.579 0.000 0.304 0.696
#&gt; GSM317659     3  0.1643      0.646 0.044 0.000 0.956
#&gt; GSM317661     2  0.0000      0.844 0.000 1.000 0.000
#&gt; GSM317662     2  0.0000      0.844 0.000 1.000 0.000
#&gt; GSM317668     1  0.5216      0.789 0.740 0.000 0.260
#&gt; GSM317669     1  0.0747      0.781 0.984 0.016 0.000
#&gt; GSM317671     1  0.0424      0.785 0.992 0.008 0.000
#&gt; GSM317676     3  0.0747      0.654 0.016 0.000 0.984
#&gt; GSM317680     1  0.0592      0.783 0.988 0.012 0.000
#&gt; GSM317684     3  0.5363      0.256 0.276 0.000 0.724
#&gt; GSM317685     1  0.5254      0.787 0.736 0.000 0.264
#&gt; GSM317694     1  0.5882      0.714 0.652 0.000 0.348
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-2-a').click(function(){
  $('#tab-SD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-3'>
<p><a id='tab-SD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     3  0.0707      0.908 0.020 0.000 0.980 0.000
#&gt; GSM317652     3  0.2676      0.880 0.092 0.012 0.896 0.000
#&gt; GSM317666     4  0.0188      0.828 0.000 0.004 0.000 0.996
#&gt; GSM317672     2  0.0336      0.880 0.000 0.992 0.008 0.000
#&gt; GSM317679     3  0.0707      0.908 0.020 0.000 0.980 0.000
#&gt; GSM317681     3  0.4632      0.566 0.000 0.308 0.688 0.004
#&gt; GSM317682     1  0.5010      0.557 0.700 0.024 0.276 0.000
#&gt; GSM317683     2  0.0336      0.882 0.000 0.992 0.000 0.008
#&gt; GSM317689     2  0.0779      0.877 0.000 0.980 0.004 0.016
#&gt; GSM317691     1  0.0336      0.904 0.992 0.000 0.000 0.008
#&gt; GSM317692     1  0.0469      0.901 0.988 0.012 0.000 0.000
#&gt; GSM317693     1  0.0188      0.905 0.996 0.000 0.000 0.004
#&gt; GSM317696     1  0.0000      0.906 1.000 0.000 0.000 0.000
#&gt; GSM317697     1  0.0000      0.906 1.000 0.000 0.000 0.000
#&gt; GSM317698     1  0.0000      0.906 1.000 0.000 0.000 0.000
#&gt; GSM317650     2  0.0188      0.883 0.000 0.996 0.000 0.004
#&gt; GSM317651     3  0.3448      0.805 0.168 0.004 0.828 0.000
#&gt; GSM317657     4  0.7771      0.112 0.396 0.200 0.004 0.400
#&gt; GSM317667     4  0.0469      0.827 0.000 0.012 0.000 0.988
#&gt; GSM317670     2  0.6161      0.640 0.176 0.716 0.072 0.036
#&gt; GSM317674     1  0.0000      0.906 1.000 0.000 0.000 0.000
#&gt; GSM317675     1  0.0000      0.906 1.000 0.000 0.000 0.000
#&gt; GSM317677     1  0.2281      0.829 0.904 0.000 0.000 0.096
#&gt; GSM317678     2  0.0188      0.882 0.000 0.996 0.004 0.000
#&gt; GSM317687     4  0.4382      0.570 0.296 0.000 0.000 0.704
#&gt; GSM317695     3  0.3583      0.773 0.180 0.000 0.816 0.004
#&gt; GSM317653     4  0.4296      0.738 0.008 0.076 0.084 0.832
#&gt; GSM317656     1  0.4277      0.604 0.720 0.000 0.280 0.000
#&gt; GSM317658     2  0.5929      0.155 0.448 0.520 0.004 0.028
#&gt; GSM317660     3  0.1743      0.888 0.004 0.056 0.940 0.000
#&gt; GSM317663     4  0.0188      0.828 0.000 0.004 0.000 0.996
#&gt; GSM317664     1  0.0188      0.905 0.996 0.000 0.000 0.004
#&gt; GSM317665     3  0.1256      0.900 0.008 0.028 0.964 0.000
#&gt; GSM317673     1  0.0000      0.906 1.000 0.000 0.000 0.000
#&gt; GSM317686     4  0.0707      0.823 0.000 0.020 0.000 0.980
#&gt; GSM317688     1  0.0376      0.904 0.992 0.004 0.004 0.000
#&gt; GSM317690     2  0.1661      0.852 0.000 0.944 0.004 0.052
#&gt; GSM317654     3  0.1820      0.902 0.036 0.020 0.944 0.000
#&gt; GSM317655     4  0.0376      0.828 0.000 0.004 0.004 0.992
#&gt; GSM317659     1  0.4356      0.534 0.708 0.000 0.000 0.292
#&gt; GSM317661     2  0.0779      0.878 0.000 0.980 0.016 0.004
#&gt; GSM317662     2  0.0376      0.883 0.000 0.992 0.004 0.004
#&gt; GSM317668     1  0.5138      0.365 0.600 0.000 0.392 0.008
#&gt; GSM317669     3  0.0336      0.905 0.008 0.000 0.992 0.000
#&gt; GSM317671     3  0.1109      0.904 0.028 0.000 0.968 0.004
#&gt; GSM317676     4  0.2704      0.770 0.124 0.000 0.000 0.876
#&gt; GSM317680     3  0.0469      0.907 0.012 0.000 0.988 0.000
#&gt; GSM317684     1  0.0592      0.897 0.984 0.000 0.000 0.016
#&gt; GSM317685     1  0.0336      0.903 0.992 0.000 0.008 0.000
#&gt; GSM317694     1  0.0000      0.906 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-3-a').click(function(){
  $('#tab-SD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-4'>
<p><a id='tab-SD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     3  0.0404     0.6335 0.000 0.000 0.988 0.000 0.012
#&gt; GSM317652     3  0.5341    -0.2521 0.060 0.000 0.564 0.000 0.376
#&gt; GSM317666     4  0.0000     0.8496 0.000 0.000 0.000 1.000 0.000
#&gt; GSM317672     2  0.3160     0.7775 0.000 0.808 0.004 0.000 0.188
#&gt; GSM317679     3  0.0000     0.6402 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317681     5  0.6834     0.4412 0.004 0.048 0.268 0.120 0.560
#&gt; GSM317682     1  0.3039     0.6754 0.808 0.000 0.000 0.000 0.192
#&gt; GSM317683     2  0.0162     0.9105 0.000 0.996 0.000 0.000 0.004
#&gt; GSM317689     2  0.0000     0.9103 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317691     1  0.3452     0.7382 0.756 0.000 0.000 0.000 0.244
#&gt; GSM317692     1  0.0404     0.8773 0.988 0.000 0.000 0.000 0.012
#&gt; GSM317693     1  0.0000     0.8783 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317696     1  0.0162     0.8783 0.996 0.000 0.004 0.000 0.000
#&gt; GSM317697     1  0.0162     0.8787 0.996 0.000 0.000 0.000 0.004
#&gt; GSM317698     1  0.0703     0.8766 0.976 0.000 0.000 0.000 0.024
#&gt; GSM317650     2  0.0162     0.9110 0.000 0.996 0.000 0.000 0.004
#&gt; GSM317651     5  0.6032     0.2280 0.388 0.000 0.120 0.000 0.492
#&gt; GSM317657     4  0.6506     0.5252 0.016 0.152 0.000 0.536 0.296
#&gt; GSM317667     4  0.0794     0.8461 0.000 0.000 0.000 0.972 0.028
#&gt; GSM317670     5  0.6500    -0.2371 0.000 0.404 0.188 0.000 0.408
#&gt; GSM317674     1  0.1956     0.8588 0.916 0.000 0.008 0.000 0.076
#&gt; GSM317675     1  0.0963     0.8740 0.964 0.000 0.000 0.000 0.036
#&gt; GSM317677     1  0.5441     0.6084 0.624 0.000 0.000 0.096 0.280
#&gt; GSM317678     2  0.1792     0.8821 0.000 0.916 0.000 0.000 0.084
#&gt; GSM317687     4  0.3039     0.7269 0.152 0.000 0.000 0.836 0.012
#&gt; GSM317695     3  0.2488     0.6180 0.004 0.000 0.872 0.000 0.124
#&gt; GSM317653     5  0.5015     0.1655 0.000 0.004 0.028 0.392 0.576
#&gt; GSM317656     3  0.6392     0.1208 0.400 0.000 0.432 0.000 0.168
#&gt; GSM317658     2  0.1195     0.8911 0.028 0.960 0.000 0.000 0.012
#&gt; GSM317660     5  0.4291     0.3694 0.000 0.000 0.464 0.000 0.536
#&gt; GSM317663     4  0.0794     0.8488 0.000 0.000 0.000 0.972 0.028
#&gt; GSM317664     1  0.3779     0.7681 0.776 0.000 0.024 0.000 0.200
#&gt; GSM317665     5  0.4305     0.3322 0.000 0.000 0.488 0.000 0.512
#&gt; GSM317673     1  0.0162     0.8779 0.996 0.000 0.000 0.000 0.004
#&gt; GSM317686     4  0.0162     0.8500 0.000 0.004 0.000 0.996 0.000
#&gt; GSM317688     1  0.0510     0.8731 0.984 0.000 0.000 0.000 0.016
#&gt; GSM317690     2  0.0566     0.9059 0.000 0.984 0.000 0.004 0.012
#&gt; GSM317654     5  0.4597     0.4033 0.000 0.000 0.424 0.012 0.564
#&gt; GSM317655     4  0.2773     0.8107 0.000 0.000 0.000 0.836 0.164
#&gt; GSM317659     1  0.6417    -0.0228 0.424 0.000 0.000 0.404 0.172
#&gt; GSM317661     2  0.3835     0.6693 0.000 0.732 0.000 0.008 0.260
#&gt; GSM317662     2  0.0963     0.9060 0.000 0.964 0.000 0.000 0.036
#&gt; GSM317668     3  0.6055     0.2797 0.120 0.000 0.472 0.000 0.408
#&gt; GSM317669     3  0.1043     0.6064 0.000 0.000 0.960 0.000 0.040
#&gt; GSM317671     3  0.2424     0.6149 0.000 0.000 0.868 0.000 0.132
#&gt; GSM317676     4  0.3010     0.7750 0.004 0.000 0.000 0.824 0.172
#&gt; GSM317680     3  0.0000     0.6402 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317684     1  0.0162     0.8785 0.996 0.000 0.000 0.000 0.004
#&gt; GSM317685     1  0.0404     0.8766 0.988 0.000 0.000 0.000 0.012
#&gt; GSM317694     1  0.1965     0.8487 0.904 0.000 0.000 0.000 0.096
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-4-a').click(function(){
  $('#tab-SD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-5'>
<p><a id='tab-SD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM317649     3  0.2173      0.934 0.004 0.000 0.904 0.028 0.064 0.000
#&gt; GSM317652     5  0.4938      0.626 0.024 0.000 0.216 0.080 0.680 0.000
#&gt; GSM317666     6  0.0146      0.905 0.000 0.000 0.000 0.000 0.004 0.996
#&gt; GSM317672     2  0.2653      0.831 0.000 0.844 0.000 0.012 0.144 0.000
#&gt; GSM317679     3  0.0972      0.964 0.000 0.000 0.964 0.008 0.028 0.000
#&gt; GSM317681     5  0.3804      0.791 0.012 0.036 0.016 0.032 0.836 0.068
#&gt; GSM317682     1  0.1863      0.853 0.920 0.000 0.000 0.044 0.036 0.000
#&gt; GSM317683     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317689     2  0.0458      0.942 0.000 0.984 0.000 0.016 0.000 0.000
#&gt; GSM317691     1  0.3047      0.816 0.832 0.000 0.012 0.144 0.008 0.004
#&gt; GSM317692     1  0.2222      0.840 0.896 0.012 0.000 0.084 0.008 0.000
#&gt; GSM317693     1  0.1204      0.861 0.944 0.000 0.000 0.056 0.000 0.000
#&gt; GSM317696     1  0.0458      0.869 0.984 0.000 0.000 0.016 0.000 0.000
#&gt; GSM317697     1  0.0858      0.868 0.968 0.004 0.000 0.028 0.000 0.000
#&gt; GSM317698     1  0.2275      0.852 0.888 0.000 0.008 0.096 0.008 0.000
#&gt; GSM317650     2  0.0260      0.943 0.000 0.992 0.000 0.008 0.000 0.000
#&gt; GSM317651     5  0.2213      0.809 0.044 0.000 0.004 0.048 0.904 0.000
#&gt; GSM317657     4  0.5916      0.192 0.008 0.120 0.004 0.464 0.004 0.400
#&gt; GSM317667     6  0.0146      0.905 0.000 0.000 0.000 0.000 0.004 0.996
#&gt; GSM317670     4  0.3869      0.489 0.000 0.168 0.060 0.768 0.004 0.000
#&gt; GSM317674     1  0.2917      0.827 0.840 0.000 0.016 0.136 0.008 0.000
#&gt; GSM317675     1  0.2845      0.824 0.836 0.000 0.008 0.148 0.008 0.000
#&gt; GSM317677     1  0.5431      0.429 0.560 0.000 0.016 0.356 0.012 0.056
#&gt; GSM317678     2  0.1092      0.939 0.000 0.960 0.000 0.020 0.020 0.000
#&gt; GSM317687     6  0.3513      0.717 0.104 0.000 0.000 0.072 0.008 0.816
#&gt; GSM317695     3  0.0405      0.958 0.000 0.000 0.988 0.004 0.008 0.000
#&gt; GSM317653     5  0.1556      0.816 0.000 0.000 0.000 0.000 0.920 0.080
#&gt; GSM317656     4  0.7086      0.112 0.300 0.000 0.288 0.344 0.068 0.000
#&gt; GSM317658     2  0.1218      0.927 0.028 0.956 0.000 0.012 0.004 0.000
#&gt; GSM317660     5  0.1777      0.840 0.004 0.000 0.044 0.024 0.928 0.000
#&gt; GSM317663     6  0.2119      0.866 0.000 0.008 0.016 0.060 0.004 0.912
#&gt; GSM317664     1  0.4406      0.709 0.712 0.000 0.056 0.220 0.012 0.000
#&gt; GSM317665     5  0.1838      0.835 0.000 0.000 0.068 0.016 0.916 0.000
#&gt; GSM317673     1  0.1333      0.867 0.944 0.000 0.000 0.048 0.008 0.000
#&gt; GSM317686     6  0.0405      0.901 0.000 0.008 0.000 0.004 0.000 0.988
#&gt; GSM317688     1  0.2009      0.865 0.908 0.000 0.000 0.068 0.024 0.000
#&gt; GSM317690     2  0.1010      0.931 0.000 0.960 0.000 0.036 0.004 0.000
#&gt; GSM317654     5  0.1461      0.841 0.000 0.000 0.044 0.000 0.940 0.016
#&gt; GSM317655     4  0.4346      0.382 0.000 0.004 0.000 0.632 0.028 0.336
#&gt; GSM317659     4  0.6278      0.346 0.252 0.000 0.000 0.460 0.016 0.272
#&gt; GSM317661     5  0.3922      0.495 0.000 0.320 0.000 0.016 0.664 0.000
#&gt; GSM317662     2  0.1531      0.917 0.000 0.928 0.000 0.004 0.068 0.000
#&gt; GSM317668     4  0.3134      0.505 0.016 0.000 0.148 0.824 0.012 0.000
#&gt; GSM317669     3  0.1349      0.954 0.000 0.000 0.940 0.004 0.056 0.000
#&gt; GSM317671     3  0.0260      0.953 0.000 0.000 0.992 0.008 0.000 0.000
#&gt; GSM317676     4  0.3993      0.339 0.000 0.000 0.000 0.592 0.008 0.400
#&gt; GSM317680     3  0.0972      0.964 0.000 0.000 0.964 0.008 0.028 0.000
#&gt; GSM317684     1  0.2195      0.844 0.904 0.000 0.000 0.068 0.012 0.016
#&gt; GSM317685     1  0.1625      0.863 0.928 0.000 0.000 0.060 0.012 0.000
#&gt; GSM317694     1  0.2001      0.860 0.900 0.000 0.004 0.092 0.004 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-5-a').click(function(){
  $('#tab-SD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-NMF-signature_compare](figure_cola/SD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-SD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-NMF-collect-classes](figure_cola/SD-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n disease.state(p) k
#> SD:NMF 48            0.719 2
#> SD:NMF 45            0.625 3
#> SD:NMF 47            0.901 4
#> SD:NMF 39            0.775 5
#> SD:NMF 42            0.581 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:hclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "hclust"]
# you can also extract it by
# res = res_list["CV:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-hclust-collect-plots](figure_cola/CV-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-hclust-select-partition-number](figure_cola/CV-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.958           0.952       0.984         0.0582 0.960   0.960
#> 3 3 0.636           0.828       0.936         2.2811 0.887   0.883
#> 4 4 0.466           0.660       0.878         0.5847 0.928   0.915
#> 5 5 0.355           0.659       0.863         0.2545 0.965   0.955
#> 6 6 0.385           0.640       0.838         0.1009 0.999   0.999
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-classes'>
<ul>
<li><a href='#tab-CV-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-hclust-get-classes-1'>
<p><a id='tab-CV-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     1  0.0000      0.985 1.000 0.000
#&gt; GSM317652     1  0.0000      0.985 1.000 0.000
#&gt; GSM317666     1  0.5737      0.835 0.864 0.136
#&gt; GSM317672     1  0.0000      0.985 1.000 0.000
#&gt; GSM317679     1  0.0000      0.985 1.000 0.000
#&gt; GSM317681     1  0.0000      0.985 1.000 0.000
#&gt; GSM317682     1  0.0000      0.985 1.000 0.000
#&gt; GSM317683     1  0.1414      0.970 0.980 0.020
#&gt; GSM317689     1  0.0938      0.977 0.988 0.012
#&gt; GSM317691     1  0.0672      0.981 0.992 0.008
#&gt; GSM317692     1  0.0000      0.985 1.000 0.000
#&gt; GSM317693     1  0.0672      0.981 0.992 0.008
#&gt; GSM317696     1  0.0000      0.985 1.000 0.000
#&gt; GSM317697     1  0.0000      0.985 1.000 0.000
#&gt; GSM317698     1  0.0000      0.985 1.000 0.000
#&gt; GSM317650     2  0.5946      0.000 0.144 0.856
#&gt; GSM317651     1  0.0000      0.985 1.000 0.000
#&gt; GSM317657     1  0.0672      0.981 0.992 0.008
#&gt; GSM317667     1  0.5946      0.824 0.856 0.144
#&gt; GSM317670     1  0.0000      0.985 1.000 0.000
#&gt; GSM317674     1  0.0000      0.985 1.000 0.000
#&gt; GSM317675     1  0.0000      0.985 1.000 0.000
#&gt; GSM317677     1  0.0000      0.985 1.000 0.000
#&gt; GSM317678     1  0.1184      0.973 0.984 0.016
#&gt; GSM317687     1  0.0672      0.981 0.992 0.008
#&gt; GSM317695     1  0.0000      0.985 1.000 0.000
#&gt; GSM317653     1  0.0672      0.981 0.992 0.008
#&gt; GSM317656     1  0.0000      0.985 1.000 0.000
#&gt; GSM317658     1  0.0672      0.981 0.992 0.008
#&gt; GSM317660     1  0.0000      0.985 1.000 0.000
#&gt; GSM317663     1  0.3879      0.911 0.924 0.076
#&gt; GSM317664     1  0.0000      0.985 1.000 0.000
#&gt; GSM317665     1  0.0000      0.985 1.000 0.000
#&gt; GSM317673     1  0.0000      0.985 1.000 0.000
#&gt; GSM317686     1  0.5842      0.830 0.860 0.140
#&gt; GSM317688     1  0.0000      0.985 1.000 0.000
#&gt; GSM317690     1  0.0376      0.983 0.996 0.004
#&gt; GSM317654     1  0.0000      0.985 1.000 0.000
#&gt; GSM317655     1  0.0000      0.985 1.000 0.000
#&gt; GSM317659     1  0.0000      0.985 1.000 0.000
#&gt; GSM317661     1  0.1184      0.973 0.984 0.016
#&gt; GSM317662     1  0.1184      0.973 0.984 0.016
#&gt; GSM317668     1  0.0000      0.985 1.000 0.000
#&gt; GSM317669     1  0.0000      0.985 1.000 0.000
#&gt; GSM317671     1  0.0000      0.985 1.000 0.000
#&gt; GSM317676     1  0.0376      0.983 0.996 0.004
#&gt; GSM317680     1  0.0000      0.985 1.000 0.000
#&gt; GSM317684     1  0.0672      0.981 0.992 0.008
#&gt; GSM317685     1  0.0000      0.985 1.000 0.000
#&gt; GSM317694     1  0.0000      0.985 1.000 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-1-a').click(function(){
  $('#tab-CV-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-2'>
<p><a id='tab-CV-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM317649     1  0.1031      0.921 0.976 0.024 0.000
#&gt; GSM317652     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317666     2  0.6252      0.576 0.444 0.556 0.000
#&gt; GSM317672     1  0.1031      0.924 0.976 0.024 0.000
#&gt; GSM317679     1  0.1031      0.921 0.976 0.024 0.000
#&gt; GSM317681     1  0.1643      0.908 0.956 0.044 0.000
#&gt; GSM317682     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317683     1  0.4609      0.770 0.844 0.128 0.028
#&gt; GSM317689     1  0.3587      0.841 0.892 0.088 0.020
#&gt; GSM317691     1  0.0424      0.928 0.992 0.008 0.000
#&gt; GSM317692     1  0.0892      0.923 0.980 0.020 0.000
#&gt; GSM317693     1  0.0424      0.928 0.992 0.008 0.000
#&gt; GSM317696     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317697     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317698     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317650     3  0.0000      0.000 0.000 0.000 1.000
#&gt; GSM317651     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317657     1  0.0892      0.926 0.980 0.020 0.000
#&gt; GSM317667     2  0.2711     -0.125 0.088 0.912 0.000
#&gt; GSM317670     1  0.2537      0.873 0.920 0.080 0.000
#&gt; GSM317674     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317675     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317677     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317678     1  0.4045      0.813 0.872 0.104 0.024
#&gt; GSM317687     1  0.0424      0.928 0.992 0.008 0.000
#&gt; GSM317695     1  0.1031      0.921 0.976 0.024 0.000
#&gt; GSM317653     1  0.3752      0.778 0.856 0.144 0.000
#&gt; GSM317656     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317658     1  0.0424      0.928 0.992 0.008 0.000
#&gt; GSM317660     1  0.2625      0.869 0.916 0.084 0.000
#&gt; GSM317663     1  0.5810      0.224 0.664 0.336 0.000
#&gt; GSM317664     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317665     1  0.1643      0.908 0.956 0.044 0.000
#&gt; GSM317673     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317686     2  0.6008      0.623 0.372 0.628 0.000
#&gt; GSM317688     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317690     1  0.3686      0.797 0.860 0.140 0.000
#&gt; GSM317654     1  0.1643      0.908 0.956 0.044 0.000
#&gt; GSM317655     1  0.2537      0.873 0.920 0.080 0.000
#&gt; GSM317659     1  0.0747      0.926 0.984 0.016 0.000
#&gt; GSM317661     1  0.5903      0.595 0.744 0.232 0.024
#&gt; GSM317662     1  0.6105      0.544 0.724 0.252 0.024
#&gt; GSM317668     1  0.2066      0.892 0.940 0.060 0.000
#&gt; GSM317669     1  0.1643      0.908 0.956 0.044 0.000
#&gt; GSM317671     1  0.1031      0.921 0.976 0.024 0.000
#&gt; GSM317676     1  0.1529      0.913 0.960 0.040 0.000
#&gt; GSM317680     1  0.1031      0.921 0.976 0.024 0.000
#&gt; GSM317684     1  0.0424      0.928 0.992 0.008 0.000
#&gt; GSM317685     1  0.0000      0.930 1.000 0.000 0.000
#&gt; GSM317694     1  0.0000      0.930 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-2-a').click(function(){
  $('#tab-CV-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-3'>
<p><a id='tab-CV-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     1  0.1940    0.79550 0.924 0.076 0.000 0.000
#&gt; GSM317652     1  0.0188    0.83646 0.996 0.004 0.000 0.000
#&gt; GSM317666     4  0.4769    0.22399 0.308 0.008 0.000 0.684
#&gt; GSM317672     1  0.2586    0.78251 0.912 0.040 0.000 0.048
#&gt; GSM317679     1  0.1940    0.79550 0.924 0.076 0.000 0.000
#&gt; GSM317681     1  0.2760    0.74017 0.872 0.128 0.000 0.000
#&gt; GSM317682     1  0.0000    0.83742 1.000 0.000 0.000 0.000
#&gt; GSM317683     1  0.6830   -0.17577 0.620 0.188 0.004 0.188
#&gt; GSM317689     1  0.5361    0.44893 0.744 0.108 0.000 0.148
#&gt; GSM317691     1  0.1004    0.83166 0.972 0.004 0.000 0.024
#&gt; GSM317692     1  0.2483    0.77709 0.916 0.032 0.000 0.052
#&gt; GSM317693     1  0.0336    0.83619 0.992 0.000 0.000 0.008
#&gt; GSM317696     1  0.0000    0.83742 1.000 0.000 0.000 0.000
#&gt; GSM317697     1  0.0336    0.83646 0.992 0.000 0.000 0.008
#&gt; GSM317698     1  0.0000    0.83742 1.000 0.000 0.000 0.000
#&gt; GSM317650     3  0.0000    0.00000 0.000 0.000 1.000 0.000
#&gt; GSM317651     1  0.0524    0.83683 0.988 0.004 0.000 0.008
#&gt; GSM317657     1  0.2142    0.80207 0.928 0.016 0.000 0.056
#&gt; GSM317667     4  0.4720    0.00681 0.044 0.188 0.000 0.768
#&gt; GSM317670     1  0.4152    0.60879 0.808 0.032 0.000 0.160
#&gt; GSM317674     1  0.0000    0.83742 1.000 0.000 0.000 0.000
#&gt; GSM317675     1  0.0000    0.83742 1.000 0.000 0.000 0.000
#&gt; GSM317677     1  0.0000    0.83742 1.000 0.000 0.000 0.000
#&gt; GSM317678     1  0.6163    0.16859 0.676 0.160 0.000 0.164
#&gt; GSM317687     1  0.0524    0.83572 0.988 0.004 0.000 0.008
#&gt; GSM317695     1  0.1940    0.79550 0.924 0.076 0.000 0.000
#&gt; GSM317653     1  0.4718    0.61022 0.792 0.092 0.000 0.116
#&gt; GSM317656     1  0.0000    0.83742 1.000 0.000 0.000 0.000
#&gt; GSM317658     1  0.1004    0.83166 0.972 0.004 0.000 0.024
#&gt; GSM317660     1  0.5263   -0.10617 0.544 0.448 0.000 0.008
#&gt; GSM317663     1  0.5755   -0.36562 0.528 0.028 0.000 0.444
#&gt; GSM317664     1  0.0000    0.83742 1.000 0.000 0.000 0.000
#&gt; GSM317665     1  0.2704    0.74394 0.876 0.124 0.000 0.000
#&gt; GSM317673     1  0.0000    0.83742 1.000 0.000 0.000 0.000
#&gt; GSM317686     4  0.3870    0.45281 0.208 0.004 0.000 0.788
#&gt; GSM317688     1  0.0188    0.83706 0.996 0.000 0.000 0.004
#&gt; GSM317690     1  0.5083    0.37742 0.716 0.036 0.000 0.248
#&gt; GSM317654     1  0.2704    0.74394 0.876 0.124 0.000 0.000
#&gt; GSM317655     1  0.4152    0.60879 0.808 0.032 0.000 0.160
#&gt; GSM317659     1  0.1398    0.82144 0.956 0.004 0.000 0.040
#&gt; GSM317661     2  0.7740    0.95527 0.364 0.404 0.000 0.232
#&gt; GSM317662     2  0.7811    0.95495 0.368 0.380 0.000 0.252
#&gt; GSM317668     1  0.3760    0.66249 0.836 0.028 0.000 0.136
#&gt; GSM317669     1  0.2704    0.74394 0.876 0.124 0.000 0.000
#&gt; GSM317671     1  0.1940    0.79550 0.924 0.076 0.000 0.000
#&gt; GSM317676     1  0.3266    0.72856 0.868 0.024 0.000 0.108
#&gt; GSM317680     1  0.1940    0.79550 0.924 0.076 0.000 0.000
#&gt; GSM317684     1  0.0524    0.83572 0.988 0.004 0.000 0.008
#&gt; GSM317685     1  0.0000    0.83742 1.000 0.000 0.000 0.000
#&gt; GSM317694     1  0.0000    0.83742 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-3-a').click(function(){
  $('#tab-CV-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-4'>
<p><a id='tab-CV-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     1  0.3010      0.715 0.824 0.004 0.172 0.000 0.000
#&gt; GSM317652     1  0.0290      0.830 0.992 0.000 0.008 0.000 0.000
#&gt; GSM317666     4  0.4975      0.405 0.272 0.040 0.012 0.676 0.000
#&gt; GSM317672     1  0.3099      0.789 0.880 0.040 0.040 0.040 0.000
#&gt; GSM317679     1  0.3010      0.715 0.824 0.004 0.172 0.000 0.000
#&gt; GSM317681     1  0.3480      0.604 0.752 0.000 0.248 0.000 0.000
#&gt; GSM317682     1  0.0324      0.831 0.992 0.004 0.004 0.000 0.000
#&gt; GSM317683     1  0.6169      0.352 0.600 0.268 0.016 0.112 0.004
#&gt; GSM317689     1  0.5809      0.572 0.692 0.152 0.060 0.096 0.000
#&gt; GSM317691     1  0.0932      0.828 0.972 0.004 0.004 0.020 0.000
#&gt; GSM317692     1  0.2853      0.788 0.892 0.036 0.028 0.044 0.000
#&gt; GSM317693     1  0.0290      0.830 0.992 0.000 0.000 0.008 0.000
#&gt; GSM317696     1  0.0000      0.831 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317697     1  0.0290      0.831 0.992 0.000 0.000 0.008 0.000
#&gt; GSM317698     1  0.0000      0.831 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317650     5  0.0000      0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM317651     1  0.0613      0.832 0.984 0.008 0.004 0.004 0.000
#&gt; GSM317657     1  0.2060      0.809 0.924 0.016 0.008 0.052 0.000
#&gt; GSM317667     4  0.3081     -0.037 0.000 0.012 0.156 0.832 0.000
#&gt; GSM317670     1  0.4986      0.652 0.748 0.068 0.036 0.148 0.000
#&gt; GSM317674     1  0.0000      0.831 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317675     1  0.0000      0.831 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317677     1  0.0000      0.831 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317678     1  0.6001      0.472 0.648 0.216 0.040 0.096 0.000
#&gt; GSM317687     1  0.0451      0.830 0.988 0.000 0.004 0.008 0.000
#&gt; GSM317695     1  0.3010      0.715 0.824 0.004 0.172 0.000 0.000
#&gt; GSM317653     1  0.5293      0.565 0.720 0.024 0.120 0.136 0.000
#&gt; GSM317656     1  0.0162      0.831 0.996 0.000 0.004 0.000 0.000
#&gt; GSM317658     1  0.0932      0.828 0.972 0.004 0.004 0.020 0.000
#&gt; GSM317660     3  0.3452      0.000 0.244 0.000 0.756 0.000 0.000
#&gt; GSM317663     1  0.6258     -0.112 0.472 0.072 0.028 0.428 0.000
#&gt; GSM317664     1  0.0000      0.831 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317665     1  0.3452      0.612 0.756 0.000 0.244 0.000 0.000
#&gt; GSM317673     1  0.0451      0.830 0.988 0.004 0.008 0.000 0.000
#&gt; GSM317686     4  0.4190      0.479 0.172 0.060 0.000 0.768 0.000
#&gt; GSM317688     1  0.0290      0.831 0.992 0.000 0.000 0.008 0.000
#&gt; GSM317690     1  0.5812      0.509 0.656 0.080 0.036 0.228 0.000
#&gt; GSM317654     1  0.3452      0.614 0.756 0.000 0.244 0.000 0.000
#&gt; GSM317655     1  0.4986      0.652 0.748 0.068 0.036 0.148 0.000
#&gt; GSM317659     1  0.1285      0.822 0.956 0.004 0.004 0.036 0.000
#&gt; GSM317661     2  0.4534      0.599 0.072 0.796 0.068 0.064 0.000
#&gt; GSM317662     2  0.2238      0.551 0.004 0.912 0.064 0.020 0.000
#&gt; GSM317668     1  0.4223      0.700 0.796 0.060 0.016 0.128 0.000
#&gt; GSM317669     1  0.3480      0.605 0.752 0.000 0.248 0.000 0.000
#&gt; GSM317671     1  0.3010      0.715 0.824 0.004 0.172 0.000 0.000
#&gt; GSM317676     1  0.3265      0.762 0.856 0.040 0.008 0.096 0.000
#&gt; GSM317680     1  0.3010      0.715 0.824 0.004 0.172 0.000 0.000
#&gt; GSM317684     1  0.0451      0.830 0.988 0.000 0.004 0.008 0.000
#&gt; GSM317685     1  0.0000      0.831 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317694     1  0.0000      0.831 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-4-a').click(function(){
  $('#tab-CV-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-5'>
<p><a id='tab-CV-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM317649     1  0.3052     0.7388 0.780 0.000 0.000 0.004 0.216 0.000
#&gt; GSM317652     1  0.0363     0.8312 0.988 0.000 0.000 0.000 0.012 0.000
#&gt; GSM317666     4  0.4999     0.4514 0.244 0.000 0.080 0.660 0.012 0.004
#&gt; GSM317672     1  0.3072     0.7862 0.856 0.000 0.084 0.024 0.036 0.000
#&gt; GSM317679     1  0.3081     0.7363 0.776 0.000 0.000 0.004 0.220 0.000
#&gt; GSM317681     1  0.3653     0.6545 0.692 0.000 0.008 0.000 0.300 0.000
#&gt; GSM317682     1  0.0260     0.8316 0.992 0.000 0.008 0.000 0.000 0.000
#&gt; GSM317683     1  0.5743     0.3831 0.584 0.004 0.292 0.076 0.000 0.044
#&gt; GSM317689     1  0.5884     0.5593 0.628 0.000 0.216 0.064 0.080 0.012
#&gt; GSM317691     1  0.0837     0.8287 0.972 0.000 0.004 0.020 0.004 0.000
#&gt; GSM317692     1  0.2976     0.7811 0.860 0.000 0.088 0.028 0.024 0.000
#&gt; GSM317693     1  0.0260     0.8303 0.992 0.000 0.000 0.008 0.000 0.000
#&gt; GSM317696     1  0.0000     0.8311 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317697     1  0.0291     0.8314 0.992 0.000 0.004 0.004 0.000 0.000
#&gt; GSM317698     1  0.0000     0.8311 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317650     2  0.0000     0.0000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317651     1  0.0551     0.8319 0.984 0.000 0.004 0.008 0.004 0.000
#&gt; GSM317657     1  0.2594     0.8026 0.892 0.000 0.048 0.040 0.016 0.004
#&gt; GSM317667     4  0.3048     0.0215 0.000 0.000 0.004 0.824 0.020 0.152
#&gt; GSM317670     1  0.5696     0.6026 0.668 0.000 0.120 0.152 0.032 0.028
#&gt; GSM317674     1  0.0000     0.8311 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317675     1  0.0000     0.8311 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317677     1  0.0000     0.8311 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317678     1  0.5713     0.4957 0.620 0.000 0.264 0.048 0.036 0.032
#&gt; GSM317687     1  0.0405     0.8302 0.988 0.000 0.000 0.008 0.004 0.000
#&gt; GSM317695     1  0.3081     0.7363 0.776 0.000 0.000 0.004 0.220 0.000
#&gt; GSM317653     1  0.5527     0.6105 0.680 0.000 0.024 0.140 0.128 0.028
#&gt; GSM317656     1  0.0260     0.8315 0.992 0.000 0.000 0.000 0.008 0.000
#&gt; GSM317658     1  0.0837     0.8287 0.972 0.000 0.004 0.020 0.004 0.000
#&gt; GSM317660     5  0.2563     0.0000 0.028 0.000 0.008 0.000 0.880 0.084
#&gt; GSM317663     1  0.6128    -0.1781 0.428 0.000 0.124 0.420 0.024 0.004
#&gt; GSM317664     1  0.0000     0.8311 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317665     1  0.3409     0.6600 0.700 0.000 0.000 0.000 0.300 0.000
#&gt; GSM317673     1  0.0603     0.8310 0.980 0.000 0.000 0.004 0.016 0.000
#&gt; GSM317686     4  0.4055     0.4818 0.136 0.000 0.088 0.768 0.000 0.008
#&gt; GSM317688     1  0.0260     0.8315 0.992 0.000 0.000 0.008 0.000 0.000
#&gt; GSM317690     1  0.6069     0.4522 0.588 0.000 0.152 0.220 0.024 0.016
#&gt; GSM317654     1  0.3464     0.6480 0.688 0.000 0.000 0.000 0.312 0.000
#&gt; GSM317655     1  0.5696     0.6026 0.668 0.000 0.120 0.152 0.032 0.028
#&gt; GSM317659     1  0.1699     0.8212 0.936 0.000 0.016 0.032 0.016 0.000
#&gt; GSM317661     3  0.1297     0.0000 0.000 0.000 0.948 0.000 0.012 0.040
#&gt; GSM317662     6  0.3720     0.0000 0.000 0.000 0.236 0.028 0.000 0.736
#&gt; GSM317668     1  0.4828     0.6749 0.732 0.000 0.116 0.120 0.016 0.016
#&gt; GSM317669     1  0.3446     0.6512 0.692 0.000 0.000 0.000 0.308 0.000
#&gt; GSM317671     1  0.3081     0.7363 0.776 0.000 0.000 0.004 0.220 0.000
#&gt; GSM317676     1  0.3737     0.7552 0.816 0.000 0.088 0.072 0.020 0.004
#&gt; GSM317680     1  0.3081     0.7363 0.776 0.000 0.000 0.004 0.220 0.000
#&gt; GSM317684     1  0.0405     0.8302 0.988 0.000 0.000 0.008 0.004 0.000
#&gt; GSM317685     1  0.0000     0.8311 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317694     1  0.0000     0.8311 1.000 0.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-5-a').click(function(){
  $('#tab-CV-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-hclust-signature_compare](figure_cola/CV-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-hclust-collect-classes](figure_cola/CV-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) k
#> CV:hclust 49               NA 2
#> CV:hclust 47            0.572 3
#> CV:hclust 40            0.224 4
#> CV:hclust 42            0.196 5
#> CV:hclust 39               NA 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:kmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "kmeans"]
# you can also extract it by
# res = res_list["CV:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-kmeans-collect-plots](figure_cola/CV-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-kmeans-select-partition-number](figure_cola/CV-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.742           0.893       0.948         0.4724 0.542   0.542
#> 3 3 0.557           0.664       0.811         0.2159 0.807   0.660
#> 4 4 0.668           0.761       0.864         0.1307 0.865   0.681
#> 5 5 0.673           0.645       0.788         0.0669 0.962   0.884
#> 6 6 0.693           0.533       0.713         0.0572 0.896   0.695
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-classes'>
<ul>
<li><a href='#tab-CV-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-kmeans-get-classes-1'>
<p><a id='tab-CV-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     1  0.6148      0.811 0.848 0.152
#&gt; GSM317652     1  0.0000      0.930 1.000 0.000
#&gt; GSM317666     2  0.1184      0.964 0.016 0.984
#&gt; GSM317672     2  0.0376      0.968 0.004 0.996
#&gt; GSM317679     1  0.8144      0.695 0.748 0.252
#&gt; GSM317681     2  0.0376      0.968 0.004 0.996
#&gt; GSM317682     1  0.0000      0.930 1.000 0.000
#&gt; GSM317683     2  0.0672      0.968 0.008 0.992
#&gt; GSM317689     2  0.0376      0.968 0.004 0.996
#&gt; GSM317691     1  0.0000      0.930 1.000 0.000
#&gt; GSM317692     1  0.4562      0.870 0.904 0.096
#&gt; GSM317693     1  0.0000      0.930 1.000 0.000
#&gt; GSM317696     1  0.0000      0.930 1.000 0.000
#&gt; GSM317697     1  0.0000      0.930 1.000 0.000
#&gt; GSM317698     1  0.0000      0.930 1.000 0.000
#&gt; GSM317650     2  0.0000      0.967 0.000 1.000
#&gt; GSM317651     1  0.0000      0.930 1.000 0.000
#&gt; GSM317657     1  0.8499      0.650 0.724 0.276
#&gt; GSM317667     2  0.0672      0.968 0.008 0.992
#&gt; GSM317670     1  0.4562      0.862 0.904 0.096
#&gt; GSM317674     1  0.0000      0.930 1.000 0.000
#&gt; GSM317675     1  0.0000      0.930 1.000 0.000
#&gt; GSM317677     1  0.0000      0.930 1.000 0.000
#&gt; GSM317678     2  0.0376      0.968 0.004 0.996
#&gt; GSM317687     1  0.0000      0.930 1.000 0.000
#&gt; GSM317695     1  0.0000      0.930 1.000 0.000
#&gt; GSM317653     2  0.1184      0.964 0.016 0.984
#&gt; GSM317656     1  0.0000      0.930 1.000 0.000
#&gt; GSM317658     1  0.0000      0.930 1.000 0.000
#&gt; GSM317660     2  0.0000      0.967 0.000 1.000
#&gt; GSM317663     2  0.0672      0.968 0.008 0.992
#&gt; GSM317664     1  0.0000      0.930 1.000 0.000
#&gt; GSM317665     2  0.7376      0.734 0.208 0.792
#&gt; GSM317673     1  0.0000      0.930 1.000 0.000
#&gt; GSM317686     2  0.0938      0.966 0.012 0.988
#&gt; GSM317688     1  0.0000      0.930 1.000 0.000
#&gt; GSM317690     2  0.0672      0.968 0.008 0.992
#&gt; GSM317654     2  0.7056      0.759 0.192 0.808
#&gt; GSM317655     1  0.9087      0.573 0.676 0.324
#&gt; GSM317659     1  0.0000      0.930 1.000 0.000
#&gt; GSM317661     2  0.0000      0.967 0.000 1.000
#&gt; GSM317662     2  0.0000      0.967 0.000 1.000
#&gt; GSM317668     1  0.0000      0.930 1.000 0.000
#&gt; GSM317669     1  0.9393      0.501 0.644 0.356
#&gt; GSM317671     1  0.8081      0.701 0.752 0.248
#&gt; GSM317676     1  0.3114      0.895 0.944 0.056
#&gt; GSM317680     1  0.8081      0.701 0.752 0.248
#&gt; GSM317684     1  0.0000      0.930 1.000 0.000
#&gt; GSM317685     1  0.0000      0.930 1.000 0.000
#&gt; GSM317694     1  0.0000      0.930 1.000 0.000
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-1-a').click(function(){
  $('#tab-CV-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-2'>
<p><a id='tab-CV-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM317649     3  0.6309     0.3149 0.500 0.000 0.500
#&gt; GSM317652     1  0.1163     0.8883 0.972 0.000 0.028
#&gt; GSM317666     2  0.6919     0.6056 0.016 0.536 0.448
#&gt; GSM317672     3  0.3682     0.0084 0.008 0.116 0.876
#&gt; GSM317679     3  0.6244     0.4668 0.440 0.000 0.560
#&gt; GSM317681     3  0.0983     0.2129 0.004 0.016 0.980
#&gt; GSM317682     1  0.0475     0.9040 0.992 0.004 0.004
#&gt; GSM317683     2  0.6095     0.7406 0.000 0.608 0.392
#&gt; GSM317689     2  0.6308     0.7138 0.000 0.508 0.492
#&gt; GSM317691     1  0.0237     0.9045 0.996 0.004 0.000
#&gt; GSM317692     1  0.4960     0.7097 0.832 0.040 0.128
#&gt; GSM317693     1  0.0237     0.9045 0.996 0.004 0.000
#&gt; GSM317696     1  0.0475     0.9040 0.992 0.004 0.004
#&gt; GSM317697     1  0.0237     0.9045 0.996 0.004 0.000
#&gt; GSM317698     1  0.0000     0.9044 1.000 0.000 0.000
#&gt; GSM317650     2  0.4504     0.6245 0.000 0.804 0.196
#&gt; GSM317651     1  0.1411     0.8819 0.964 0.000 0.036
#&gt; GSM317657     1  0.8957     0.1095 0.492 0.376 0.132
#&gt; GSM317667     2  0.6026     0.6995 0.000 0.624 0.376
#&gt; GSM317670     1  0.7507     0.4413 0.644 0.288 0.068
#&gt; GSM317674     1  0.0237     0.9041 0.996 0.000 0.004
#&gt; GSM317675     1  0.0000     0.9044 1.000 0.000 0.000
#&gt; GSM317677     1  0.0000     0.9044 1.000 0.000 0.000
#&gt; GSM317678     2  0.6302     0.7099 0.000 0.520 0.480
#&gt; GSM317687     1  0.0237     0.9045 0.996 0.004 0.000
#&gt; GSM317695     1  0.0592     0.9005 0.988 0.000 0.012
#&gt; GSM317653     3  0.6633    -0.5627 0.008 0.444 0.548
#&gt; GSM317656     1  0.0592     0.9004 0.988 0.000 0.012
#&gt; GSM317658     1  0.0237     0.9045 0.996 0.004 0.000
#&gt; GSM317660     3  0.3551     0.1578 0.000 0.132 0.868
#&gt; GSM317663     2  0.6771     0.6558 0.012 0.548 0.440
#&gt; GSM317664     1  0.0237     0.9041 0.996 0.000 0.004
#&gt; GSM317665     3  0.3784     0.4533 0.132 0.004 0.864
#&gt; GSM317673     1  0.0237     0.9041 0.996 0.000 0.004
#&gt; GSM317686     2  0.6008     0.7082 0.004 0.664 0.332
#&gt; GSM317688     1  0.0237     0.9041 0.996 0.000 0.004
#&gt; GSM317690     2  0.6339     0.7241 0.008 0.632 0.360
#&gt; GSM317654     3  0.3715     0.4492 0.128 0.004 0.868
#&gt; GSM317655     1  0.9147     0.1193 0.496 0.348 0.156
#&gt; GSM317659     1  0.0237     0.9030 0.996 0.000 0.004
#&gt; GSM317661     2  0.5363     0.6893 0.000 0.724 0.276
#&gt; GSM317662     2  0.5327     0.6907 0.000 0.728 0.272
#&gt; GSM317668     1  0.1919     0.8750 0.956 0.020 0.024
#&gt; GSM317669     3  0.5497     0.5364 0.292 0.000 0.708
#&gt; GSM317671     3  0.6267     0.4464 0.452 0.000 0.548
#&gt; GSM317676     1  0.6829     0.5934 0.736 0.168 0.096
#&gt; GSM317680     3  0.6267     0.4464 0.452 0.000 0.548
#&gt; GSM317684     1  0.0237     0.9045 0.996 0.004 0.000
#&gt; GSM317685     1  0.0475     0.9040 0.992 0.004 0.004
#&gt; GSM317694     1  0.0237     0.9041 0.996 0.000 0.004
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-2-a').click(function(){
  $('#tab-CV-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-3'>
<p><a id='tab-CV-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     3  0.4374      0.723 0.228 0.008 0.760 0.004
#&gt; GSM317652     1  0.1863      0.929 0.944 0.004 0.040 0.012
#&gt; GSM317666     4  0.1610      0.447 0.000 0.016 0.032 0.952
#&gt; GSM317672     3  0.4630      0.555 0.000 0.036 0.768 0.196
#&gt; GSM317679     3  0.4569      0.727 0.220 0.008 0.760 0.012
#&gt; GSM317681     3  0.1398      0.740 0.000 0.004 0.956 0.040
#&gt; GSM317682     1  0.0000      0.970 1.000 0.000 0.000 0.000
#&gt; GSM317683     2  0.5793      0.675 0.000 0.628 0.048 0.324
#&gt; GSM317689     2  0.7371      0.581 0.000 0.472 0.168 0.360
#&gt; GSM317691     1  0.0336      0.970 0.992 0.000 0.000 0.008
#&gt; GSM317692     1  0.4695      0.713 0.800 0.024 0.028 0.148
#&gt; GSM317693     1  0.0336      0.970 0.992 0.000 0.000 0.008
#&gt; GSM317696     1  0.0000      0.970 1.000 0.000 0.000 0.000
#&gt; GSM317697     1  0.0336      0.970 0.992 0.000 0.000 0.008
#&gt; GSM317698     1  0.0376      0.970 0.992 0.004 0.000 0.004
#&gt; GSM317650     2  0.2399      0.679 0.000 0.920 0.032 0.048
#&gt; GSM317651     1  0.1771      0.934 0.948 0.004 0.036 0.012
#&gt; GSM317657     4  0.6611      0.473 0.336 0.068 0.012 0.584
#&gt; GSM317667     4  0.2032      0.429 0.000 0.036 0.028 0.936
#&gt; GSM317670     4  0.7671      0.449 0.360 0.132 0.020 0.488
#&gt; GSM317674     1  0.0188      0.970 0.996 0.004 0.000 0.000
#&gt; GSM317675     1  0.0376      0.970 0.992 0.004 0.000 0.004
#&gt; GSM317677     1  0.0524      0.969 0.988 0.004 0.000 0.008
#&gt; GSM317678     2  0.7382      0.659 0.000 0.520 0.260 0.220
#&gt; GSM317687     1  0.0336      0.970 0.992 0.000 0.000 0.008
#&gt; GSM317695     1  0.1452      0.934 0.956 0.008 0.036 0.000
#&gt; GSM317653     4  0.4452      0.239 0.000 0.008 0.260 0.732
#&gt; GSM317656     1  0.0188      0.970 0.996 0.004 0.000 0.000
#&gt; GSM317658     1  0.0524      0.968 0.988 0.000 0.004 0.008
#&gt; GSM317660     3  0.3398      0.686 0.000 0.068 0.872 0.060
#&gt; GSM317663     4  0.4082      0.407 0.004 0.052 0.108 0.836
#&gt; GSM317664     1  0.0188      0.970 0.996 0.004 0.000 0.000
#&gt; GSM317665     3  0.1888      0.755 0.016 0.000 0.940 0.044
#&gt; GSM317673     1  0.0000      0.970 1.000 0.000 0.000 0.000
#&gt; GSM317686     4  0.2089      0.432 0.000 0.048 0.020 0.932
#&gt; GSM317688     1  0.0188      0.970 0.996 0.004 0.000 0.000
#&gt; GSM317690     4  0.5749      0.279 0.012 0.188 0.076 0.724
#&gt; GSM317654     3  0.1854      0.752 0.012 0.000 0.940 0.048
#&gt; GSM317655     4  0.7569      0.468 0.324 0.132 0.020 0.524
#&gt; GSM317659     1  0.0779      0.966 0.980 0.004 0.000 0.016
#&gt; GSM317661     2  0.4966      0.768 0.000 0.768 0.076 0.156
#&gt; GSM317662     2  0.4989      0.767 0.000 0.764 0.072 0.164
#&gt; GSM317668     1  0.3699      0.838 0.864 0.048 0.008 0.080
#&gt; GSM317669     3  0.1807      0.762 0.052 0.008 0.940 0.000
#&gt; GSM317671     3  0.4604      0.727 0.224 0.008 0.756 0.012
#&gt; GSM317676     4  0.6667      0.352 0.436 0.056 0.012 0.496
#&gt; GSM317680     3  0.4604      0.727 0.224 0.008 0.756 0.012
#&gt; GSM317684     1  0.0336      0.970 0.992 0.000 0.000 0.008
#&gt; GSM317685     1  0.0000      0.970 1.000 0.000 0.000 0.000
#&gt; GSM317694     1  0.0000      0.970 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-3-a').click(function(){
  $('#tab-CV-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-4'>
<p><a id='tab-CV-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     3  0.5547     0.6888 0.148 0.000 0.644 0.000 0.208
#&gt; GSM317652     1  0.2353     0.8241 0.908 0.000 0.060 0.004 0.028
#&gt; GSM317666     4  0.1854     0.5690 0.000 0.008 0.036 0.936 0.020
#&gt; GSM317672     3  0.6072     0.4060 0.000 0.100 0.680 0.120 0.100
#&gt; GSM317679     3  0.5581     0.6947 0.140 0.000 0.636 0.000 0.224
#&gt; GSM317681     3  0.2149     0.6692 0.000 0.036 0.924 0.012 0.028
#&gt; GSM317682     1  0.0566     0.8799 0.984 0.000 0.000 0.004 0.012
#&gt; GSM317683     2  0.5490     0.6547 0.000 0.688 0.016 0.176 0.120
#&gt; GSM317689     2  0.7375     0.5083 0.000 0.520 0.084 0.176 0.220
#&gt; GSM317691     1  0.0865     0.8778 0.972 0.000 0.000 0.004 0.024
#&gt; GSM317692     1  0.5897     0.3520 0.668 0.012 0.024 0.084 0.212
#&gt; GSM317693     1  0.0865     0.8778 0.972 0.000 0.000 0.004 0.024
#&gt; GSM317696     1  0.0000     0.8833 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317697     1  0.0865     0.8778 0.972 0.000 0.000 0.004 0.024
#&gt; GSM317698     1  0.0290     0.8828 0.992 0.000 0.000 0.000 0.008
#&gt; GSM317650     2  0.3730     0.5263 0.000 0.712 0.000 0.000 0.288
#&gt; GSM317651     1  0.2721     0.8161 0.896 0.000 0.052 0.016 0.036
#&gt; GSM317657     4  0.7542    -0.8374 0.252 0.016 0.016 0.380 0.336
#&gt; GSM317667     4  0.1891     0.5574 0.000 0.032 0.016 0.936 0.016
#&gt; GSM317670     5  0.7984     0.9769 0.240 0.044 0.020 0.296 0.400
#&gt; GSM317674     1  0.0290     0.8820 0.992 0.000 0.000 0.000 0.008
#&gt; GSM317675     1  0.0000     0.8833 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317677     1  0.0865     0.8778 0.972 0.000 0.000 0.004 0.024
#&gt; GSM317678     2  0.6673     0.6461 0.000 0.624 0.136 0.128 0.112
#&gt; GSM317687     1  0.1041     0.8769 0.964 0.000 0.000 0.004 0.032
#&gt; GSM317695     1  0.3710     0.6242 0.784 0.000 0.024 0.000 0.192
#&gt; GSM317653     4  0.5525     0.3495 0.000 0.040 0.296 0.632 0.032
#&gt; GSM317656     1  0.0290     0.8820 0.992 0.000 0.000 0.000 0.008
#&gt; GSM317658     1  0.1205     0.8678 0.956 0.000 0.000 0.004 0.040
#&gt; GSM317660     3  0.4700     0.5020 0.000 0.160 0.752 0.012 0.076
#&gt; GSM317663     4  0.5148     0.4203 0.000 0.032 0.064 0.724 0.180
#&gt; GSM317664     1  0.0290     0.8820 0.992 0.000 0.000 0.000 0.008
#&gt; GSM317665     3  0.1095     0.6827 0.000 0.012 0.968 0.012 0.008
#&gt; GSM317673     1  0.0290     0.8820 0.992 0.000 0.000 0.000 0.008
#&gt; GSM317686     4  0.0579     0.5650 0.000 0.008 0.000 0.984 0.008
#&gt; GSM317688     1  0.0000     0.8833 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317690     4  0.7190    -0.0708 0.004 0.172 0.028 0.420 0.376
#&gt; GSM317654     3  0.1200     0.6812 0.000 0.016 0.964 0.012 0.008
#&gt; GSM317655     5  0.7973     0.9766 0.232 0.044 0.020 0.304 0.400
#&gt; GSM317659     1  0.1525     0.8667 0.948 0.000 0.004 0.012 0.036
#&gt; GSM317661     2  0.2772     0.7050 0.000 0.896 0.032 0.044 0.028
#&gt; GSM317662     2  0.2378     0.7030 0.000 0.908 0.012 0.064 0.016
#&gt; GSM317668     1  0.5673     0.0930 0.608 0.000 0.020 0.060 0.312
#&gt; GSM317669     3  0.3779     0.6955 0.024 0.000 0.776 0.000 0.200
#&gt; GSM317671     3  0.5581     0.6947 0.140 0.000 0.636 0.000 0.224
#&gt; GSM317676     1  0.7218    -0.8079 0.344 0.000 0.016 0.304 0.336
#&gt; GSM317680     3  0.5581     0.6947 0.140 0.000 0.636 0.000 0.224
#&gt; GSM317684     1  0.1041     0.8769 0.964 0.000 0.000 0.004 0.032
#&gt; GSM317685     1  0.0771     0.8799 0.976 0.000 0.000 0.004 0.020
#&gt; GSM317694     1  0.0290     0.8820 0.992 0.000 0.000 0.000 0.008
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-4-a').click(function(){
  $('#tab-CV-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-5'>
<p><a id='tab-CV-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM317649     3  0.5345     0.1379 0.068 0.004 0.468 0.008 0.452 0.000
#&gt; GSM317652     1  0.3748     0.7913 0.824 0.056 0.012 0.024 0.084 0.000
#&gt; GSM317666     3  0.6768    -0.0449 0.000 0.312 0.380 0.268 0.040 0.000
#&gt; GSM317672     5  0.5639     0.2834 0.000 0.268 0.008 0.144 0.576 0.004
#&gt; GSM317679     3  0.5564     0.1573 0.052 0.008 0.468 0.024 0.448 0.000
#&gt; GSM317681     5  0.2250     0.5694 0.000 0.092 0.020 0.000 0.888 0.000
#&gt; GSM317682     1  0.0632     0.9120 0.976 0.024 0.000 0.000 0.000 0.000
#&gt; GSM317683     2  0.6605     0.5213 0.000 0.468 0.016 0.204 0.020 0.292
#&gt; GSM317689     2  0.6721     0.4839 0.000 0.444 0.000 0.312 0.060 0.184
#&gt; GSM317691     1  0.0551     0.9126 0.984 0.008 0.000 0.004 0.004 0.000
#&gt; GSM317692     1  0.6292     0.0810 0.528 0.180 0.012 0.260 0.020 0.000
#&gt; GSM317693     1  0.0291     0.9144 0.992 0.004 0.000 0.000 0.004 0.000
#&gt; GSM317696     1  0.0000     0.9143 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317697     1  0.0291     0.9144 0.992 0.004 0.000 0.000 0.004 0.000
#&gt; GSM317698     1  0.0000     0.9143 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317650     6  0.0146     0.2789 0.000 0.000 0.000 0.004 0.000 0.996
#&gt; GSM317651     1  0.4134     0.7666 0.804 0.056 0.012 0.056 0.072 0.000
#&gt; GSM317657     4  0.5427     0.5837 0.144 0.108 0.044 0.692 0.004 0.008
#&gt; GSM317667     3  0.5878     0.0742 0.000 0.308 0.492 0.196 0.004 0.000
#&gt; GSM317670     4  0.2896     0.6284 0.140 0.012 0.004 0.840 0.004 0.000
#&gt; GSM317674     1  0.0508     0.9137 0.984 0.004 0.012 0.000 0.000 0.000
#&gt; GSM317675     1  0.0000     0.9143 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317677     1  0.0146     0.9148 0.996 0.004 0.000 0.000 0.000 0.000
#&gt; GSM317678     2  0.7262     0.5071 0.000 0.484 0.028 0.144 0.096 0.248
#&gt; GSM317687     1  0.1155     0.9018 0.956 0.036 0.000 0.004 0.004 0.000
#&gt; GSM317695     1  0.4581     0.1890 0.524 0.004 0.448 0.004 0.020 0.000
#&gt; GSM317653     5  0.6966     0.1210 0.000 0.164 0.336 0.092 0.408 0.000
#&gt; GSM317656     1  0.1026     0.9087 0.968 0.004 0.012 0.008 0.008 0.000
#&gt; GSM317658     1  0.0692     0.9049 0.976 0.000 0.000 0.020 0.004 0.000
#&gt; GSM317660     5  0.4625     0.4660 0.000 0.256 0.036 0.020 0.684 0.004
#&gt; GSM317663     4  0.6822     0.0811 0.000 0.268 0.212 0.464 0.048 0.008
#&gt; GSM317664     1  0.0508     0.9137 0.984 0.004 0.012 0.000 0.000 0.000
#&gt; GSM317665     5  0.0405     0.5492 0.000 0.004 0.008 0.000 0.988 0.000
#&gt; GSM317673     1  0.0405     0.9144 0.988 0.004 0.008 0.000 0.000 0.000
#&gt; GSM317686     3  0.5878     0.0529 0.000 0.308 0.468 0.224 0.000 0.000
#&gt; GSM317688     1  0.0146     0.9147 0.996 0.004 0.000 0.000 0.000 0.000
#&gt; GSM317690     4  0.2865     0.3949 0.000 0.092 0.016 0.868 0.008 0.016
#&gt; GSM317654     5  0.1036     0.5623 0.000 0.024 0.008 0.004 0.964 0.000
#&gt; GSM317655     4  0.2989     0.6237 0.120 0.012 0.004 0.848 0.016 0.000
#&gt; GSM317659     1  0.2107     0.8787 0.920 0.024 0.012 0.036 0.008 0.000
#&gt; GSM317661     2  0.5663    -0.4797 0.000 0.472 0.024 0.028 0.032 0.444
#&gt; GSM317662     6  0.5663    -0.1563 0.000 0.444 0.032 0.028 0.024 0.472
#&gt; GSM317668     4  0.4069     0.4664 0.376 0.000 0.008 0.612 0.004 0.000
#&gt; GSM317669     5  0.4220    -0.2285 0.004 0.008 0.468 0.000 0.520 0.000
#&gt; GSM317671     3  0.5564     0.1573 0.052 0.008 0.468 0.024 0.448 0.000
#&gt; GSM317676     4  0.5009     0.5992 0.232 0.036 0.024 0.684 0.024 0.000
#&gt; GSM317680     3  0.5564     0.1573 0.052 0.008 0.468 0.024 0.448 0.000
#&gt; GSM317684     1  0.1003     0.9066 0.964 0.028 0.000 0.004 0.004 0.000
#&gt; GSM317685     1  0.1074     0.9094 0.960 0.028 0.012 0.000 0.000 0.000
#&gt; GSM317694     1  0.0508     0.9137 0.984 0.004 0.012 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-5-a').click(function(){
  $('#tab-CV-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-kmeans-signature_compare](figure_cola/CV-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-kmeans-collect-classes](figure_cola/CV-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) k
#> CV:kmeans 50            0.394 2
#> CV:kmeans 37            0.733 3
#> CV:kmeans 40            0.519 4
#> CV:kmeans 42            0.712 5
#> CV:kmeans 30            0.612 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:skmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "skmeans"]
# you can also extract it by
# res = res_list["CV:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-skmeans-collect-plots](figure_cola/CV-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-skmeans-select-partition-number](figure_cola/CV-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.674           0.845       0.931         0.5086 0.490   0.490
#> 3 3 0.364           0.603       0.785         0.3126 0.770   0.566
#> 4 4 0.371           0.435       0.666         0.1217 0.902   0.732
#> 5 5 0.414           0.303       0.587         0.0663 0.930   0.775
#> 6 6 0.492           0.332       0.565         0.0406 0.909   0.680
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-classes'>
<ul>
<li><a href='#tab-CV-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-skmeans-get-classes-1'>
<p><a id='tab-CV-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     2  0.9963      0.207 0.464 0.536
#&gt; GSM317652     1  0.5629      0.835 0.868 0.132
#&gt; GSM317666     2  0.0938      0.900 0.012 0.988
#&gt; GSM317672     2  0.0000      0.902 0.000 1.000
#&gt; GSM317679     2  0.5059      0.841 0.112 0.888
#&gt; GSM317681     2  0.0000      0.902 0.000 1.000
#&gt; GSM317682     1  0.2043      0.926 0.968 0.032
#&gt; GSM317683     2  0.0672      0.902 0.008 0.992
#&gt; GSM317689     2  0.1843      0.893 0.028 0.972
#&gt; GSM317691     1  0.0672      0.941 0.992 0.008
#&gt; GSM317692     2  0.9795      0.305 0.416 0.584
#&gt; GSM317693     1  0.0000      0.943 1.000 0.000
#&gt; GSM317696     1  0.0000      0.943 1.000 0.000
#&gt; GSM317697     1  0.0000      0.943 1.000 0.000
#&gt; GSM317698     1  0.0000      0.943 1.000 0.000
#&gt; GSM317650     2  0.0000      0.902 0.000 1.000
#&gt; GSM317651     1  0.9323      0.453 0.652 0.348
#&gt; GSM317657     2  0.9881      0.231 0.436 0.564
#&gt; GSM317667     2  0.0000      0.902 0.000 1.000
#&gt; GSM317670     1  0.8499      0.621 0.724 0.276
#&gt; GSM317674     1  0.0000      0.943 1.000 0.000
#&gt; GSM317675     1  0.0000      0.943 1.000 0.000
#&gt; GSM317677     1  0.0000      0.943 1.000 0.000
#&gt; GSM317678     2  0.0000      0.902 0.000 1.000
#&gt; GSM317687     1  0.4298      0.881 0.912 0.088
#&gt; GSM317695     1  0.0000      0.943 1.000 0.000
#&gt; GSM317653     2  0.0376      0.902 0.004 0.996
#&gt; GSM317656     1  0.1843      0.929 0.972 0.028
#&gt; GSM317658     1  0.1184      0.937 0.984 0.016
#&gt; GSM317660     2  0.0000      0.902 0.000 1.000
#&gt; GSM317663     2  0.0000      0.902 0.000 1.000
#&gt; GSM317664     1  0.0000      0.943 1.000 0.000
#&gt; GSM317665     2  0.2043      0.893 0.032 0.968
#&gt; GSM317673     1  0.0000      0.943 1.000 0.000
#&gt; GSM317686     2  0.0938      0.900 0.012 0.988
#&gt; GSM317688     1  0.0000      0.943 1.000 0.000
#&gt; GSM317690     2  0.0672      0.901 0.008 0.992
#&gt; GSM317654     2  0.0938      0.901 0.012 0.988
#&gt; GSM317655     2  0.7139      0.739 0.196 0.804
#&gt; GSM317659     1  0.1633      0.932 0.976 0.024
#&gt; GSM317661     2  0.0000      0.902 0.000 1.000
#&gt; GSM317662     2  0.0000      0.902 0.000 1.000
#&gt; GSM317668     1  0.0938      0.939 0.988 0.012
#&gt; GSM317669     2  0.4562      0.853 0.096 0.904
#&gt; GSM317671     2  0.6531      0.791 0.168 0.832
#&gt; GSM317676     1  0.8555      0.620 0.720 0.280
#&gt; GSM317680     2  0.7056      0.766 0.192 0.808
#&gt; GSM317684     1  0.0000      0.943 1.000 0.000
#&gt; GSM317685     1  0.0000      0.943 1.000 0.000
#&gt; GSM317694     1  0.0000      0.943 1.000 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-1-a').click(function(){
  $('#tab-CV-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-2'>
<p><a id='tab-CV-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM317649     3  0.3295    0.63951 0.096 0.008 0.896
#&gt; GSM317652     3  0.8734   -0.00949 0.424 0.108 0.468
#&gt; GSM317666     2  0.5414    0.59493 0.016 0.772 0.212
#&gt; GSM317672     2  0.6754    0.22856 0.012 0.556 0.432
#&gt; GSM317679     3  0.2063    0.66712 0.008 0.044 0.948
#&gt; GSM317681     3  0.5621    0.40201 0.000 0.308 0.692
#&gt; GSM317682     1  0.6537    0.69673 0.740 0.064 0.196
#&gt; GSM317683     2  0.4539    0.66748 0.016 0.836 0.148
#&gt; GSM317689     2  0.6737    0.57507 0.040 0.688 0.272
#&gt; GSM317691     1  0.4281    0.81817 0.872 0.072 0.056
#&gt; GSM317692     2  0.9528    0.25330 0.288 0.484 0.228
#&gt; GSM317693     1  0.2187    0.83154 0.948 0.028 0.024
#&gt; GSM317696     1  0.1163    0.83246 0.972 0.000 0.028
#&gt; GSM317697     1  0.1337    0.83107 0.972 0.016 0.012
#&gt; GSM317698     1  0.0237    0.82918 0.996 0.000 0.004
#&gt; GSM317650     2  0.4887    0.61955 0.000 0.772 0.228
#&gt; GSM317651     3  0.9874    0.17906 0.304 0.284 0.412
#&gt; GSM317657     2  0.6304    0.55278 0.192 0.752 0.056
#&gt; GSM317667     2  0.1529    0.66332 0.000 0.960 0.040
#&gt; GSM317670     2  0.8703    0.36341 0.332 0.544 0.124
#&gt; GSM317674     1  0.2165    0.83163 0.936 0.000 0.064
#&gt; GSM317675     1  0.0237    0.82990 0.996 0.000 0.004
#&gt; GSM317677     1  0.0661    0.83083 0.988 0.004 0.008
#&gt; GSM317678     2  0.6252    0.28330 0.000 0.556 0.444
#&gt; GSM317687     1  0.8334    0.54071 0.616 0.248 0.136
#&gt; GSM317695     1  0.6095    0.47926 0.608 0.000 0.392
#&gt; GSM317653     2  0.6627    0.40161 0.020 0.644 0.336
#&gt; GSM317656     1  0.7128    0.58578 0.664 0.052 0.284
#&gt; GSM317658     1  0.5891    0.73747 0.780 0.168 0.052
#&gt; GSM317660     3  0.6274    0.02017 0.000 0.456 0.544
#&gt; GSM317663     2  0.4452    0.64682 0.000 0.808 0.192
#&gt; GSM317664     1  0.2796    0.82421 0.908 0.000 0.092
#&gt; GSM317665     3  0.5360    0.57463 0.012 0.220 0.768
#&gt; GSM317673     1  0.5987    0.73406 0.756 0.036 0.208
#&gt; GSM317686     2  0.0237    0.65777 0.000 0.996 0.004
#&gt; GSM317688     1  0.4015    0.82525 0.876 0.028 0.096
#&gt; GSM317690     2  0.4196    0.67013 0.024 0.864 0.112
#&gt; GSM317654     3  0.6096    0.49827 0.016 0.280 0.704
#&gt; GSM317655     2  0.5780    0.63294 0.080 0.800 0.120
#&gt; GSM317659     1  0.7344    0.64998 0.696 0.204 0.100
#&gt; GSM317661     2  0.4399    0.64120 0.000 0.812 0.188
#&gt; GSM317662     2  0.4062    0.65455 0.000 0.836 0.164
#&gt; GSM317668     1  0.8125    0.58928 0.648 0.176 0.176
#&gt; GSM317669     3  0.1919    0.67324 0.024 0.020 0.956
#&gt; GSM317671     3  0.1905    0.67107 0.016 0.028 0.956
#&gt; GSM317676     2  0.8179    0.29697 0.352 0.564 0.084
#&gt; GSM317680     3  0.1585    0.67110 0.028 0.008 0.964
#&gt; GSM317684     1  0.2434    0.82969 0.940 0.036 0.024
#&gt; GSM317685     1  0.5473    0.78312 0.808 0.052 0.140
#&gt; GSM317694     1  0.2066    0.83344 0.940 0.000 0.060
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-2-a').click(function(){
  $('#tab-CV-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-3'>
<p><a id='tab-CV-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     3   0.268     0.6352 0.044 0.012 0.916 0.028
#&gt; GSM317652     4   0.952     0.1758 0.280 0.108 0.284 0.328
#&gt; GSM317666     2   0.787     0.3067 0.028 0.492 0.140 0.340
#&gt; GSM317672     2   0.786     0.3935 0.020 0.524 0.248 0.208
#&gt; GSM317679     3   0.159     0.6672 0.004 0.024 0.956 0.016
#&gt; GSM317681     3   0.677     0.1444 0.000 0.364 0.532 0.104
#&gt; GSM317682     1   0.731     0.5414 0.644 0.056 0.144 0.156
#&gt; GSM317683     2   0.531     0.5253 0.020 0.768 0.060 0.152
#&gt; GSM317689     2   0.709     0.4749 0.032 0.644 0.156 0.168
#&gt; GSM317691     1   0.680     0.5482 0.656 0.036 0.088 0.220
#&gt; GSM317692     2   0.962    -0.0482 0.164 0.360 0.180 0.296
#&gt; GSM317693     1   0.334     0.6733 0.856 0.016 0.000 0.128
#&gt; GSM317696     1   0.300     0.6871 0.892 0.000 0.060 0.048
#&gt; GSM317697     1   0.289     0.6778 0.896 0.020 0.004 0.080
#&gt; GSM317698     1   0.155     0.6780 0.952 0.000 0.008 0.040
#&gt; GSM317650     2   0.568     0.5205 0.000 0.720 0.140 0.140
#&gt; GSM317651     4   0.883     0.2719 0.128 0.148 0.220 0.504
#&gt; GSM317657     2   0.768    -0.0448 0.148 0.432 0.012 0.408
#&gt; GSM317667     2   0.573     0.4567 0.004 0.648 0.040 0.308
#&gt; GSM317670     4   0.876     0.3376 0.268 0.248 0.052 0.432
#&gt; GSM317674     1   0.423     0.6773 0.824 0.000 0.084 0.092
#&gt; GSM317675     1   0.166     0.6783 0.944 0.000 0.004 0.052
#&gt; GSM317677     1   0.307     0.6729 0.848 0.000 0.000 0.152
#&gt; GSM317678     2   0.659     0.4735 0.004 0.608 0.288 0.100
#&gt; GSM317687     1   0.839     0.0790 0.452 0.168 0.044 0.336
#&gt; GSM317695     3   0.599    -0.0210 0.392 0.004 0.568 0.036
#&gt; GSM317653     2   0.785     0.3603 0.024 0.520 0.168 0.288
#&gt; GSM317656     1   0.855     0.2409 0.484 0.068 0.288 0.160
#&gt; GSM317658     1   0.715     0.4999 0.656 0.116 0.056 0.172
#&gt; GSM317660     2   0.747     0.2822 0.000 0.488 0.312 0.200
#&gt; GSM317663     2   0.753     0.4333 0.016 0.564 0.204 0.216
#&gt; GSM317664     1   0.551     0.6130 0.720 0.000 0.196 0.084
#&gt; GSM317665     3   0.769     0.3496 0.012 0.220 0.528 0.240
#&gt; GSM317673     1   0.648     0.6161 0.692 0.024 0.136 0.148
#&gt; GSM317686     2   0.499     0.4430 0.012 0.720 0.012 0.256
#&gt; GSM317688     1   0.633     0.5890 0.680 0.008 0.136 0.176
#&gt; GSM317690     2   0.589     0.4173 0.016 0.692 0.052 0.240
#&gt; GSM317654     3   0.795     0.2338 0.024 0.284 0.508 0.184
#&gt; GSM317655     4   0.689    -0.0370 0.032 0.408 0.044 0.516
#&gt; GSM317659     1   0.729     0.2444 0.500 0.060 0.040 0.400
#&gt; GSM317661     2   0.441     0.5452 0.000 0.812 0.108 0.080
#&gt; GSM317662     2   0.360     0.5451 0.000 0.860 0.084 0.056
#&gt; GSM317668     1   0.816     0.0591 0.444 0.076 0.084 0.396
#&gt; GSM317669     3   0.202     0.6643 0.004 0.028 0.940 0.028
#&gt; GSM317671     3   0.217     0.6629 0.008 0.032 0.936 0.024
#&gt; GSM317676     4   0.748     0.3456 0.176 0.180 0.036 0.608
#&gt; GSM317680     3   0.139     0.6656 0.008 0.016 0.964 0.012
#&gt; GSM317684     1   0.448     0.6358 0.760 0.008 0.008 0.224
#&gt; GSM317685     1   0.716     0.5055 0.616 0.020 0.156 0.208
#&gt; GSM317694     1   0.451     0.6738 0.812 0.004 0.112 0.072
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-3-a').click(function(){
  $('#tab-CV-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-4'>
<p><a id='tab-CV-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     3   0.332    0.67528 0.040 0.004 0.868 0.020 0.068
#&gt; GSM317652     4   0.921    0.00236 0.228 0.036 0.196 0.276 0.264
#&gt; GSM317666     2   0.846    0.20712 0.012 0.328 0.100 0.272 0.288
#&gt; GSM317672     2   0.822    0.35461 0.020 0.448 0.224 0.092 0.216
#&gt; GSM317679     3   0.288    0.69177 0.008 0.040 0.896 0.020 0.036
#&gt; GSM317681     3   0.759   -0.01316 0.004 0.348 0.424 0.064 0.160
#&gt; GSM317682     1   0.757    0.24077 0.520 0.024 0.112 0.072 0.272
#&gt; GSM317683     2   0.644    0.38152 0.028 0.648 0.032 0.096 0.196
#&gt; GSM317689     2   0.651    0.40248 0.008 0.656 0.092 0.124 0.120
#&gt; GSM317691     1   0.826   -0.08817 0.424 0.040 0.068 0.156 0.312
#&gt; GSM317692     2   0.910    0.09353 0.104 0.336 0.084 0.156 0.320
#&gt; GSM317693     1   0.520    0.38313 0.712 0.004 0.008 0.096 0.180
#&gt; GSM317696     1   0.364    0.52467 0.848 0.000 0.052 0.032 0.068
#&gt; GSM317697     1   0.474    0.41785 0.744 0.000 0.008 0.084 0.164
#&gt; GSM317698     1   0.207    0.51470 0.920 0.000 0.000 0.032 0.048
#&gt; GSM317650     2   0.456    0.44054 0.000 0.792 0.048 0.080 0.080
#&gt; GSM317651     4   0.933    0.09396 0.128 0.088 0.176 0.344 0.264
#&gt; GSM317657     4   0.829   -0.00408 0.088 0.200 0.024 0.436 0.252
#&gt; GSM317667     2   0.699    0.29436 0.000 0.448 0.016 0.312 0.224
#&gt; GSM317670     4   0.726    0.30686 0.148 0.220 0.028 0.564 0.040
#&gt; GSM317674     1   0.439    0.51671 0.812 0.008 0.056 0.040 0.084
#&gt; GSM317675     1   0.234    0.52435 0.916 0.000 0.016 0.032 0.036
#&gt; GSM317677     1   0.424    0.47749 0.788 0.000 0.008 0.132 0.072
#&gt; GSM317678     2   0.665    0.41815 0.016 0.628 0.204 0.060 0.092
#&gt; GSM317687     5   0.820    0.00000 0.292 0.064 0.020 0.232 0.392
#&gt; GSM317695     3   0.544    0.34066 0.280 0.000 0.644 0.016 0.060
#&gt; GSM317653     2   0.819    0.29737 0.012 0.396 0.096 0.180 0.316
#&gt; GSM317656     1   0.880    0.17247 0.436 0.060 0.196 0.116 0.192
#&gt; GSM317658     1   0.825    0.16976 0.520 0.108 0.060 0.172 0.140
#&gt; GSM317660     2   0.789    0.30743 0.004 0.468 0.248 0.116 0.164
#&gt; GSM317663     2   0.816    0.27812 0.004 0.408 0.124 0.284 0.180
#&gt; GSM317664     1   0.610    0.44146 0.664 0.004 0.184 0.048 0.100
#&gt; GSM317665     3   0.770    0.23670 0.008 0.208 0.488 0.072 0.224
#&gt; GSM317673     1   0.713    0.39211 0.616 0.032 0.128 0.072 0.152
#&gt; GSM317686     2   0.654    0.25128 0.000 0.464 0.004 0.356 0.176
#&gt; GSM317688     1   0.748    0.28130 0.540 0.004 0.152 0.112 0.192
#&gt; GSM317690     2   0.669    0.20311 0.008 0.520 0.052 0.356 0.064
#&gt; GSM317654     2   0.843    0.00766 0.024 0.344 0.324 0.076 0.232
#&gt; GSM317655     4   0.491    0.08843 0.004 0.304 0.020 0.660 0.012
#&gt; GSM317659     1   0.769   -0.11880 0.428 0.044 0.024 0.360 0.144
#&gt; GSM317661     2   0.472    0.46131 0.000 0.780 0.064 0.104 0.052
#&gt; GSM317662     2   0.320    0.46813 0.000 0.872 0.032 0.064 0.032
#&gt; GSM317668     4   0.734    0.08196 0.336 0.028 0.052 0.496 0.088
#&gt; GSM317669     3   0.329    0.68378 0.020 0.040 0.876 0.012 0.052
#&gt; GSM317671     3   0.230    0.70164 0.012 0.028 0.924 0.020 0.016
#&gt; GSM317676     4   0.746    0.14193 0.124 0.132 0.024 0.584 0.136
#&gt; GSM317680     3   0.213    0.70372 0.016 0.016 0.928 0.004 0.036
#&gt; GSM317684     1   0.621   -0.00464 0.528 0.000 0.004 0.140 0.328
#&gt; GSM317685     1   0.721    0.21057 0.540 0.032 0.056 0.076 0.296
#&gt; GSM317694     1   0.517    0.49660 0.740 0.000 0.104 0.036 0.120
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-4-a').click(function(){
  $('#tab-CV-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-5'>
<p><a id='tab-CV-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM317649     3   0.324    0.66102 0.008 0.016 0.848 0.004 0.104 0.020
#&gt; GSM317652     5   0.949    0.35440 0.212 0.076 0.160 0.140 0.300 0.112
#&gt; GSM317666     4   0.669    0.34621 0.016 0.172 0.080 0.616 0.052 0.064
#&gt; GSM317672     2   0.817    0.25240 0.008 0.452 0.132 0.188 0.100 0.120
#&gt; GSM317679     3   0.337    0.68321 0.004 0.052 0.860 0.036 0.024 0.024
#&gt; GSM317681     2   0.774    0.18553 0.004 0.392 0.332 0.104 0.124 0.044
#&gt; GSM317682     1   0.799    0.26055 0.424 0.064 0.136 0.056 0.292 0.028
#&gt; GSM317683     2   0.699    0.19291 0.032 0.564 0.024 0.224 0.064 0.092
#&gt; GSM317689     2   0.645    0.35205 0.008 0.644 0.060 0.104 0.064 0.120
#&gt; GSM317691     1   0.798    0.32689 0.464 0.016 0.052 0.116 0.212 0.140
#&gt; GSM317692     2   0.948   -0.01390 0.088 0.276 0.108 0.144 0.260 0.124
#&gt; GSM317693     1   0.539    0.51501 0.676 0.000 0.000 0.076 0.164 0.084
#&gt; GSM317696     1   0.392    0.55114 0.808 0.000 0.048 0.016 0.108 0.020
#&gt; GSM317697     1   0.489    0.52544 0.724 0.000 0.004 0.044 0.152 0.076
#&gt; GSM317698     1   0.316    0.55216 0.856 0.004 0.004 0.008 0.056 0.072
#&gt; GSM317650     2   0.502    0.37463 0.000 0.744 0.028 0.076 0.052 0.100
#&gt; GSM317651     5   0.931    0.26879 0.080 0.084 0.128 0.152 0.304 0.252
#&gt; GSM317657     4   0.830    0.00742 0.072 0.140 0.016 0.384 0.100 0.288
#&gt; GSM317667     4   0.516    0.34698 0.000 0.268 0.016 0.644 0.012 0.060
#&gt; GSM317670     6   0.546    0.46010 0.076 0.112 0.016 0.044 0.028 0.724
#&gt; GSM317674     1   0.542    0.48470 0.696 0.000 0.084 0.020 0.152 0.048
#&gt; GSM317675     1   0.279    0.54568 0.876 0.000 0.008 0.004 0.060 0.052
#&gt; GSM317677     1   0.536    0.50496 0.684 0.004 0.008 0.024 0.136 0.144
#&gt; GSM317678     2   0.673    0.34366 0.012 0.612 0.148 0.120 0.048 0.060
#&gt; GSM317687     4   0.884   -0.08146 0.252 0.052 0.056 0.308 0.244 0.088
#&gt; GSM317695     3   0.498    0.41465 0.216 0.008 0.692 0.008 0.064 0.012
#&gt; GSM317653     4   0.739   -0.08725 0.004 0.384 0.052 0.388 0.112 0.060
#&gt; GSM317656     1   0.884   -0.09570 0.340 0.044 0.188 0.080 0.260 0.088
#&gt; GSM317658     1   0.851    0.27734 0.464 0.112 0.056 0.084 0.128 0.156
#&gt; GSM317660     2   0.755    0.29968 0.000 0.508 0.136 0.152 0.140 0.064
#&gt; GSM317663     4   0.696    0.30631 0.008 0.184 0.128 0.572 0.056 0.052
#&gt; GSM317664     1   0.555    0.42140 0.656 0.000 0.176 0.008 0.128 0.032
#&gt; GSM317665     3   0.829   -0.01462 0.008 0.240 0.392 0.136 0.164 0.060
#&gt; GSM317673     1   0.807    0.32846 0.472 0.040 0.120 0.060 0.228 0.080
#&gt; GSM317686     4   0.588    0.34594 0.004 0.264 0.004 0.584 0.024 0.120
#&gt; GSM317688     1   0.801    0.36877 0.496 0.028 0.132 0.064 0.168 0.112
#&gt; GSM317690     6   0.701    0.05345 0.012 0.336 0.024 0.236 0.008 0.384
#&gt; GSM317654     2   0.885    0.06498 0.020 0.312 0.244 0.188 0.164 0.072
#&gt; GSM317655     6   0.458    0.46439 0.004 0.128 0.012 0.076 0.020 0.760
#&gt; GSM317659     1   0.839   -0.06537 0.316 0.020 0.040 0.120 0.188 0.316
#&gt; GSM317661     2   0.458    0.34885 0.000 0.764 0.028 0.132 0.036 0.040
#&gt; GSM317662     2   0.451    0.34868 0.000 0.768 0.020 0.120 0.024 0.068
#&gt; GSM317668     6   0.668    0.22081 0.200 0.028 0.048 0.032 0.088 0.604
#&gt; GSM317669     3   0.302    0.67098 0.000 0.032 0.872 0.024 0.060 0.012
#&gt; GSM317671     3   0.167    0.70896 0.004 0.012 0.944 0.012 0.008 0.020
#&gt; GSM317676     6   0.718    0.24245 0.104 0.032 0.004 0.216 0.116 0.528
#&gt; GSM317680     3   0.191    0.70689 0.004 0.004 0.928 0.020 0.040 0.004
#&gt; GSM317684     1   0.620    0.43053 0.548 0.000 0.008 0.100 0.292 0.052
#&gt; GSM317685     1   0.714    0.31806 0.480 0.020 0.048 0.052 0.332 0.068
#&gt; GSM317694     1   0.607    0.49884 0.644 0.004 0.104 0.024 0.172 0.052
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-5-a').click(function(){
  $('#tab-CV-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-skmeans-signature_compare](figure_cola/CV-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-skmeans-collect-classes](figure_cola/CV-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> CV:skmeans 46           0.7626 2
#> CV:skmeans 38           0.8377 3
#> CV:skmeans 24           0.5060 4
#> CV:skmeans  9           0.0842 5
#> CV:skmeans 11           0.0601 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:pam






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "pam"]
# you can also extract it by
# res = res_list["CV:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   There is no best k.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-pam-collect-plots](figure_cola/CV-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-pam-select-partition-number](figure_cola/CV-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.630           0.902       0.946          0.116 0.960   0.960
#> 3 3 0.512           0.799       0.913          0.480 0.961   0.959
#> 4 4 0.439           0.797       0.904          0.208 0.962   0.958
#> 5 5 0.488           0.840       0.922          0.174 0.962   0.957
#> 6 6 0.433           0.782       0.907          0.187 0.963   0.957
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] NA
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-classes'>
<ul>
<li><a href='#tab-CV-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-pam-get-classes-1'>
<p><a id='tab-CV-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     1  0.0000      0.947 1.000 0.000
#&gt; GSM317652     1  0.0000      0.947 1.000 0.000
#&gt; GSM317666     1  0.2603      0.936 0.956 0.044
#&gt; GSM317672     1  0.0000      0.947 1.000 0.000
#&gt; GSM317679     1  0.0000      0.947 1.000 0.000
#&gt; GSM317681     1  0.0000      0.947 1.000 0.000
#&gt; GSM317682     1  0.0000      0.947 1.000 0.000
#&gt; GSM317683     1  0.7139      0.826 0.804 0.196
#&gt; GSM317689     1  0.5946      0.874 0.856 0.144
#&gt; GSM317691     1  0.2043      0.943 0.968 0.032
#&gt; GSM317692     1  0.0000      0.947 1.000 0.000
#&gt; GSM317693     1  0.2603      0.939 0.956 0.044
#&gt; GSM317696     1  0.0672      0.946 0.992 0.008
#&gt; GSM317697     1  0.2603      0.939 0.956 0.044
#&gt; GSM317698     1  0.1633      0.944 0.976 0.024
#&gt; GSM317650     2  0.1414      0.000 0.020 0.980
#&gt; GSM317651     1  0.0000      0.947 1.000 0.000
#&gt; GSM317657     1  0.7528      0.817 0.784 0.216
#&gt; GSM317667     1  0.7299      0.820 0.796 0.204
#&gt; GSM317670     1  0.2236      0.941 0.964 0.036
#&gt; GSM317674     1  0.1184      0.944 0.984 0.016
#&gt; GSM317675     1  0.1184      0.944 0.984 0.016
#&gt; GSM317677     1  0.1184      0.944 0.984 0.016
#&gt; GSM317678     1  0.6247      0.865 0.844 0.156
#&gt; GSM317687     1  0.2236      0.942 0.964 0.036
#&gt; GSM317695     1  0.0000      0.947 1.000 0.000
#&gt; GSM317653     1  0.1633      0.943 0.976 0.024
#&gt; GSM317656     1  0.0000      0.947 1.000 0.000
#&gt; GSM317658     1  0.2043      0.941 0.968 0.032
#&gt; GSM317660     1  0.0000      0.947 1.000 0.000
#&gt; GSM317663     1  0.4815      0.896 0.896 0.104
#&gt; GSM317664     1  0.0672      0.946 0.992 0.008
#&gt; GSM317665     1  0.0000      0.947 1.000 0.000
#&gt; GSM317673     1  0.0938      0.947 0.988 0.012
#&gt; GSM317686     1  0.7528      0.817 0.784 0.216
#&gt; GSM317688     1  0.1184      0.944 0.984 0.016
#&gt; GSM317690     1  0.6048      0.871 0.852 0.148
#&gt; GSM317654     1  0.0000      0.947 1.000 0.000
#&gt; GSM317655     1  0.5842      0.884 0.860 0.140
#&gt; GSM317659     1  0.1184      0.944 0.984 0.016
#&gt; GSM317661     1  0.6887      0.834 0.816 0.184
#&gt; GSM317662     1  0.7219      0.822 0.800 0.200
#&gt; GSM317668     1  0.1414      0.945 0.980 0.020
#&gt; GSM317669     1  0.0000      0.947 1.000 0.000
#&gt; GSM317671     1  0.0000      0.947 1.000 0.000
#&gt; GSM317676     1  0.6887      0.841 0.816 0.184
#&gt; GSM317680     1  0.0000      0.947 1.000 0.000
#&gt; GSM317684     1  0.4690      0.909 0.900 0.100
#&gt; GSM317685     1  0.0000      0.947 1.000 0.000
#&gt; GSM317694     1  0.0376      0.947 0.996 0.004
</code></pre>

<script>
$('#tab-CV-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-1-a').click(function(){
  $('#tab-CV-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-2'>
<p><a id='tab-CV-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM317649     1  0.0000     0.8898 1.000 0.000 0.000
#&gt; GSM317652     1  0.0000     0.8898 1.000 0.000 0.000
#&gt; GSM317666     1  0.1753     0.8836 0.952 0.000 0.048
#&gt; GSM317672     1  0.0237     0.8900 0.996 0.004 0.000
#&gt; GSM317679     1  0.0000     0.8898 1.000 0.000 0.000
#&gt; GSM317681     1  0.0000     0.8898 1.000 0.000 0.000
#&gt; GSM317682     1  0.0000     0.8898 1.000 0.000 0.000
#&gt; GSM317683     1  0.4654     0.7116 0.792 0.000 0.208
#&gt; GSM317689     1  0.3816     0.7994 0.852 0.000 0.148
#&gt; GSM317691     1  0.3590     0.8697 0.896 0.076 0.028
#&gt; GSM317692     1  0.0000     0.8898 1.000 0.000 0.000
#&gt; GSM317693     1  0.4165     0.8572 0.876 0.076 0.048
#&gt; GSM317696     1  0.2261     0.8815 0.932 0.068 0.000
#&gt; GSM317697     1  0.4165     0.8572 0.876 0.076 0.048
#&gt; GSM317698     1  0.3325     0.8705 0.904 0.076 0.020
#&gt; GSM317650     2  0.3482     0.0000 0.000 0.872 0.128
#&gt; GSM317651     1  0.0000     0.8898 1.000 0.000 0.000
#&gt; GSM317657     1  0.4796     0.7019 0.780 0.000 0.220
#&gt; GSM317667     3  0.5956     0.0000 0.324 0.004 0.672
#&gt; GSM317670     1  0.3921     0.8605 0.884 0.080 0.036
#&gt; GSM317674     1  0.2866     0.8743 0.916 0.076 0.008
#&gt; GSM317675     1  0.3031     0.8720 0.912 0.076 0.012
#&gt; GSM317677     1  0.3031     0.8720 0.912 0.076 0.012
#&gt; GSM317678     1  0.4062     0.7812 0.836 0.000 0.164
#&gt; GSM317687     1  0.2434     0.8886 0.940 0.036 0.024
#&gt; GSM317695     1  0.0000     0.8898 1.000 0.000 0.000
#&gt; GSM317653     1  0.1031     0.8883 0.976 0.000 0.024
#&gt; GSM317656     1  0.0000     0.8898 1.000 0.000 0.000
#&gt; GSM317658     1  0.2297     0.8839 0.944 0.020 0.036
#&gt; GSM317660     1  0.0000     0.8898 1.000 0.000 0.000
#&gt; GSM317663     1  0.3116     0.8270 0.892 0.000 0.108
#&gt; GSM317664     1  0.2200     0.8845 0.940 0.056 0.004
#&gt; GSM317665     1  0.0892     0.8912 0.980 0.020 0.000
#&gt; GSM317673     1  0.2590     0.8800 0.924 0.072 0.004
#&gt; GSM317686     1  0.4842     0.6946 0.776 0.000 0.224
#&gt; GSM317688     1  0.1585     0.8896 0.964 0.028 0.008
#&gt; GSM317690     1  0.4002     0.7850 0.840 0.000 0.160
#&gt; GSM317654     1  0.0000     0.8898 1.000 0.000 0.000
#&gt; GSM317655     1  0.5426     0.8025 0.820 0.088 0.092
#&gt; GSM317659     1  0.3031     0.8720 0.912 0.076 0.012
#&gt; GSM317661     1  0.6282     0.0778 0.612 0.004 0.384
#&gt; GSM317662     1  0.6313     0.4010 0.676 0.016 0.308
#&gt; GSM317668     1  0.3690     0.8614 0.884 0.100 0.016
#&gt; GSM317669     1  0.0000     0.8898 1.000 0.000 0.000
#&gt; GSM317671     1  0.0000     0.8898 1.000 0.000 0.000
#&gt; GSM317676     1  0.4291     0.7559 0.820 0.000 0.180
#&gt; GSM317680     1  0.0000     0.8898 1.000 0.000 0.000
#&gt; GSM317684     1  0.4075     0.8621 0.880 0.072 0.048
#&gt; GSM317685     1  0.0000     0.8898 1.000 0.000 0.000
#&gt; GSM317694     1  0.0592     0.8909 0.988 0.012 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-2-a').click(function(){
  $('#tab-CV-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-3'>
<p><a id='tab-CV-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317652     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317666     1  0.1388      0.899 0.960 0.000 0.012 0.028
#&gt; GSM317672     1  0.0188      0.902 0.996 0.000 0.004 0.000
#&gt; GSM317679     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317681     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317682     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317683     1  0.4151      0.772 0.800 0.004 0.016 0.180
#&gt; GSM317689     1  0.3519      0.829 0.852 0.004 0.016 0.128
#&gt; GSM317691     1  0.2714      0.880 0.884 0.000 0.112 0.004
#&gt; GSM317692     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317693     1  0.2773      0.877 0.880 0.000 0.116 0.004
#&gt; GSM317696     1  0.1940      0.893 0.924 0.000 0.076 0.000
#&gt; GSM317697     1  0.2899      0.878 0.880 0.004 0.112 0.004
#&gt; GSM317698     1  0.2593      0.881 0.892 0.000 0.104 0.004
#&gt; GSM317650     2  0.1576      0.000 0.000 0.948 0.004 0.048
#&gt; GSM317651     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317657     1  0.4406      0.764 0.788 0.004 0.024 0.184
#&gt; GSM317667     4  0.3024      0.000 0.148 0.000 0.000 0.852
#&gt; GSM317670     1  0.3441      0.858 0.856 0.024 0.120 0.000
#&gt; GSM317674     1  0.2466      0.884 0.900 0.000 0.096 0.004
#&gt; GSM317675     1  0.2530      0.882 0.896 0.000 0.100 0.004
#&gt; GSM317677     1  0.2530      0.882 0.896 0.000 0.100 0.004
#&gt; GSM317678     1  0.3679      0.819 0.840 0.004 0.016 0.140
#&gt; GSM317687     1  0.1675      0.902 0.948 0.004 0.044 0.004
#&gt; GSM317695     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317653     1  0.0967      0.901 0.976 0.004 0.004 0.016
#&gt; GSM317656     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317658     1  0.1398      0.902 0.956 0.004 0.040 0.000
#&gt; GSM317660     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317663     1  0.2647      0.844 0.880 0.000 0.000 0.120
#&gt; GSM317664     1  0.1792      0.896 0.932 0.000 0.068 0.000
#&gt; GSM317665     1  0.0817      0.903 0.976 0.000 0.024 0.000
#&gt; GSM317673     1  0.2216      0.889 0.908 0.000 0.092 0.000
#&gt; GSM317686     1  0.4448      0.759 0.784 0.004 0.024 0.188
#&gt; GSM317688     1  0.1305      0.902 0.960 0.000 0.036 0.004
#&gt; GSM317690     1  0.3774      0.824 0.844 0.008 0.020 0.128
#&gt; GSM317654     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317655     1  0.4536      0.826 0.812 0.020 0.136 0.032
#&gt; GSM317659     1  0.2530      0.882 0.896 0.000 0.100 0.004
#&gt; GSM317661     1  0.7977     -0.404 0.432 0.008 0.232 0.328
#&gt; GSM317662     3  0.7006      0.000 0.236 0.032 0.632 0.100
#&gt; GSM317668     1  0.3789      0.846 0.836 0.020 0.140 0.004
#&gt; GSM317669     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317671     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317676     1  0.3852      0.787 0.808 0.000 0.012 0.180
#&gt; GSM317680     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317684     1  0.3463      0.872 0.864 0.000 0.096 0.040
#&gt; GSM317685     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317694     1  0.0592      0.903 0.984 0.000 0.016 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-3-a').click(function(){
  $('#tab-CV-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-4'>
<p><a id='tab-CV-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317652     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317666     1  0.1195      0.931 0.960 0.000 0.000 0.028 0.012
#&gt; GSM317672     1  0.0162      0.932 0.996 0.000 0.004 0.000 0.000
#&gt; GSM317679     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317681     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317682     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317683     1  0.3359      0.857 0.816 0.000 0.000 0.164 0.020
#&gt; GSM317689     1  0.2921      0.886 0.856 0.000 0.000 0.124 0.020
#&gt; GSM317691     1  0.2727      0.912 0.868 0.000 0.116 0.000 0.016
#&gt; GSM317692     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317693     1  0.2825      0.908 0.860 0.000 0.124 0.000 0.016
#&gt; GSM317696     1  0.1792      0.925 0.916 0.000 0.084 0.000 0.000
#&gt; GSM317697     1  0.2873      0.909 0.860 0.000 0.120 0.000 0.020
#&gt; GSM317698     1  0.2488      0.911 0.872 0.000 0.124 0.000 0.004
#&gt; GSM317650     2  0.0162      0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM317651     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317657     1  0.4117      0.846 0.788 0.000 0.028 0.164 0.020
#&gt; GSM317667     4  0.0162      0.000 0.004 0.000 0.000 0.996 0.000
#&gt; GSM317670     1  0.3197      0.891 0.836 0.000 0.140 0.000 0.024
#&gt; GSM317674     1  0.2179      0.917 0.888 0.000 0.112 0.000 0.000
#&gt; GSM317675     1  0.2329      0.912 0.876 0.000 0.124 0.000 0.000
#&gt; GSM317677     1  0.2329      0.912 0.876 0.000 0.124 0.000 0.000
#&gt; GSM317678     1  0.3016      0.882 0.848 0.000 0.000 0.132 0.020
#&gt; GSM317687     1  0.1525      0.933 0.948 0.000 0.036 0.004 0.012
#&gt; GSM317695     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317653     1  0.0798      0.932 0.976 0.000 0.000 0.016 0.008
#&gt; GSM317656     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317658     1  0.1310      0.933 0.956 0.000 0.024 0.000 0.020
#&gt; GSM317660     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317663     1  0.2179      0.899 0.888 0.000 0.000 0.112 0.000
#&gt; GSM317664     1  0.1671      0.927 0.924 0.000 0.076 0.000 0.000
#&gt; GSM317665     1  0.0794      0.933 0.972 0.000 0.028 0.000 0.000
#&gt; GSM317673     1  0.2233      0.919 0.892 0.000 0.104 0.000 0.004
#&gt; GSM317686     1  0.4274      0.834 0.776 0.000 0.032 0.172 0.020
#&gt; GSM317688     1  0.1121      0.932 0.956 0.000 0.044 0.000 0.000
#&gt; GSM317690     1  0.3106      0.887 0.856 0.000 0.008 0.116 0.020
#&gt; GSM317654     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317655     1  0.3888      0.870 0.796 0.000 0.168 0.020 0.016
#&gt; GSM317659     1  0.2329      0.912 0.876 0.000 0.124 0.000 0.000
#&gt; GSM317661     3  0.3709      0.000 0.012 0.004 0.808 0.164 0.012
#&gt; GSM317662     5  0.0566      0.000 0.012 0.000 0.000 0.004 0.984
#&gt; GSM317668     1  0.3282      0.874 0.804 0.000 0.188 0.000 0.008
#&gt; GSM317669     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317671     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317676     1  0.3449      0.859 0.812 0.000 0.024 0.164 0.000
#&gt; GSM317680     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317684     1  0.3051      0.906 0.852 0.000 0.120 0.028 0.000
#&gt; GSM317685     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317694     1  0.0510      0.933 0.984 0.000 0.016 0.000 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-4-a').click(function(){
  $('#tab-CV-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-5'>
<p><a id='tab-CV-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM317649     1  0.0000      0.896 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317652     1  0.0000      0.896 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317666     1  0.1363      0.895 0.952 0.012 0.000 0.028 0.004 0.004
#&gt; GSM317672     1  0.0146      0.897 0.996 0.004 0.000 0.000 0.000 0.000
#&gt; GSM317679     1  0.0000      0.896 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317681     1  0.0000      0.896 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317682     1  0.0000      0.896 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317683     1  0.3242      0.834 0.840 0.008 0.016 0.120 0.004 0.012
#&gt; GSM317689     1  0.2869      0.853 0.864 0.008 0.008 0.104 0.004 0.012
#&gt; GSM317691     1  0.2362      0.877 0.860 0.136 0.000 0.000 0.000 0.004
#&gt; GSM317692     1  0.0000      0.896 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317693     1  0.2946      0.845 0.808 0.184 0.000 0.000 0.004 0.004
#&gt; GSM317696     1  0.1863      0.887 0.896 0.104 0.000 0.000 0.000 0.000
#&gt; GSM317697     1  0.3053      0.849 0.812 0.172 0.000 0.000 0.004 0.012
#&gt; GSM317698     1  0.2631      0.849 0.820 0.180 0.000 0.000 0.000 0.000
#&gt; GSM317650     2  0.3172      0.000 0.000 0.816 0.036 0.000 0.148 0.000
#&gt; GSM317651     1  0.0000      0.896 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317657     1  0.4417      0.807 0.772 0.076 0.016 0.120 0.004 0.012
#&gt; GSM317667     4  0.0000      0.000 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317670     1  0.3644      0.836 0.812 0.068 0.004 0.000 0.108 0.008
#&gt; GSM317674     1  0.2178      0.876 0.868 0.132 0.000 0.000 0.000 0.000
#&gt; GSM317675     1  0.2562      0.851 0.828 0.172 0.000 0.000 0.000 0.000
#&gt; GSM317677     1  0.2597      0.850 0.824 0.176 0.000 0.000 0.000 0.000
#&gt; GSM317678     1  0.2923      0.858 0.868 0.012 0.012 0.092 0.004 0.012
#&gt; GSM317687     1  0.1578      0.899 0.936 0.048 0.000 0.004 0.000 0.012
#&gt; GSM317695     1  0.0000      0.896 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317653     1  0.0717      0.897 0.976 0.000 0.000 0.016 0.000 0.008
#&gt; GSM317656     1  0.0146      0.896 0.996 0.004 0.000 0.000 0.000 0.000
#&gt; GSM317658     1  0.1218      0.899 0.956 0.028 0.000 0.000 0.004 0.012
#&gt; GSM317660     5  0.3351      0.000 0.288 0.000 0.000 0.000 0.712 0.000
#&gt; GSM317663     1  0.1858      0.873 0.912 0.000 0.012 0.076 0.000 0.000
#&gt; GSM317664     1  0.1863      0.887 0.896 0.104 0.000 0.000 0.000 0.000
#&gt; GSM317665     1  0.0790      0.899 0.968 0.032 0.000 0.000 0.000 0.000
#&gt; GSM317673     1  0.2100      0.885 0.884 0.112 0.000 0.000 0.000 0.004
#&gt; GSM317686     1  0.5434      0.736 0.712 0.096 0.016 0.124 0.040 0.012
#&gt; GSM317688     1  0.1141      0.898 0.948 0.052 0.000 0.000 0.000 0.000
#&gt; GSM317690     1  0.3034      0.856 0.864 0.012 0.008 0.092 0.012 0.012
#&gt; GSM317654     1  0.0000      0.896 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317655     1  0.4402      0.790 0.752 0.124 0.004 0.012 0.108 0.000
#&gt; GSM317659     1  0.2597      0.850 0.824 0.176 0.000 0.000 0.000 0.000
#&gt; GSM317661     3  0.0937      0.000 0.000 0.000 0.960 0.040 0.000 0.000
#&gt; GSM317662     6  0.0000      0.000 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM317668     1  0.4199      0.786 0.748 0.148 0.004 0.000 0.100 0.000
#&gt; GSM317669     1  0.0000      0.896 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317671     1  0.0000      0.896 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317676     1  0.4129      0.807 0.772 0.092 0.016 0.120 0.000 0.000
#&gt; GSM317680     1  0.0000      0.896 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317684     1  0.3268      0.845 0.808 0.164 0.008 0.020 0.000 0.000
#&gt; GSM317685     1  0.0146      0.896 0.996 0.004 0.000 0.000 0.000 0.000
#&gt; GSM317694     1  0.0547      0.899 0.980 0.020 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-5-a').click(function(){
  $('#tab-CV-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-membership-heatmap'>
<ul>
<li><a href='#tab-CV-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-pam-signature_compare](figure_cola/CV-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-pam-dimension-reduction'>
<ul>
<li><a href='#tab-CV-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-pam-collect-classes](figure_cola/CV-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n disease.state(p) k
#> CV:pam 49               NA 2
#> CV:pam 46               NA 3
#> CV:pam 46               NA 4
#> CV:pam 46               NA 5
#> CV:pam 45               NA 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "mclust"]
# you can also extract it by
# res = res_list["CV:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-mclust-collect-plots](figure_cola/CV-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-mclust-select-partition-number](figure_cola/CV-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.916           0.911       0.964        0.50301 0.493   0.493
#> 3 3 0.958           0.928       0.966        0.19260 0.874   0.754
#> 4 4 0.805           0.793       0.907        0.00479 0.724   0.496
#> 5 5 0.838           0.783       0.846        0.13840 0.867   0.689
#> 6 6 0.685           0.745       0.799        0.08424 0.951   0.851
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-classes'>
<ul>
<li><a href='#tab-CV-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-mclust-get-classes-1'>
<p><a id='tab-CV-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     2  0.0672     0.9714 0.008 0.992
#&gt; GSM317652     1  0.3879     0.8891 0.924 0.076
#&gt; GSM317666     2  0.0672     0.9714 0.008 0.992
#&gt; GSM317672     2  0.0672     0.9714 0.008 0.992
#&gt; GSM317679     2  0.0672     0.9714 0.008 0.992
#&gt; GSM317681     2  0.0672     0.9714 0.008 0.992
#&gt; GSM317682     1  0.0000     0.9517 1.000 0.000
#&gt; GSM317683     2  0.0000     0.9687 0.000 1.000
#&gt; GSM317689     2  0.0672     0.9714 0.008 0.992
#&gt; GSM317691     1  0.0000     0.9517 1.000 0.000
#&gt; GSM317692     2  0.9850     0.2215 0.428 0.572
#&gt; GSM317693     1  0.0000     0.9517 1.000 0.000
#&gt; GSM317696     1  0.0000     0.9517 1.000 0.000
#&gt; GSM317697     1  0.0000     0.9517 1.000 0.000
#&gt; GSM317698     1  0.0000     0.9517 1.000 0.000
#&gt; GSM317650     2  0.0000     0.9687 0.000 1.000
#&gt; GSM317651     1  1.0000    -0.0123 0.500 0.500
#&gt; GSM317657     2  0.0672     0.9714 0.008 0.992
#&gt; GSM317667     2  0.0000     0.9687 0.000 1.000
#&gt; GSM317670     2  0.0376     0.9676 0.004 0.996
#&gt; GSM317674     1  0.0000     0.9517 1.000 0.000
#&gt; GSM317675     1  0.0000     0.9517 1.000 0.000
#&gt; GSM317677     1  0.0000     0.9517 1.000 0.000
#&gt; GSM317678     2  0.0672     0.9714 0.008 0.992
#&gt; GSM317687     1  0.8555     0.5988 0.720 0.280
#&gt; GSM317695     1  0.5842     0.8206 0.860 0.140
#&gt; GSM317653     2  0.0672     0.9714 0.008 0.992
#&gt; GSM317656     1  0.0000     0.9517 1.000 0.000
#&gt; GSM317658     1  0.0000     0.9517 1.000 0.000
#&gt; GSM317660     2  0.0672     0.9714 0.008 0.992
#&gt; GSM317663     2  0.0672     0.9714 0.008 0.992
#&gt; GSM317664     1  0.0000     0.9517 1.000 0.000
#&gt; GSM317665     2  0.0672     0.9714 0.008 0.992
#&gt; GSM317673     1  0.0000     0.9517 1.000 0.000
#&gt; GSM317686     2  0.0000     0.9687 0.000 1.000
#&gt; GSM317688     1  0.0000     0.9517 1.000 0.000
#&gt; GSM317690     2  0.0000     0.9687 0.000 1.000
#&gt; GSM317654     2  0.0672     0.9714 0.008 0.992
#&gt; GSM317655     2  0.0000     0.9687 0.000 1.000
#&gt; GSM317659     1  0.0000     0.9517 1.000 0.000
#&gt; GSM317661     2  0.0000     0.9687 0.000 1.000
#&gt; GSM317662     2  0.0000     0.9687 0.000 1.000
#&gt; GSM317668     1  0.0376     0.9492 0.996 0.004
#&gt; GSM317669     2  0.0672     0.9714 0.008 0.992
#&gt; GSM317671     2  0.0672     0.9714 0.008 0.992
#&gt; GSM317676     2  0.7376     0.7260 0.208 0.792
#&gt; GSM317680     2  0.0672     0.9714 0.008 0.992
#&gt; GSM317684     1  0.0000     0.9517 1.000 0.000
#&gt; GSM317685     1  0.0938     0.9438 0.988 0.012
#&gt; GSM317694     1  0.0000     0.9517 1.000 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-1-a').click(function(){
  $('#tab-CV-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-2'>
<p><a id='tab-CV-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM317649     3  0.0747      0.957 0.000 0.016 0.984
#&gt; GSM317652     1  0.0848      0.972 0.984 0.008 0.008
#&gt; GSM317666     2  0.0424      0.939 0.000 0.992 0.008
#&gt; GSM317672     2  0.0592      0.937 0.000 0.988 0.012
#&gt; GSM317679     3  0.0892      0.957 0.000 0.020 0.980
#&gt; GSM317681     2  0.2448      0.893 0.000 0.924 0.076
#&gt; GSM317682     1  0.0237      0.979 0.996 0.000 0.004
#&gt; GSM317683     2  0.0424      0.937 0.000 0.992 0.008
#&gt; GSM317689     2  0.0000      0.938 0.000 1.000 0.000
#&gt; GSM317691     1  0.0000      0.980 1.000 0.000 0.000
#&gt; GSM317692     1  0.4796      0.685 0.780 0.220 0.000
#&gt; GSM317693     1  0.0000      0.980 1.000 0.000 0.000
#&gt; GSM317696     1  0.0237      0.979 0.996 0.000 0.004
#&gt; GSM317697     1  0.0000      0.980 1.000 0.000 0.000
#&gt; GSM317698     1  0.0000      0.980 1.000 0.000 0.000
#&gt; GSM317650     2  0.0592      0.935 0.000 0.988 0.012
#&gt; GSM317651     1  0.2496      0.908 0.928 0.068 0.004
#&gt; GSM317657     2  0.0424      0.939 0.000 0.992 0.008
#&gt; GSM317667     2  0.0424      0.939 0.000 0.992 0.008
#&gt; GSM317670     2  0.0848      0.933 0.008 0.984 0.008
#&gt; GSM317674     1  0.0237      0.979 0.996 0.000 0.004
#&gt; GSM317675     1  0.0000      0.980 1.000 0.000 0.000
#&gt; GSM317677     1  0.0000      0.980 1.000 0.000 0.000
#&gt; GSM317678     2  0.0424      0.938 0.000 0.992 0.008
#&gt; GSM317687     1  0.1411      0.946 0.964 0.036 0.000
#&gt; GSM317695     1  0.0661      0.975 0.988 0.004 0.008
#&gt; GSM317653     2  0.0424      0.939 0.000 0.992 0.008
#&gt; GSM317656     1  0.0000      0.980 1.000 0.000 0.000
#&gt; GSM317658     1  0.0000      0.980 1.000 0.000 0.000
#&gt; GSM317660     2  0.2448      0.891 0.000 0.924 0.076
#&gt; GSM317663     2  0.0424      0.939 0.000 0.992 0.008
#&gt; GSM317664     1  0.0237      0.979 0.996 0.000 0.004
#&gt; GSM317665     2  0.5650      0.556 0.000 0.688 0.312
#&gt; GSM317673     1  0.0237      0.979 0.996 0.000 0.004
#&gt; GSM317686     2  0.0424      0.939 0.000 0.992 0.008
#&gt; GSM317688     1  0.0237      0.979 0.996 0.000 0.004
#&gt; GSM317690     2  0.0424      0.937 0.000 0.992 0.008
#&gt; GSM317654     2  0.4178      0.790 0.000 0.828 0.172
#&gt; GSM317655     2  0.0424      0.937 0.000 0.992 0.008
#&gt; GSM317659     1  0.0000      0.980 1.000 0.000 0.000
#&gt; GSM317661     2  0.0592      0.935 0.000 0.988 0.012
#&gt; GSM317662     2  0.0592      0.935 0.000 0.988 0.012
#&gt; GSM317668     1  0.0000      0.980 1.000 0.000 0.000
#&gt; GSM317669     3  0.4002      0.818 0.000 0.160 0.840
#&gt; GSM317671     3  0.0747      0.957 0.000 0.016 0.984
#&gt; GSM317676     2  0.5722      0.511 0.292 0.704 0.004
#&gt; GSM317680     3  0.1031      0.956 0.000 0.024 0.976
#&gt; GSM317684     1  0.0000      0.980 1.000 0.000 0.000
#&gt; GSM317685     1  0.0661      0.975 0.988 0.004 0.008
#&gt; GSM317694     1  0.0237      0.979 0.996 0.000 0.004
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-2-a').click(function(){
  $('#tab-CV-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-3'>
<p><a id='tab-CV-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     3  0.0336      0.864 0.008 0.000 0.992 0.000
#&gt; GSM317652     1  0.0469      0.917 0.988 0.000 0.000 0.012
#&gt; GSM317666     4  0.4870      0.782 0.096 0.056 0.036 0.812
#&gt; GSM317672     3  0.5105      0.782 0.060 0.056 0.804 0.080
#&gt; GSM317679     3  0.0336      0.864 0.008 0.000 0.992 0.000
#&gt; GSM317681     3  0.3372      0.854 0.000 0.036 0.868 0.096
#&gt; GSM317682     1  0.0707      0.916 0.980 0.000 0.000 0.020
#&gt; GSM317683     2  0.3900      0.585 0.164 0.816 0.000 0.020
#&gt; GSM317689     1  0.7496     -0.262 0.444 0.444 0.072 0.040
#&gt; GSM317691     1  0.0336      0.916 0.992 0.008 0.000 0.000
#&gt; GSM317692     1  0.0779      0.912 0.980 0.016 0.000 0.004
#&gt; GSM317693     1  0.0336      0.916 0.992 0.008 0.000 0.000
#&gt; GSM317696     1  0.0817      0.915 0.976 0.000 0.000 0.024
#&gt; GSM317697     1  0.0000      0.918 1.000 0.000 0.000 0.000
#&gt; GSM317698     1  0.0000      0.918 1.000 0.000 0.000 0.000
#&gt; GSM317650     2  0.0921      0.667 0.000 0.972 0.000 0.028
#&gt; GSM317651     1  0.0524      0.915 0.988 0.000 0.004 0.008
#&gt; GSM317657     1  0.3647      0.785 0.852 0.108 0.000 0.040
#&gt; GSM317667     4  0.1867      0.852 0.000 0.072 0.000 0.928
#&gt; GSM317670     1  0.5024      0.368 0.632 0.360 0.000 0.008
#&gt; GSM317674     1  0.0817      0.915 0.976 0.000 0.000 0.024
#&gt; GSM317675     1  0.0000      0.918 1.000 0.000 0.000 0.000
#&gt; GSM317677     1  0.0000      0.918 1.000 0.000 0.000 0.000
#&gt; GSM317678     3  0.6840      0.344 0.012 0.392 0.524 0.072
#&gt; GSM317687     1  0.0336      0.916 0.992 0.008 0.000 0.000
#&gt; GSM317695     1  0.0817      0.915 0.976 0.000 0.000 0.024
#&gt; GSM317653     4  0.4779      0.791 0.020 0.048 0.128 0.804
#&gt; GSM317656     1  0.0592      0.917 0.984 0.000 0.000 0.016
#&gt; GSM317658     1  0.0469      0.915 0.988 0.012 0.000 0.000
#&gt; GSM317660     3  0.3533      0.845 0.000 0.056 0.864 0.080
#&gt; GSM317663     1  0.7107      0.481 0.660 0.056 0.168 0.116
#&gt; GSM317664     1  0.0817      0.915 0.976 0.000 0.000 0.024
#&gt; GSM317665     3  0.3134      0.864 0.008 0.012 0.880 0.100
#&gt; GSM317673     1  0.0817      0.915 0.976 0.000 0.000 0.024
#&gt; GSM317686     4  0.1940      0.850 0.000 0.076 0.000 0.924
#&gt; GSM317688     1  0.0707      0.916 0.980 0.000 0.000 0.020
#&gt; GSM317690     2  0.5112      0.333 0.384 0.608 0.000 0.008
#&gt; GSM317654     3  0.3670      0.856 0.008 0.032 0.860 0.100
#&gt; GSM317655     1  0.5138      0.295 0.600 0.392 0.000 0.008
#&gt; GSM317659     1  0.0336      0.916 0.992 0.008 0.000 0.000
#&gt; GSM317661     2  0.1022      0.671 0.000 0.968 0.000 0.032
#&gt; GSM317662     2  0.1022      0.671 0.000 0.968 0.000 0.032
#&gt; GSM317668     1  0.0000      0.918 1.000 0.000 0.000 0.000
#&gt; GSM317669     3  0.1339      0.869 0.008 0.004 0.964 0.024
#&gt; GSM317671     3  0.0336      0.864 0.008 0.000 0.992 0.000
#&gt; GSM317676     1  0.1151      0.905 0.968 0.024 0.000 0.008
#&gt; GSM317680     3  0.0336      0.864 0.008 0.000 0.992 0.000
#&gt; GSM317684     1  0.0336      0.916 0.992 0.008 0.000 0.000
#&gt; GSM317685     1  0.0469      0.917 0.988 0.000 0.000 0.012
#&gt; GSM317694     1  0.0336      0.918 0.992 0.000 0.000 0.008
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-3-a').click(function(){
  $('#tab-CV-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-4'>
<p><a id='tab-CV-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     3  0.0000    0.79345 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317652     1  0.1461    0.95156 0.952 0.016 0.004 0.028 0.000
#&gt; GSM317666     4  0.2165    0.62341 0.016 0.056 0.004 0.920 0.004
#&gt; GSM317672     3  0.6972    0.30663 0.012 0.196 0.444 0.344 0.004
#&gt; GSM317679     3  0.0000    0.79345 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317681     3  0.4403    0.75340 0.004 0.032 0.724 0.240 0.000
#&gt; GSM317682     1  0.0486    0.96222 0.988 0.004 0.004 0.004 0.000
#&gt; GSM317683     2  0.5019    0.47744 0.024 0.536 0.000 0.004 0.436
#&gt; GSM317689     2  0.6636    0.54865 0.036 0.572 0.020 0.072 0.300
#&gt; GSM317691     1  0.0613    0.96237 0.984 0.008 0.000 0.004 0.004
#&gt; GSM317692     1  0.4478    0.53788 0.700 0.272 0.000 0.020 0.008
#&gt; GSM317693     1  0.0798    0.96082 0.976 0.016 0.000 0.008 0.000
#&gt; GSM317696     1  0.0613    0.96048 0.984 0.004 0.008 0.004 0.000
#&gt; GSM317697     1  0.0740    0.96179 0.980 0.008 0.000 0.008 0.004
#&gt; GSM317698     1  0.0775    0.96165 0.980 0.008 0.004 0.004 0.004
#&gt; GSM317650     5  0.0290    0.98789 0.000 0.008 0.000 0.000 0.992
#&gt; GSM317651     1  0.2011    0.92476 0.928 0.020 0.008 0.044 0.000
#&gt; GSM317657     2  0.6404    0.37757 0.300 0.568 0.000 0.092 0.040
#&gt; GSM317667     4  0.4851    0.52024 0.000 0.340 0.000 0.624 0.036
#&gt; GSM317670     2  0.6234    0.51769 0.176 0.528 0.000 0.000 0.296
#&gt; GSM317674     1  0.0451    0.96152 0.988 0.000 0.008 0.004 0.000
#&gt; GSM317675     1  0.1029    0.96063 0.972 0.008 0.008 0.008 0.004
#&gt; GSM317677     1  0.0451    0.96237 0.988 0.004 0.000 0.008 0.000
#&gt; GSM317678     2  0.8749    0.11866 0.012 0.340 0.252 0.188 0.208
#&gt; GSM317687     1  0.1211    0.95176 0.960 0.016 0.000 0.024 0.000
#&gt; GSM317695     1  0.1251    0.94464 0.956 0.008 0.036 0.000 0.000
#&gt; GSM317653     4  0.2220    0.62029 0.016 0.052 0.008 0.920 0.004
#&gt; GSM317656     1  0.0324    0.96205 0.992 0.000 0.004 0.004 0.000
#&gt; GSM317658     1  0.0932    0.96078 0.972 0.020 0.000 0.004 0.004
#&gt; GSM317660     3  0.4567    0.75188 0.004 0.044 0.720 0.232 0.000
#&gt; GSM317663     4  0.7666   -0.00838 0.076 0.324 0.140 0.452 0.008
#&gt; GSM317664     1  0.0671    0.95894 0.980 0.004 0.016 0.000 0.000
#&gt; GSM317665     3  0.4288    0.76145 0.004 0.032 0.740 0.224 0.000
#&gt; GSM317673     1  0.0324    0.96258 0.992 0.000 0.004 0.004 0.000
#&gt; GSM317686     4  0.4866    0.52232 0.000 0.344 0.000 0.620 0.036
#&gt; GSM317688     1  0.0486    0.96263 0.988 0.004 0.000 0.004 0.004
#&gt; GSM317690     2  0.5118    0.54719 0.036 0.584 0.000 0.004 0.376
#&gt; GSM317654     3  0.4318    0.76034 0.004 0.032 0.736 0.228 0.000
#&gt; GSM317655     2  0.6063    0.58845 0.096 0.588 0.000 0.020 0.296
#&gt; GSM317659     1  0.0671    0.96069 0.980 0.016 0.000 0.004 0.000
#&gt; GSM317661     5  0.0162    0.98988 0.000 0.004 0.000 0.000 0.996
#&gt; GSM317662     5  0.0000    0.99155 0.000 0.000 0.000 0.000 1.000
#&gt; GSM317668     1  0.0613    0.96157 0.984 0.008 0.000 0.004 0.004
#&gt; GSM317669     3  0.0290    0.79390 0.000 0.000 0.992 0.008 0.000
#&gt; GSM317671     3  0.0000    0.79345 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317676     1  0.3334    0.85374 0.852 0.064 0.000 0.080 0.004
#&gt; GSM317680     3  0.0000    0.79345 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317684     1  0.0798    0.96066 0.976 0.016 0.000 0.008 0.000
#&gt; GSM317685     1  0.1074    0.96062 0.968 0.016 0.004 0.012 0.000
#&gt; GSM317694     1  0.0486    0.96222 0.988 0.004 0.004 0.004 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-4-a').click(function(){
  $('#tab-CV-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-5'>
<p><a id='tab-CV-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM317649     3  0.3714     0.9909 0.000 0.004 0.656 0.000 0.340 0.000
#&gt; GSM317652     1  0.3192     0.8540 0.828 0.136 0.016 0.000 0.020 0.000
#&gt; GSM317666     5  0.6356     0.0997 0.012 0.176 0.004 0.012 0.504 0.292
#&gt; GSM317672     5  0.2293     0.4585 0.016 0.004 0.004 0.080 0.896 0.000
#&gt; GSM317679     3  0.3578     0.9959 0.000 0.000 0.660 0.000 0.340 0.000
#&gt; GSM317681     5  0.3103     0.2565 0.008 0.000 0.208 0.000 0.784 0.000
#&gt; GSM317682     1  0.2313     0.8550 0.884 0.100 0.004 0.000 0.012 0.000
#&gt; GSM317683     4  0.1293     0.8339 0.000 0.020 0.016 0.956 0.004 0.004
#&gt; GSM317689     4  0.3019     0.7643 0.008 0.020 0.004 0.844 0.124 0.000
#&gt; GSM317691     1  0.1806     0.8575 0.908 0.088 0.000 0.000 0.004 0.000
#&gt; GSM317692     1  0.5549     0.3356 0.552 0.044 0.000 0.348 0.056 0.000
#&gt; GSM317693     1  0.2838     0.8209 0.808 0.188 0.000 0.000 0.000 0.004
#&gt; GSM317696     1  0.2346     0.8457 0.868 0.124 0.000 0.000 0.008 0.000
#&gt; GSM317697     1  0.2100     0.8524 0.884 0.112 0.000 0.000 0.000 0.004
#&gt; GSM317698     1  0.1588     0.8629 0.924 0.072 0.000 0.000 0.000 0.004
#&gt; GSM317650     2  0.6322     0.9846 0.000 0.412 0.316 0.260 0.000 0.012
#&gt; GSM317651     1  0.4577     0.8172 0.752 0.152 0.016 0.008 0.064 0.008
#&gt; GSM317657     4  0.4665     0.6479 0.104 0.060 0.004 0.768 0.052 0.012
#&gt; GSM317667     6  0.0146     0.9825 0.000 0.004 0.000 0.000 0.000 0.996
#&gt; GSM317670     4  0.0924     0.8592 0.008 0.008 0.004 0.972 0.008 0.000
#&gt; GSM317674     1  0.2212     0.8500 0.880 0.112 0.000 0.000 0.008 0.000
#&gt; GSM317675     1  0.1812     0.8631 0.912 0.080 0.000 0.000 0.008 0.000
#&gt; GSM317677     1  0.2100     0.8531 0.884 0.112 0.004 0.000 0.000 0.000
#&gt; GSM317678     5  0.5210     0.0484 0.008 0.028 0.024 0.424 0.516 0.000
#&gt; GSM317687     1  0.3748     0.7760 0.748 0.224 0.016 0.000 0.012 0.000
#&gt; GSM317695     1  0.3354     0.8132 0.796 0.168 0.036 0.000 0.000 0.000
#&gt; GSM317653     5  0.6140     0.1317 0.008 0.176 0.004 0.008 0.524 0.280
#&gt; GSM317656     1  0.2673     0.8436 0.852 0.132 0.004 0.000 0.012 0.000
#&gt; GSM317658     1  0.1779     0.8633 0.920 0.064 0.000 0.000 0.016 0.000
#&gt; GSM317660     5  0.3383     0.2501 0.004 0.004 0.208 0.008 0.776 0.000
#&gt; GSM317663     5  0.5469     0.3954 0.020 0.180 0.000 0.112 0.668 0.020
#&gt; GSM317664     1  0.3053     0.8206 0.812 0.172 0.012 0.000 0.004 0.000
#&gt; GSM317665     5  0.3271     0.2072 0.008 0.000 0.232 0.000 0.760 0.000
#&gt; GSM317673     1  0.2313     0.8547 0.884 0.100 0.004 0.000 0.012 0.000
#&gt; GSM317686     6  0.0405     0.9825 0.000 0.000 0.004 0.008 0.000 0.988
#&gt; GSM317688     1  0.0767     0.8666 0.976 0.012 0.004 0.000 0.008 0.000
#&gt; GSM317690     4  0.0291     0.8587 0.000 0.000 0.000 0.992 0.004 0.004
#&gt; GSM317654     5  0.3161     0.2403 0.008 0.000 0.216 0.000 0.776 0.000
#&gt; GSM317655     4  0.0665     0.8604 0.000 0.008 0.004 0.980 0.008 0.000
#&gt; GSM317659     1  0.2520     0.8345 0.844 0.152 0.000 0.000 0.004 0.000
#&gt; GSM317661     2  0.6326     0.9923 0.000 0.412 0.312 0.264 0.000 0.012
#&gt; GSM317662     2  0.6326     0.9923 0.000 0.412 0.312 0.264 0.000 0.012
#&gt; GSM317668     1  0.1261     0.8674 0.956 0.028 0.004 0.000 0.008 0.004
#&gt; GSM317669     3  0.3592     0.9912 0.000 0.000 0.656 0.000 0.344 0.000
#&gt; GSM317671     3  0.3578     0.9959 0.000 0.000 0.660 0.000 0.340 0.000
#&gt; GSM317676     1  0.5534     0.6889 0.652 0.236 0.020 0.032 0.056 0.004
#&gt; GSM317680     3  0.3578     0.9959 0.000 0.000 0.660 0.000 0.340 0.000
#&gt; GSM317684     1  0.2793     0.8161 0.800 0.200 0.000 0.000 0.000 0.000
#&gt; GSM317685     1  0.2692     0.8601 0.840 0.148 0.000 0.000 0.012 0.000
#&gt; GSM317694     1  0.2302     0.8535 0.872 0.120 0.000 0.000 0.008 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-5-a').click(function(){
  $('#tab-CV-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-mclust-signature_compare](figure_cola/CV-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-mclust-collect-classes](figure_cola/CV-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) k
#> CV:mclust 48            0.929 2
#> CV:mclust 50            0.447 3
#> CV:mclust 44            0.519 4
#> CV:mclust 45            0.689 5
#> CV:mclust 40            0.570 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:NMF






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "NMF"]
# you can also extract it by
# res = res_list["CV:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-NMF-collect-plots](figure_cola/CV-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-NMF-select-partition-number](figure_cola/CV-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.722           0.845       0.936         0.5039 0.490   0.490
#> 3 3 0.661           0.776       0.896         0.3101 0.816   0.638
#> 4 4 0.640           0.718       0.853         0.0961 0.881   0.685
#> 5 5 0.644           0.558       0.768         0.0662 0.878   0.623
#> 6 6 0.709           0.665       0.820         0.0462 0.932   0.727
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-classes'>
<ul>
<li><a href='#tab-CV-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-NMF-get-classes-1'>
<p><a id='tab-CV-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     2  0.9850      0.333 0.428 0.572
#&gt; GSM317652     1  0.2423      0.930 0.960 0.040
#&gt; GSM317666     2  0.0000      0.887 0.000 1.000
#&gt; GSM317672     2  0.0000      0.887 0.000 1.000
#&gt; GSM317679     2  0.7883      0.686 0.236 0.764
#&gt; GSM317681     2  0.0000      0.887 0.000 1.000
#&gt; GSM317682     1  0.0000      0.969 1.000 0.000
#&gt; GSM317683     2  0.0000      0.887 0.000 1.000
#&gt; GSM317689     2  0.0000      0.887 0.000 1.000
#&gt; GSM317691     1  0.0000      0.969 1.000 0.000
#&gt; GSM317692     2  0.8207      0.656 0.256 0.744
#&gt; GSM317693     1  0.0000      0.969 1.000 0.000
#&gt; GSM317696     1  0.0000      0.969 1.000 0.000
#&gt; GSM317697     1  0.0000      0.969 1.000 0.000
#&gt; GSM317698     1  0.0000      0.969 1.000 0.000
#&gt; GSM317650     2  0.0000      0.887 0.000 1.000
#&gt; GSM317651     1  0.9850      0.106 0.572 0.428
#&gt; GSM317657     2  0.9963      0.188 0.464 0.536
#&gt; GSM317667     2  0.0000      0.887 0.000 1.000
#&gt; GSM317670     1  0.0376      0.965 0.996 0.004
#&gt; GSM317674     1  0.0000      0.969 1.000 0.000
#&gt; GSM317675     1  0.0000      0.969 1.000 0.000
#&gt; GSM317677     1  0.0000      0.969 1.000 0.000
#&gt; GSM317678     2  0.0000      0.887 0.000 1.000
#&gt; GSM317687     1  0.0000      0.969 1.000 0.000
#&gt; GSM317695     1  0.0000      0.969 1.000 0.000
#&gt; GSM317653     2  0.0000      0.887 0.000 1.000
#&gt; GSM317656     1  0.1633      0.947 0.976 0.024
#&gt; GSM317658     1  0.0000      0.969 1.000 0.000
#&gt; GSM317660     2  0.0000      0.887 0.000 1.000
#&gt; GSM317663     2  0.0000      0.887 0.000 1.000
#&gt; GSM317664     1  0.0000      0.969 1.000 0.000
#&gt; GSM317665     2  0.0000      0.887 0.000 1.000
#&gt; GSM317673     1  0.0000      0.969 1.000 0.000
#&gt; GSM317686     2  0.0938      0.881 0.012 0.988
#&gt; GSM317688     1  0.0000      0.969 1.000 0.000
#&gt; GSM317690     2  0.0000      0.887 0.000 1.000
#&gt; GSM317654     2  0.0000      0.887 0.000 1.000
#&gt; GSM317655     2  0.5842      0.787 0.140 0.860
#&gt; GSM317659     1  0.0000      0.969 1.000 0.000
#&gt; GSM317661     2  0.0000      0.887 0.000 1.000
#&gt; GSM317662     2  0.0000      0.887 0.000 1.000
#&gt; GSM317668     1  0.0000      0.969 1.000 0.000
#&gt; GSM317669     2  0.5519      0.800 0.128 0.872
#&gt; GSM317671     2  0.9580      0.447 0.380 0.620
#&gt; GSM317676     1  0.6343      0.778 0.840 0.160
#&gt; GSM317680     2  0.9993      0.165 0.484 0.516
#&gt; GSM317684     1  0.0000      0.969 1.000 0.000
#&gt; GSM317685     1  0.0000      0.969 1.000 0.000
#&gt; GSM317694     1  0.0000      0.969 1.000 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-1-a').click(function(){
  $('#tab-CV-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-2'>
<p><a id='tab-CV-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM317649     3  0.1031      0.846 0.024 0.000 0.976
#&gt; GSM317652     1  0.5201      0.717 0.760 0.004 0.236
#&gt; GSM317666     2  0.0000      0.863 0.000 1.000 0.000
#&gt; GSM317672     3  0.6111      0.298 0.000 0.396 0.604
#&gt; GSM317679     3  0.0237      0.853 0.004 0.000 0.996
#&gt; GSM317681     3  0.1289      0.846 0.000 0.032 0.968
#&gt; GSM317682     1  0.4235      0.788 0.824 0.000 0.176
#&gt; GSM317683     2  0.0747      0.863 0.000 0.984 0.016
#&gt; GSM317689     2  0.4399      0.747 0.000 0.812 0.188
#&gt; GSM317691     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM317692     2  0.6482      0.623 0.244 0.716 0.040
#&gt; GSM317693     1  0.1411      0.881 0.964 0.036 0.000
#&gt; GSM317696     1  0.0424      0.894 0.992 0.000 0.008
#&gt; GSM317697     1  0.0424      0.892 0.992 0.008 0.000
#&gt; GSM317698     1  0.0237      0.894 0.996 0.000 0.004
#&gt; GSM317650     2  0.6168      0.293 0.000 0.588 0.412
#&gt; GSM317651     3  0.6051      0.464 0.292 0.012 0.696
#&gt; GSM317657     2  0.2711      0.813 0.088 0.912 0.000
#&gt; GSM317667     2  0.0237      0.863 0.004 0.996 0.000
#&gt; GSM317670     1  0.5948      0.435 0.640 0.360 0.000
#&gt; GSM317674     1  0.0747      0.893 0.984 0.000 0.016
#&gt; GSM317675     1  0.0424      0.894 0.992 0.000 0.008
#&gt; GSM317677     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM317678     3  0.5529      0.538 0.000 0.296 0.704
#&gt; GSM317687     1  0.2537      0.854 0.920 0.080 0.000
#&gt; GSM317695     1  0.6235      0.338 0.564 0.000 0.436
#&gt; GSM317653     2  0.2165      0.843 0.000 0.936 0.064
#&gt; GSM317656     1  0.6286      0.259 0.536 0.000 0.464
#&gt; GSM317658     1  0.0747      0.891 0.984 0.016 0.000
#&gt; GSM317660     3  0.4291      0.719 0.000 0.180 0.820
#&gt; GSM317663     2  0.0892      0.862 0.000 0.980 0.020
#&gt; GSM317664     1  0.2356      0.870 0.928 0.000 0.072
#&gt; GSM317665     3  0.0424      0.852 0.000 0.008 0.992
#&gt; GSM317673     1  0.3412      0.833 0.876 0.000 0.124
#&gt; GSM317686     2  0.0424      0.862 0.008 0.992 0.000
#&gt; GSM317688     1  0.0592      0.893 0.988 0.000 0.012
#&gt; GSM317690     2  0.0237      0.863 0.000 0.996 0.004
#&gt; GSM317654     3  0.1289      0.846 0.000 0.032 0.968
#&gt; GSM317655     2  0.0592      0.861 0.012 0.988 0.000
#&gt; GSM317659     1  0.2878      0.843 0.904 0.096 0.000
#&gt; GSM317661     2  0.4346      0.753 0.000 0.816 0.184
#&gt; GSM317662     2  0.3551      0.801 0.000 0.868 0.132
#&gt; GSM317668     1  0.0424      0.894 0.992 0.000 0.008
#&gt; GSM317669     3  0.0000      0.853 0.000 0.000 1.000
#&gt; GSM317671     3  0.0592      0.851 0.012 0.000 0.988
#&gt; GSM317676     2  0.4555      0.694 0.200 0.800 0.000
#&gt; GSM317680     3  0.1529      0.835 0.040 0.000 0.960
#&gt; GSM317684     1  0.0747      0.889 0.984 0.016 0.000
#&gt; GSM317685     1  0.2165      0.874 0.936 0.000 0.064
#&gt; GSM317694     1  0.0892      0.892 0.980 0.000 0.020
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-2-a').click(function(){
  $('#tab-CV-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-3'>
<p><a id='tab-CV-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     3  0.0469     0.8175 0.012 0.000 0.988 0.000
#&gt; GSM317652     1  0.8277     0.0198 0.412 0.016 0.292 0.280
#&gt; GSM317666     4  0.1004     0.7781 0.000 0.024 0.004 0.972
#&gt; GSM317672     3  0.6167     0.5652 0.000 0.096 0.648 0.256
#&gt; GSM317679     3  0.0188     0.8205 0.004 0.000 0.996 0.000
#&gt; GSM317681     3  0.2313     0.8147 0.000 0.044 0.924 0.032
#&gt; GSM317682     1  0.3123     0.7605 0.844 0.000 0.156 0.000
#&gt; GSM317683     2  0.2868     0.8657 0.000 0.864 0.000 0.136
#&gt; GSM317689     2  0.3205     0.8704 0.000 0.872 0.024 0.104
#&gt; GSM317691     1  0.0804     0.8515 0.980 0.000 0.008 0.012
#&gt; GSM317692     1  0.8124     0.0923 0.464 0.352 0.036 0.148
#&gt; GSM317693     1  0.1118     0.8433 0.964 0.000 0.000 0.036
#&gt; GSM317696     1  0.0336     0.8507 0.992 0.000 0.008 0.000
#&gt; GSM317697     1  0.0336     0.8503 0.992 0.000 0.000 0.008
#&gt; GSM317698     1  0.0188     0.8506 0.996 0.004 0.000 0.000
#&gt; GSM317650     2  0.2522     0.8591 0.000 0.908 0.016 0.076
#&gt; GSM317651     3  0.6495     0.6875 0.092 0.068 0.716 0.124
#&gt; GSM317657     4  0.6373     0.5127 0.148 0.200 0.000 0.652
#&gt; GSM317667     4  0.1022     0.7763 0.000 0.032 0.000 0.968
#&gt; GSM317670     1  0.6743     0.1372 0.472 0.452 0.008 0.068
#&gt; GSM317674     1  0.0524     0.8512 0.988 0.004 0.008 0.000
#&gt; GSM317675     1  0.0336     0.8512 0.992 0.008 0.000 0.000
#&gt; GSM317677     1  0.0524     0.8515 0.988 0.008 0.000 0.004
#&gt; GSM317678     3  0.6785     0.1023 0.000 0.420 0.484 0.096
#&gt; GSM317687     4  0.4585     0.4882 0.332 0.000 0.000 0.668
#&gt; GSM317695     3  0.4936     0.2735 0.372 0.004 0.624 0.000
#&gt; GSM317653     4  0.2706     0.7216 0.000 0.080 0.020 0.900
#&gt; GSM317656     1  0.4720     0.6974 0.768 0.032 0.196 0.004
#&gt; GSM317658     1  0.2153     0.8370 0.936 0.036 0.008 0.020
#&gt; GSM317660     3  0.4673     0.7571 0.000 0.132 0.792 0.076
#&gt; GSM317663     4  0.1913     0.7696 0.000 0.040 0.020 0.940
#&gt; GSM317664     1  0.2125     0.8319 0.920 0.004 0.076 0.000
#&gt; GSM317665     3  0.2816     0.8103 0.000 0.064 0.900 0.036
#&gt; GSM317673     1  0.1576     0.8441 0.948 0.004 0.048 0.000
#&gt; GSM317686     4  0.0817     0.7738 0.000 0.024 0.000 0.976
#&gt; GSM317688     1  0.0592     0.8513 0.984 0.000 0.016 0.000
#&gt; GSM317690     2  0.2589     0.8060 0.000 0.884 0.000 0.116
#&gt; GSM317654     3  0.3679     0.7974 0.000 0.084 0.856 0.060
#&gt; GSM317655     2  0.4244     0.7148 0.032 0.800 0.000 0.168
#&gt; GSM317659     1  0.3533     0.7917 0.864 0.056 0.000 0.080
#&gt; GSM317661     2  0.4322     0.8096 0.000 0.804 0.044 0.152
#&gt; GSM317662     2  0.3351     0.8627 0.000 0.844 0.008 0.148
#&gt; GSM317668     1  0.4739     0.7205 0.788 0.160 0.008 0.044
#&gt; GSM317669     3  0.0188     0.8203 0.000 0.004 0.996 0.000
#&gt; GSM317671     3  0.0188     0.8205 0.004 0.000 0.996 0.000
#&gt; GSM317676     4  0.6104     0.5973 0.140 0.180 0.000 0.680
#&gt; GSM317680     3  0.0188     0.8205 0.004 0.000 0.996 0.000
#&gt; GSM317684     1  0.1557     0.8344 0.944 0.000 0.000 0.056
#&gt; GSM317685     1  0.3900     0.7333 0.816 0.000 0.020 0.164
#&gt; GSM317694     1  0.0657     0.8512 0.984 0.004 0.012 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-3-a').click(function(){
  $('#tab-CV-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-4'>
<p><a id='tab-CV-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     3  0.0566     0.7881 0.012 0.004 0.984 0.000 0.000
#&gt; GSM317652     1  0.8110     0.0366 0.424 0.236 0.092 0.240 0.008
#&gt; GSM317666     4  0.0613     0.7062 0.000 0.004 0.004 0.984 0.008
#&gt; GSM317672     3  0.7187     0.0973 0.000 0.340 0.412 0.224 0.024
#&gt; GSM317679     3  0.0404     0.7879 0.012 0.000 0.988 0.000 0.000
#&gt; GSM317681     3  0.3724     0.6424 0.000 0.184 0.788 0.028 0.000
#&gt; GSM317682     1  0.2965     0.8119 0.876 0.028 0.084 0.000 0.012
#&gt; GSM317683     2  0.5574     0.3846 0.000 0.512 0.000 0.072 0.416
#&gt; GSM317689     2  0.5277     0.4131 0.000 0.524 0.008 0.032 0.436
#&gt; GSM317691     1  0.2376     0.8392 0.916 0.024 0.004 0.044 0.012
#&gt; GSM317692     2  0.8664     0.1078 0.232 0.364 0.024 0.112 0.268
#&gt; GSM317693     1  0.0727     0.8637 0.980 0.004 0.000 0.012 0.004
#&gt; GSM317696     1  0.0324     0.8649 0.992 0.004 0.004 0.000 0.000
#&gt; GSM317697     1  0.0854     0.8638 0.976 0.012 0.000 0.004 0.008
#&gt; GSM317698     1  0.0404     0.8636 0.988 0.000 0.000 0.000 0.012
#&gt; GSM317650     2  0.4219     0.4345 0.000 0.584 0.000 0.000 0.416
#&gt; GSM317651     2  0.7722    -0.2331 0.076 0.412 0.392 0.096 0.024
#&gt; GSM317657     4  0.6168    -0.0519 0.044 0.048 0.000 0.512 0.396
#&gt; GSM317667     4  0.0451     0.7049 0.000 0.008 0.000 0.988 0.004
#&gt; GSM317670     5  0.4888     0.5823 0.220 0.004 0.008 0.052 0.716
#&gt; GSM317674     1  0.0162     0.8643 0.996 0.000 0.000 0.000 0.004
#&gt; GSM317675     1  0.0290     0.8642 0.992 0.000 0.000 0.000 0.008
#&gt; GSM317677     1  0.1952     0.8231 0.912 0.000 0.000 0.004 0.084
#&gt; GSM317678     2  0.7229     0.4204 0.000 0.520 0.196 0.060 0.224
#&gt; GSM317687     4  0.4220     0.3518 0.300 0.004 0.000 0.688 0.008
#&gt; GSM317695     3  0.2361     0.6889 0.096 0.000 0.892 0.000 0.012
#&gt; GSM317653     4  0.4283     0.4370 0.000 0.292 0.012 0.692 0.004
#&gt; GSM317656     1  0.3699     0.7576 0.824 0.128 0.036 0.000 0.012
#&gt; GSM317658     1  0.4082     0.7365 0.812 0.040 0.004 0.020 0.124
#&gt; GSM317660     2  0.6501    -0.0294 0.000 0.564 0.288 0.112 0.036
#&gt; GSM317663     4  0.2184     0.6946 0.000 0.020 0.028 0.924 0.028
#&gt; GSM317664     1  0.2573     0.8065 0.880 0.000 0.104 0.000 0.016
#&gt; GSM317665     3  0.5166     0.2788 0.000 0.436 0.528 0.032 0.004
#&gt; GSM317673     1  0.1205     0.8575 0.956 0.000 0.040 0.000 0.004
#&gt; GSM317686     4  0.1928     0.6719 0.004 0.004 0.000 0.920 0.072
#&gt; GSM317688     1  0.0566     0.8660 0.984 0.004 0.000 0.000 0.012
#&gt; GSM317690     5  0.3477     0.4349 0.000 0.056 0.000 0.112 0.832
#&gt; GSM317654     2  0.6108    -0.2738 0.000 0.456 0.432 0.108 0.004
#&gt; GSM317655     5  0.4634     0.5404 0.036 0.064 0.004 0.108 0.788
#&gt; GSM317659     1  0.5840     0.3238 0.624 0.012 0.000 0.112 0.252
#&gt; GSM317661     2  0.5657     0.4300 0.000 0.664 0.024 0.088 0.224
#&gt; GSM317662     2  0.5192     0.4632 0.000 0.668 0.012 0.056 0.264
#&gt; GSM317668     5  0.5342     0.3506 0.412 0.000 0.012 0.032 0.544
#&gt; GSM317669     3  0.0566     0.7840 0.004 0.012 0.984 0.000 0.000
#&gt; GSM317671     3  0.0566     0.7864 0.012 0.000 0.984 0.000 0.004
#&gt; GSM317676     5  0.6024     0.1621 0.088 0.008 0.000 0.432 0.472
#&gt; GSM317680     3  0.0290     0.7878 0.008 0.000 0.992 0.000 0.000
#&gt; GSM317684     1  0.1983     0.8435 0.924 0.008 0.000 0.060 0.008
#&gt; GSM317685     1  0.3340     0.7903 0.856 0.044 0.012 0.088 0.000
#&gt; GSM317694     1  0.0290     0.8645 0.992 0.000 0.008 0.000 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-4-a').click(function(){
  $('#tab-CV-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-5'>
<p><a id='tab-CV-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM317649     3  0.0748     0.9089 0.004 0.000 0.976 0.004 0.016 0.000
#&gt; GSM317652     5  0.6710     0.3170 0.300 0.004 0.032 0.148 0.496 0.020
#&gt; GSM317666     4  0.0806     0.7937 0.000 0.000 0.000 0.972 0.008 0.020
#&gt; GSM317672     5  0.7522     0.2206 0.004 0.248 0.224 0.108 0.408 0.008
#&gt; GSM317679     3  0.0146     0.9133 0.000 0.004 0.996 0.000 0.000 0.000
#&gt; GSM317681     3  0.5082     0.4465 0.000 0.080 0.656 0.024 0.240 0.000
#&gt; GSM317682     1  0.3261     0.8109 0.856 0.068 0.036 0.000 0.032 0.008
#&gt; GSM317683     2  0.1151     0.6725 0.000 0.956 0.000 0.012 0.000 0.032
#&gt; GSM317689     2  0.3841     0.6598 0.000 0.812 0.020 0.008 0.068 0.092
#&gt; GSM317691     1  0.2838     0.7969 0.852 0.116 0.004 0.028 0.000 0.000
#&gt; GSM317692     2  0.6308     0.4531 0.184 0.628 0.012 0.052 0.100 0.024
#&gt; GSM317693     1  0.0582     0.8655 0.984 0.004 0.000 0.004 0.004 0.004
#&gt; GSM317696     1  0.0146     0.8645 0.996 0.000 0.004 0.000 0.000 0.000
#&gt; GSM317697     1  0.0436     0.8653 0.988 0.000 0.004 0.004 0.000 0.004
#&gt; GSM317698     1  0.0713     0.8645 0.972 0.000 0.000 0.000 0.000 0.028
#&gt; GSM317650     2  0.5596     0.4984 0.000 0.596 0.000 0.024 0.260 0.120
#&gt; GSM317651     5  0.6029     0.5218 0.040 0.000 0.172 0.088 0.648 0.052
#&gt; GSM317657     4  0.5456     0.1379 0.000 0.128 0.000 0.500 0.000 0.372
#&gt; GSM317667     4  0.0993     0.7919 0.000 0.000 0.000 0.964 0.012 0.024
#&gt; GSM317670     6  0.1781     0.8124 0.060 0.008 0.008 0.000 0.000 0.924
#&gt; GSM317674     1  0.1138     0.8642 0.960 0.000 0.012 0.000 0.004 0.024
#&gt; GSM317675     1  0.0777     0.8650 0.972 0.000 0.004 0.000 0.000 0.024
#&gt; GSM317677     1  0.2631     0.7596 0.820 0.000 0.000 0.000 0.000 0.180
#&gt; GSM317678     2  0.2964     0.6351 0.000 0.856 0.100 0.024 0.020 0.000
#&gt; GSM317687     4  0.3261     0.5975 0.192 0.008 0.000 0.792 0.004 0.004
#&gt; GSM317695     3  0.0777     0.8922 0.024 0.000 0.972 0.000 0.000 0.004
#&gt; GSM317653     5  0.4312     0.1768 0.000 0.012 0.000 0.476 0.508 0.004
#&gt; GSM317656     1  0.4651     0.6559 0.716 0.008 0.016 0.008 0.216 0.036
#&gt; GSM317658     1  0.4180     0.5902 0.692 0.276 0.004 0.020 0.000 0.008
#&gt; GSM317660     5  0.2395     0.5258 0.000 0.016 0.036 0.016 0.908 0.024
#&gt; GSM317663     4  0.2485     0.7754 0.000 0.040 0.032 0.900 0.004 0.024
#&gt; GSM317664     1  0.3134     0.7739 0.808 0.000 0.168 0.000 0.000 0.024
#&gt; GSM317665     5  0.3613     0.5579 0.000 0.004 0.192 0.024 0.776 0.004
#&gt; GSM317673     1  0.2015     0.8536 0.916 0.000 0.056 0.000 0.012 0.016
#&gt; GSM317686     4  0.1549     0.7949 0.000 0.020 0.000 0.936 0.000 0.044
#&gt; GSM317688     1  0.1265     0.8630 0.948 0.000 0.008 0.000 0.000 0.044
#&gt; GSM317690     6  0.2759     0.7619 0.000 0.080 0.004 0.040 0.004 0.872
#&gt; GSM317654     5  0.3809     0.5654 0.000 0.016 0.140 0.044 0.796 0.004
#&gt; GSM317655     6  0.1442     0.8080 0.000 0.004 0.000 0.040 0.012 0.944
#&gt; GSM317659     1  0.6187    -0.0535 0.448 0.000 0.000 0.088 0.060 0.404
#&gt; GSM317661     5  0.6100    -0.1754 0.000 0.384 0.004 0.036 0.476 0.100
#&gt; GSM317662     2  0.5420     0.3267 0.000 0.568 0.004 0.020 0.340 0.068
#&gt; GSM317668     6  0.3020     0.7078 0.176 0.000 0.004 0.004 0.004 0.812
#&gt; GSM317669     3  0.0547     0.9111 0.000 0.000 0.980 0.000 0.020 0.000
#&gt; GSM317671     3  0.0291     0.9141 0.000 0.000 0.992 0.000 0.004 0.004
#&gt; GSM317676     6  0.3838     0.6496 0.020 0.000 0.000 0.240 0.008 0.732
#&gt; GSM317680     3  0.0363     0.9127 0.000 0.000 0.988 0.000 0.012 0.000
#&gt; GSM317684     1  0.1057     0.8634 0.968 0.004 0.004 0.012 0.008 0.004
#&gt; GSM317685     1  0.3723     0.7869 0.824 0.008 0.008 0.076 0.076 0.008
#&gt; GSM317694     1  0.1124     0.8641 0.956 0.000 0.036 0.000 0.000 0.008
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-5-a').click(function(){
  $('#tab-CV-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-NMF-signature_compare](figure_cola/CV-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-CV-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-NMF-collect-classes](figure_cola/CV-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n disease.state(p) k
#> CV:NMF 45            0.805 2
#> CV:NMF 44            0.966 3
#> CV:NMF 44            0.880 4
#> CV:NMF 30            0.477 5
#> CV:NMF 40            0.380 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:hclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "hclust"]
# you can also extract it by
# res = res_list["MAD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-hclust-collect-plots](figure_cola/MAD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-hclust-select-partition-number](figure_cola/MAD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.759           0.900       0.949         0.3101 0.726   0.726
#> 3 3 0.779           0.886       0.949         0.0567 0.990   0.987
#> 4 4 0.654           0.840       0.899         0.9094 0.647   0.507
#> 5 5 0.525           0.676       0.807         0.1252 0.949   0.858
#> 6 6 0.584           0.693       0.758         0.0956 0.907   0.706
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-classes'>
<ul>
<li><a href='#tab-MAD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-hclust-get-classes-1'>
<p><a id='tab-MAD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     1  0.0000      0.946 1.000 0.000
#&gt; GSM317652     1  0.0000      0.946 1.000 0.000
#&gt; GSM317666     1  0.6148      0.844 0.848 0.152
#&gt; GSM317672     1  0.7299      0.775 0.796 0.204
#&gt; GSM317679     1  0.0000      0.946 1.000 0.000
#&gt; GSM317681     1  0.1184      0.942 0.984 0.016
#&gt; GSM317682     1  0.0000      0.946 1.000 0.000
#&gt; GSM317683     2  0.0000      0.939 0.000 1.000
#&gt; GSM317689     1  0.9635      0.435 0.612 0.388
#&gt; GSM317691     1  0.1414      0.940 0.980 0.020
#&gt; GSM317692     1  0.3879      0.906 0.924 0.076
#&gt; GSM317693     1  0.0938      0.943 0.988 0.012
#&gt; GSM317696     1  0.0000      0.946 1.000 0.000
#&gt; GSM317697     1  0.0376      0.945 0.996 0.004
#&gt; GSM317698     1  0.0000      0.946 1.000 0.000
#&gt; GSM317650     2  0.0000      0.939 0.000 1.000
#&gt; GSM317651     1  0.0000      0.946 1.000 0.000
#&gt; GSM317657     1  0.7139      0.795 0.804 0.196
#&gt; GSM317667     2  0.0000      0.939 0.000 1.000
#&gt; GSM317670     1  0.7299      0.785 0.796 0.204
#&gt; GSM317674     1  0.0000      0.946 1.000 0.000
#&gt; GSM317675     1  0.0000      0.946 1.000 0.000
#&gt; GSM317677     1  0.3114      0.923 0.944 0.056
#&gt; GSM317678     2  0.3584      0.887 0.068 0.932
#&gt; GSM317687     1  0.2778      0.927 0.952 0.048
#&gt; GSM317695     1  0.0000      0.946 1.000 0.000
#&gt; GSM317653     1  0.0938      0.943 0.988 0.012
#&gt; GSM317656     1  0.0000      0.946 1.000 0.000
#&gt; GSM317658     1  0.6148      0.840 0.848 0.152
#&gt; GSM317660     1  0.0672      0.943 0.992 0.008
#&gt; GSM317663     1  0.7139      0.795 0.804 0.196
#&gt; GSM317664     1  0.0000      0.946 1.000 0.000
#&gt; GSM317665     1  0.0000      0.946 1.000 0.000
#&gt; GSM317673     1  0.0000      0.946 1.000 0.000
#&gt; GSM317686     2  0.0000      0.939 0.000 1.000
#&gt; GSM317688     1  0.0000      0.946 1.000 0.000
#&gt; GSM317690     2  0.8955      0.484 0.312 0.688
#&gt; GSM317654     1  0.0000      0.946 1.000 0.000
#&gt; GSM317655     1  0.8081      0.723 0.752 0.248
#&gt; GSM317659     1  0.3114      0.923 0.944 0.056
#&gt; GSM317661     2  0.0000      0.939 0.000 1.000
#&gt; GSM317662     2  0.0000      0.939 0.000 1.000
#&gt; GSM317668     1  0.0376      0.945 0.996 0.004
#&gt; GSM317669     1  0.0000      0.946 1.000 0.000
#&gt; GSM317671     1  0.0000      0.946 1.000 0.000
#&gt; GSM317676     1  0.3274      0.920 0.940 0.060
#&gt; GSM317680     1  0.0000      0.946 1.000 0.000
#&gt; GSM317684     1  0.2236      0.933 0.964 0.036
#&gt; GSM317685     1  0.0000      0.946 1.000 0.000
#&gt; GSM317694     1  0.1633      0.938 0.976 0.024
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-1-a').click(function(){
  $('#tab-MAD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-2'>
<p><a id='tab-MAD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM317649     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM317652     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM317666     1  0.4047      0.850 0.848 0.004 0.148
#&gt; GSM317672     1  0.4784      0.763 0.796 0.200 0.004
#&gt; GSM317679     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM317681     1  0.0829      0.943 0.984 0.012 0.004
#&gt; GSM317682     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM317683     2  0.0000      0.810 0.000 1.000 0.000
#&gt; GSM317689     1  0.6566      0.411 0.612 0.376 0.012
#&gt; GSM317691     1  0.1015      0.941 0.980 0.012 0.008
#&gt; GSM317692     1  0.2680      0.903 0.924 0.068 0.008
#&gt; GSM317693     1  0.0661      0.944 0.988 0.004 0.008
#&gt; GSM317696     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM317697     1  0.0237      0.946 0.996 0.004 0.000
#&gt; GSM317698     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM317650     2  0.0000      0.810 0.000 1.000 0.000
#&gt; GSM317651     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM317657     1  0.5710      0.800 0.804 0.080 0.116
#&gt; GSM317667     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM317670     1  0.4784      0.773 0.796 0.200 0.004
#&gt; GSM317674     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM317675     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM317677     1  0.1964      0.924 0.944 0.000 0.056
#&gt; GSM317678     2  0.2261      0.740 0.068 0.932 0.000
#&gt; GSM317687     1  0.1753      0.928 0.952 0.000 0.048
#&gt; GSM317695     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM317653     1  0.0661      0.944 0.988 0.008 0.004
#&gt; GSM317656     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM317658     1  0.4047      0.832 0.848 0.148 0.004
#&gt; GSM317660     1  0.0424      0.944 0.992 0.008 0.000
#&gt; GSM317663     1  0.5710      0.800 0.804 0.080 0.116
#&gt; GSM317664     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM317665     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM317673     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM317686     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM317688     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM317690     2  0.5873      0.350 0.312 0.684 0.004
#&gt; GSM317654     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM317655     1  0.6208      0.721 0.752 0.200 0.048
#&gt; GSM317659     1  0.1964      0.924 0.944 0.000 0.056
#&gt; GSM317661     2  0.0000      0.810 0.000 1.000 0.000
#&gt; GSM317662     2  0.0000      0.810 0.000 1.000 0.000
#&gt; GSM317668     1  0.0237      0.946 0.996 0.004 0.000
#&gt; GSM317669     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM317671     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM317676     1  0.2066      0.922 0.940 0.000 0.060
#&gt; GSM317680     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM317684     1  0.1411      0.934 0.964 0.000 0.036
#&gt; GSM317685     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM317694     1  0.1031      0.939 0.976 0.000 0.024
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-2-a').click(function(){
  $('#tab-MAD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-3'>
<p><a id='tab-MAD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     3  0.0469      0.956 0.012 0.000 0.988 0.000
#&gt; GSM317652     3  0.0469      0.956 0.012 0.000 0.988 0.000
#&gt; GSM317666     1  0.2081      0.739 0.916 0.000 0.000 0.084
#&gt; GSM317672     1  0.6049      0.693 0.680 0.200 0.120 0.000
#&gt; GSM317679     3  0.0376      0.952 0.004 0.000 0.992 0.004
#&gt; GSM317681     3  0.1970      0.933 0.060 0.008 0.932 0.000
#&gt; GSM317682     3  0.1211      0.954 0.040 0.000 0.960 0.000
#&gt; GSM317683     2  0.0000      0.869 0.000 1.000 0.000 0.000
#&gt; GSM317689     1  0.5673      0.444 0.596 0.372 0.032 0.000
#&gt; GSM317691     1  0.3978      0.758 0.796 0.012 0.192 0.000
#&gt; GSM317692     1  0.5325      0.733 0.728 0.068 0.204 0.000
#&gt; GSM317693     1  0.4560      0.663 0.700 0.004 0.296 0.000
#&gt; GSM317696     3  0.1389      0.951 0.048 0.000 0.952 0.000
#&gt; GSM317697     3  0.4401      0.605 0.272 0.004 0.724 0.000
#&gt; GSM317698     3  0.1557      0.947 0.056 0.000 0.944 0.000
#&gt; GSM317650     2  0.0000      0.869 0.000 1.000 0.000 0.000
#&gt; GSM317651     3  0.0469      0.956 0.012 0.000 0.988 0.000
#&gt; GSM317657     1  0.4036      0.725 0.836 0.076 0.000 0.088
#&gt; GSM317667     4  0.0188      1.000 0.004 0.000 0.000 0.996
#&gt; GSM317670     1  0.4630      0.709 0.768 0.196 0.036 0.000
#&gt; GSM317674     3  0.1557      0.947 0.056 0.000 0.944 0.000
#&gt; GSM317675     3  0.1557      0.947 0.056 0.000 0.944 0.000
#&gt; GSM317677     1  0.3123      0.750 0.844 0.000 0.156 0.000
#&gt; GSM317678     2  0.2111      0.810 0.024 0.932 0.044 0.000
#&gt; GSM317687     1  0.1716      0.783 0.936 0.000 0.064 0.000
#&gt; GSM317695     3  0.0376      0.952 0.004 0.000 0.992 0.004
#&gt; GSM317653     3  0.1743      0.936 0.056 0.004 0.940 0.000
#&gt; GSM317656     3  0.0707      0.957 0.020 0.000 0.980 0.000
#&gt; GSM317658     1  0.5950      0.723 0.696 0.148 0.156 0.000
#&gt; GSM317660     3  0.1305      0.947 0.036 0.004 0.960 0.000
#&gt; GSM317663     1  0.4036      0.725 0.836 0.076 0.000 0.088
#&gt; GSM317664     3  0.1557      0.947 0.056 0.000 0.944 0.000
#&gt; GSM317665     3  0.1118      0.948 0.036 0.000 0.964 0.000
#&gt; GSM317673     3  0.1211      0.954 0.040 0.000 0.960 0.000
#&gt; GSM317686     4  0.0188      1.000 0.004 0.000 0.000 0.996
#&gt; GSM317688     3  0.1557      0.947 0.056 0.000 0.944 0.000
#&gt; GSM317690     2  0.4500      0.403 0.316 0.684 0.000 0.000
#&gt; GSM317654     3  0.1118      0.948 0.036 0.000 0.964 0.000
#&gt; GSM317655     1  0.4406      0.674 0.780 0.192 0.000 0.028
#&gt; GSM317659     1  0.1211      0.776 0.960 0.000 0.040 0.000
#&gt; GSM317661     2  0.0188      0.867 0.004 0.996 0.000 0.000
#&gt; GSM317662     2  0.0000      0.869 0.000 1.000 0.000 0.000
#&gt; GSM317668     1  0.3907      0.750 0.768 0.000 0.232 0.000
#&gt; GSM317669     3  0.0376      0.952 0.004 0.000 0.992 0.004
#&gt; GSM317671     3  0.0376      0.952 0.004 0.000 0.992 0.004
#&gt; GSM317676     1  0.1022      0.773 0.968 0.000 0.032 0.000
#&gt; GSM317680     3  0.0376      0.952 0.004 0.000 0.992 0.004
#&gt; GSM317684     1  0.2589      0.781 0.884 0.000 0.116 0.000
#&gt; GSM317685     3  0.1211      0.954 0.040 0.000 0.960 0.000
#&gt; GSM317694     1  0.4072      0.686 0.748 0.000 0.252 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-3-a').click(function(){
  $('#tab-MAD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-4'>
<p><a id='tab-MAD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     1   0.179      0.740 0.916 0.000 0.000 0.000 0.084
#&gt; GSM317652     1   0.218      0.729 0.888 0.000 0.000 0.000 0.112
#&gt; GSM317666     4   0.201      0.712 0.000 0.000 0.088 0.908 0.004
#&gt; GSM317672     4   0.703      0.610 0.096 0.188 0.000 0.576 0.140
#&gt; GSM317679     1   0.293      0.676 0.820 0.000 0.000 0.000 0.180
#&gt; GSM317681     5   0.475      0.923 0.352 0.000 0.000 0.028 0.620
#&gt; GSM317682     1   0.239      0.704 0.896 0.000 0.000 0.020 0.084
#&gt; GSM317683     2   0.000      0.841 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317689     4   0.630      0.452 0.008 0.292 0.000 0.548 0.152
#&gt; GSM317691     4   0.516      0.709 0.168 0.000 0.000 0.692 0.140
#&gt; GSM317692     4   0.650      0.675 0.176 0.056 0.000 0.620 0.148
#&gt; GSM317693     4   0.578      0.590 0.272 0.000 0.000 0.596 0.132
#&gt; GSM317696     1   0.140      0.749 0.952 0.000 0.000 0.028 0.020
#&gt; GSM317697     1   0.493      0.353 0.708 0.000 0.000 0.188 0.104
#&gt; GSM317698     1   0.158      0.747 0.944 0.000 0.000 0.028 0.028
#&gt; GSM317650     2   0.000      0.841 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317651     1   0.323      0.518 0.800 0.000 0.000 0.004 0.196
#&gt; GSM317657     4   0.406      0.709 0.000 0.032 0.092 0.820 0.056
#&gt; GSM317667     3   0.000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317670     4   0.536      0.667 0.004 0.100 0.000 0.664 0.232
#&gt; GSM317674     1   0.158      0.747 0.944 0.000 0.000 0.028 0.028
#&gt; GSM317675     1   0.158      0.747 0.944 0.000 0.000 0.028 0.028
#&gt; GSM317677     4   0.340      0.699 0.168 0.000 0.000 0.812 0.020
#&gt; GSM317678     2   0.210      0.806 0.012 0.924 0.000 0.016 0.048
#&gt; GSM317687     4   0.188      0.746 0.064 0.000 0.000 0.924 0.012
#&gt; GSM317695     1   0.293      0.676 0.820 0.000 0.000 0.000 0.180
#&gt; GSM317653     5   0.470      0.928 0.360 0.000 0.000 0.024 0.616
#&gt; GSM317656     1   0.191      0.741 0.908 0.000 0.000 0.000 0.092
#&gt; GSM317658     4   0.698      0.652 0.128 0.136 0.000 0.592 0.144
#&gt; GSM317660     5   0.437      0.862 0.416 0.000 0.000 0.004 0.580
#&gt; GSM317663     4   0.406      0.709 0.000 0.032 0.092 0.820 0.056
#&gt; GSM317664     1   0.158      0.747 0.944 0.000 0.000 0.028 0.028
#&gt; GSM317665     1   0.468     -0.291 0.600 0.000 0.000 0.020 0.380
#&gt; GSM317673     1   0.157      0.747 0.944 0.000 0.000 0.020 0.036
#&gt; GSM317686     3   0.000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317688     1   0.183      0.742 0.932 0.000 0.000 0.028 0.040
#&gt; GSM317690     2   0.599      0.262 0.000 0.556 0.000 0.304 0.140
#&gt; GSM317654     1   0.468     -0.291 0.600 0.000 0.000 0.020 0.380
#&gt; GSM317655     4   0.462      0.684 0.000 0.052 0.028 0.768 0.152
#&gt; GSM317659     4   0.144      0.739 0.040 0.000 0.000 0.948 0.012
#&gt; GSM317661     2   0.189      0.811 0.000 0.916 0.000 0.004 0.080
#&gt; GSM317662     2   0.000      0.841 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317668     4   0.548      0.688 0.160 0.000 0.000 0.656 0.184
#&gt; GSM317669     1   0.293      0.676 0.820 0.000 0.000 0.000 0.180
#&gt; GSM317671     1   0.293      0.676 0.820 0.000 0.000 0.000 0.180
#&gt; GSM317676     4   0.128      0.737 0.032 0.000 0.000 0.956 0.012
#&gt; GSM317680     1   0.293      0.676 0.820 0.000 0.000 0.000 0.180
#&gt; GSM317684     4   0.256      0.737 0.120 0.000 0.000 0.872 0.008
#&gt; GSM317685     1   0.131      0.747 0.956 0.000 0.000 0.020 0.024
#&gt; GSM317694     4   0.407      0.623 0.264 0.000 0.000 0.720 0.016
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-4-a').click(function(){
  $('#tab-MAD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-5'>
<p><a id='tab-MAD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM317649     1  0.4282      0.171 0.560 0.000 0.420 0.000 0.020 0.000
#&gt; GSM317652     1  0.4344      0.396 0.628 0.000 0.336 0.000 0.036 0.000
#&gt; GSM317666     4  0.1556      0.690 0.000 0.000 0.000 0.920 0.000 0.080
#&gt; GSM317672     4  0.7662      0.563 0.108 0.188 0.080 0.492 0.132 0.000
#&gt; GSM317679     3  0.2378      1.000 0.152 0.000 0.848 0.000 0.000 0.000
#&gt; GSM317681     5  0.4075      0.726 0.168 0.000 0.060 0.012 0.760 0.000
#&gt; GSM317682     1  0.2301      0.675 0.884 0.000 0.020 0.000 0.096 0.000
#&gt; GSM317683     2  0.0000      0.834 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317689     4  0.6579      0.439 0.004 0.260 0.048 0.500 0.188 0.000
#&gt; GSM317691     4  0.5889      0.680 0.208 0.000 0.068 0.612 0.112 0.000
#&gt; GSM317692     4  0.7179      0.638 0.208 0.056 0.084 0.532 0.120 0.000
#&gt; GSM317693     4  0.6268      0.586 0.300 0.000 0.068 0.524 0.108 0.000
#&gt; GSM317696     1  0.0725      0.757 0.976 0.000 0.012 0.000 0.012 0.000
#&gt; GSM317697     1  0.4747      0.437 0.732 0.000 0.052 0.144 0.072 0.000
#&gt; GSM317698     1  0.0458      0.760 0.984 0.000 0.016 0.000 0.000 0.000
#&gt; GSM317650     2  0.0000      0.834 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317651     1  0.4233      0.213 0.684 0.000 0.048 0.000 0.268 0.000
#&gt; GSM317657     4  0.3948      0.685 0.000 0.004 0.016 0.796 0.104 0.080
#&gt; GSM317667     6  0.0000      1.000 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM317670     4  0.6355      0.642 0.032 0.060 0.108 0.608 0.192 0.000
#&gt; GSM317674     1  0.0458      0.760 0.984 0.000 0.016 0.000 0.000 0.000
#&gt; GSM317675     1  0.0458      0.760 0.984 0.000 0.016 0.000 0.000 0.000
#&gt; GSM317677     4  0.3309      0.677 0.192 0.000 0.016 0.788 0.004 0.000
#&gt; GSM317678     2  0.1929      0.799 0.004 0.924 0.016 0.008 0.048 0.000
#&gt; GSM317687     4  0.1908      0.722 0.096 0.000 0.000 0.900 0.004 0.000
#&gt; GSM317695     3  0.2378      1.000 0.152 0.000 0.848 0.000 0.000 0.000
#&gt; GSM317653     5  0.4140      0.734 0.172 0.000 0.056 0.016 0.756 0.000
#&gt; GSM317656     1  0.4045      0.453 0.664 0.000 0.312 0.000 0.024 0.000
#&gt; GSM317658     4  0.7626      0.609 0.160 0.136 0.080 0.504 0.120 0.000
#&gt; GSM317660     5  0.4494      0.748 0.216 0.000 0.092 0.000 0.692 0.000
#&gt; GSM317663     4  0.3857      0.685 0.000 0.004 0.012 0.800 0.104 0.080
#&gt; GSM317664     1  0.0458      0.760 0.984 0.000 0.016 0.000 0.000 0.000
#&gt; GSM317665     5  0.5498      0.544 0.436 0.000 0.088 0.012 0.464 0.000
#&gt; GSM317673     1  0.0935      0.743 0.964 0.000 0.004 0.000 0.032 0.000
#&gt; GSM317686     6  0.0000      1.000 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM317688     1  0.0935      0.750 0.964 0.000 0.032 0.000 0.004 0.000
#&gt; GSM317690     2  0.6283      0.202 0.000 0.512 0.052 0.304 0.132 0.000
#&gt; GSM317654     5  0.5498      0.544 0.436 0.000 0.088 0.012 0.464 0.000
#&gt; GSM317655     4  0.4333      0.665 0.000 0.008 0.048 0.748 0.180 0.016
#&gt; GSM317659     4  0.1588      0.717 0.072 0.000 0.000 0.924 0.004 0.000
#&gt; GSM317661     2  0.2279      0.796 0.000 0.904 0.024 0.016 0.056 0.000
#&gt; GSM317662     2  0.0000      0.834 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317668     4  0.6115      0.640 0.096 0.000 0.228 0.584 0.092 0.000
#&gt; GSM317669     3  0.2378      1.000 0.152 0.000 0.848 0.000 0.000 0.000
#&gt; GSM317671     3  0.2378      1.000 0.152 0.000 0.848 0.000 0.000 0.000
#&gt; GSM317676     4  0.1471      0.714 0.064 0.000 0.000 0.932 0.004 0.000
#&gt; GSM317680     3  0.2378      1.000 0.152 0.000 0.848 0.000 0.000 0.000
#&gt; GSM317684     4  0.2520      0.710 0.152 0.000 0.000 0.844 0.004 0.000
#&gt; GSM317685     1  0.1297      0.734 0.948 0.000 0.012 0.000 0.040 0.000
#&gt; GSM317694     4  0.3915      0.620 0.288 0.000 0.016 0.692 0.004 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-5-a').click(function(){
  $('#tab-MAD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-hclust-signature_compare](figure_cola/MAD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-hclust-collect-classes](figure_cola/MAD-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> MAD:hclust 48            0.528 2
#> MAD:hclust 48            0.539 3
#> MAD:hclust 48            0.584 4
#> MAD:hclust 45            0.552 5
#> MAD:hclust 43            0.456 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "kmeans"]
# you can also extract it by
# res = res_list["MAD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-kmeans-collect-plots](figure_cola/MAD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-kmeans-select-partition-number](figure_cola/MAD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.944       0.977         0.4578 0.542   0.542
#> 3 3 0.520           0.615       0.723         0.3647 0.791   0.620
#> 4 4 0.608           0.739       0.842         0.1454 0.828   0.564
#> 5 5 0.683           0.709       0.823         0.0691 0.892   0.654
#> 6 6 0.692           0.578       0.759         0.0528 0.952   0.814
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-kmeans-get-classes-1'>
<p><a id='tab-MAD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     1  0.0672      0.981 0.992 0.008
#&gt; GSM317652     1  0.0672      0.981 0.992 0.008
#&gt; GSM317666     2  0.0938      0.964 0.012 0.988
#&gt; GSM317672     2  0.0000      0.964 0.000 1.000
#&gt; GSM317679     1  0.0672      0.981 0.992 0.008
#&gt; GSM317681     2  0.0672      0.962 0.008 0.992
#&gt; GSM317682     1  0.0938      0.980 0.988 0.012
#&gt; GSM317683     2  0.0000      0.964 0.000 1.000
#&gt; GSM317689     2  0.0376      0.964 0.004 0.996
#&gt; GSM317691     1  0.0000      0.982 1.000 0.000
#&gt; GSM317692     1  0.1414      0.969 0.980 0.020
#&gt; GSM317693     1  0.0376      0.982 0.996 0.004
#&gt; GSM317696     1  0.0376      0.982 0.996 0.004
#&gt; GSM317697     1  0.0376      0.982 0.996 0.004
#&gt; GSM317698     1  0.0376      0.982 0.996 0.004
#&gt; GSM317650     2  0.0000      0.964 0.000 1.000
#&gt; GSM317651     1  0.0376      0.982 0.996 0.004
#&gt; GSM317657     2  0.0938      0.964 0.012 0.988
#&gt; GSM317667     2  0.0938      0.964 0.012 0.988
#&gt; GSM317670     2  0.0938      0.962 0.012 0.988
#&gt; GSM317674     1  0.0376      0.982 0.996 0.004
#&gt; GSM317675     1  0.0376      0.982 0.996 0.004
#&gt; GSM317677     1  0.0000      0.982 1.000 0.000
#&gt; GSM317678     2  0.0000      0.964 0.000 1.000
#&gt; GSM317687     1  0.0000      0.982 1.000 0.000
#&gt; GSM317695     1  0.0000      0.982 1.000 0.000
#&gt; GSM317653     1  0.9710      0.314 0.600 0.400
#&gt; GSM317656     1  0.0938      0.980 0.988 0.012
#&gt; GSM317658     2  0.9993      0.057 0.484 0.516
#&gt; GSM317660     1  0.0938      0.980 0.988 0.012
#&gt; GSM317663     2  0.0938      0.964 0.012 0.988
#&gt; GSM317664     1  0.0376      0.982 0.996 0.004
#&gt; GSM317665     1  0.0672      0.981 0.992 0.008
#&gt; GSM317673     1  0.0376      0.982 0.996 0.004
#&gt; GSM317686     2  0.0938      0.964 0.012 0.988
#&gt; GSM317688     1  0.0938      0.980 0.988 0.012
#&gt; GSM317690     2  0.0672      0.963 0.008 0.992
#&gt; GSM317654     1  0.0672      0.981 0.992 0.008
#&gt; GSM317655     2  0.0938      0.964 0.012 0.988
#&gt; GSM317659     1  0.0000      0.982 1.000 0.000
#&gt; GSM317661     2  0.0000      0.964 0.000 1.000
#&gt; GSM317662     2  0.0000      0.964 0.000 1.000
#&gt; GSM317668     1  0.0376      0.982 0.996 0.004
#&gt; GSM317669     1  0.0672      0.981 0.992 0.008
#&gt; GSM317671     1  0.0672      0.981 0.992 0.008
#&gt; GSM317676     1  0.0000      0.982 1.000 0.000
#&gt; GSM317680     1  0.0672      0.981 0.992 0.008
#&gt; GSM317684     1  0.0000      0.982 1.000 0.000
#&gt; GSM317685     1  0.0000      0.982 1.000 0.000
#&gt; GSM317694     1  0.0000      0.982 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-2'>
<p><a id='tab-MAD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM317649     3   0.661     0.7952 0.432 0.008 0.560
#&gt; GSM317652     3   0.630     0.7257 0.484 0.000 0.516
#&gt; GSM317666     3   0.903    -0.6446 0.132 0.428 0.440
#&gt; GSM317672     2   0.277     0.8303 0.004 0.916 0.080
#&gt; GSM317679     3   0.660     0.7938 0.428 0.008 0.564
#&gt; GSM317681     2   0.337     0.8251 0.024 0.904 0.072
#&gt; GSM317682     1   0.312     0.6190 0.892 0.000 0.108
#&gt; GSM317683     2   0.000     0.8350 0.000 1.000 0.000
#&gt; GSM317689     2   0.226     0.8333 0.000 0.932 0.068
#&gt; GSM317691     1   0.424     0.6417 0.824 0.000 0.176
#&gt; GSM317692     1   0.465     0.6357 0.816 0.008 0.176
#&gt; GSM317693     1   0.394     0.6531 0.844 0.000 0.156
#&gt; GSM317696     1   0.296     0.6218 0.900 0.000 0.100
#&gt; GSM317697     1   0.236     0.6625 0.928 0.000 0.072
#&gt; GSM317698     1   0.000     0.6563 1.000 0.000 0.000
#&gt; GSM317650     2   0.000     0.8350 0.000 1.000 0.000
#&gt; GSM317651     1   0.334     0.6096 0.880 0.000 0.120
#&gt; GSM317657     2   0.742     0.7281 0.040 0.572 0.388
#&gt; GSM317667     2   0.615     0.7521 0.004 0.640 0.356
#&gt; GSM317670     2   0.517     0.8177 0.024 0.804 0.172
#&gt; GSM317674     1   0.296     0.6218 0.900 0.000 0.100
#&gt; GSM317675     1   0.288     0.6243 0.904 0.000 0.096
#&gt; GSM317677     1   0.388     0.6537 0.848 0.000 0.152
#&gt; GSM317678     2   0.000     0.8350 0.000 1.000 0.000
#&gt; GSM317687     1   0.514     0.5695 0.748 0.000 0.252
#&gt; GSM317695     3   0.624     0.7878 0.440 0.000 0.560
#&gt; GSM317653     1   0.971    -0.0412 0.420 0.356 0.224
#&gt; GSM317656     1   0.631    -0.7101 0.508 0.000 0.492
#&gt; GSM317658     2   0.834     0.0664 0.444 0.476 0.080
#&gt; GSM317660     3   0.806     0.7092 0.376 0.072 0.552
#&gt; GSM317663     2   0.630     0.7528 0.004 0.608 0.388
#&gt; GSM317664     1   0.296     0.6218 0.900 0.000 0.100
#&gt; GSM317665     3   0.663     0.7760 0.444 0.008 0.548
#&gt; GSM317673     1   0.304     0.6173 0.896 0.000 0.104
#&gt; GSM317686     2   0.543     0.7808 0.000 0.716 0.284
#&gt; GSM317688     1   0.304     0.6173 0.896 0.000 0.104
#&gt; GSM317690     2   0.245     0.8330 0.000 0.924 0.076
#&gt; GSM317654     3   0.666     0.7361 0.460 0.008 0.532
#&gt; GSM317655     2   0.630     0.7528 0.004 0.608 0.388
#&gt; GSM317659     1   0.406     0.6488 0.836 0.000 0.164
#&gt; GSM317661     2   0.000     0.8350 0.000 1.000 0.000
#&gt; GSM317662     2   0.000     0.8350 0.000 1.000 0.000
#&gt; GSM317668     1   0.571    -0.0905 0.680 0.000 0.320
#&gt; GSM317669     3   0.661     0.7952 0.432 0.008 0.560
#&gt; GSM317671     3   0.660     0.7938 0.428 0.008 0.564
#&gt; GSM317676     1   0.619     0.3872 0.580 0.000 0.420
#&gt; GSM317680     3   0.661     0.7952 0.432 0.008 0.560
#&gt; GSM317684     1   0.394     0.6531 0.844 0.000 0.156
#&gt; GSM317685     1   0.304     0.6173 0.896 0.000 0.104
#&gt; GSM317694     1   0.116     0.6604 0.972 0.000 0.028
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-3'>
<p><a id='tab-MAD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     3  0.0524     0.7986 0.008 0.000 0.988 0.004
#&gt; GSM317652     3  0.5889     0.6964 0.212 0.000 0.688 0.100
#&gt; GSM317666     4  0.4780     0.7102 0.096 0.116 0.000 0.788
#&gt; GSM317672     2  0.3632     0.7731 0.004 0.832 0.008 0.156
#&gt; GSM317679     3  0.0336     0.7981 0.008 0.000 0.992 0.000
#&gt; GSM317681     2  0.5020     0.6771 0.004 0.760 0.052 0.184
#&gt; GSM317682     1  0.4513     0.7767 0.804 0.000 0.076 0.120
#&gt; GSM317683     2  0.0000     0.8796 0.000 1.000 0.000 0.000
#&gt; GSM317689     2  0.1677     0.8536 0.012 0.948 0.000 0.040
#&gt; GSM317691     1  0.2011     0.8190 0.920 0.000 0.000 0.080
#&gt; GSM317692     1  0.2796     0.8021 0.892 0.008 0.004 0.096
#&gt; GSM317693     1  0.0921     0.8456 0.972 0.000 0.000 0.028
#&gt; GSM317696     1  0.2530     0.8556 0.896 0.000 0.100 0.004
#&gt; GSM317697     1  0.0657     0.8548 0.984 0.000 0.012 0.004
#&gt; GSM317698     1  0.1867     0.8617 0.928 0.000 0.072 0.000
#&gt; GSM317650     2  0.0000     0.8796 0.000 1.000 0.000 0.000
#&gt; GSM317651     1  0.5582     0.6961 0.724 0.000 0.108 0.168
#&gt; GSM317657     4  0.5496     0.7173 0.088 0.188 0.000 0.724
#&gt; GSM317667     4  0.4088     0.6597 0.000 0.232 0.004 0.764
#&gt; GSM317670     2  0.5766     0.5366 0.104 0.704 0.000 0.192
#&gt; GSM317674     1  0.2345     0.8559 0.900 0.000 0.100 0.000
#&gt; GSM317675     1  0.2281     0.8573 0.904 0.000 0.096 0.000
#&gt; GSM317677     1  0.1305     0.8436 0.960 0.000 0.004 0.036
#&gt; GSM317678     2  0.0000     0.8796 0.000 1.000 0.000 0.000
#&gt; GSM317687     1  0.3219     0.7298 0.836 0.000 0.000 0.164
#&gt; GSM317695     3  0.0707     0.7956 0.020 0.000 0.980 0.000
#&gt; GSM317653     4  0.8426     0.0995 0.376 0.148 0.052 0.424
#&gt; GSM317656     3  0.5337     0.3636 0.424 0.000 0.564 0.012
#&gt; GSM317658     1  0.5894     0.0796 0.536 0.428 0.000 0.036
#&gt; GSM317660     3  0.5575     0.7406 0.056 0.032 0.756 0.156
#&gt; GSM317663     4  0.5429     0.7146 0.072 0.208 0.000 0.720
#&gt; GSM317664     1  0.2345     0.8559 0.900 0.000 0.100 0.000
#&gt; GSM317665     3  0.5220     0.7512 0.092 0.000 0.752 0.156
#&gt; GSM317673     1  0.2675     0.8541 0.892 0.000 0.100 0.008
#&gt; GSM317686     4  0.4252     0.6467 0.000 0.252 0.004 0.744
#&gt; GSM317688     1  0.2675     0.8541 0.892 0.000 0.100 0.008
#&gt; GSM317690     2  0.2469     0.7906 0.000 0.892 0.000 0.108
#&gt; GSM317654     3  0.5728     0.7283 0.104 0.000 0.708 0.188
#&gt; GSM317655     4  0.5533     0.7083 0.072 0.220 0.000 0.708
#&gt; GSM317659     1  0.2011     0.8181 0.920 0.000 0.000 0.080
#&gt; GSM317661     2  0.0000     0.8796 0.000 1.000 0.000 0.000
#&gt; GSM317662     2  0.0000     0.8796 0.000 1.000 0.000 0.000
#&gt; GSM317668     3  0.5353     0.3217 0.432 0.000 0.556 0.012
#&gt; GSM317669     3  0.0188     0.7960 0.004 0.000 0.996 0.000
#&gt; GSM317671     3  0.0336     0.7981 0.008 0.000 0.992 0.000
#&gt; GSM317676     4  0.4730     0.4631 0.364 0.000 0.000 0.636
#&gt; GSM317680     3  0.0336     0.7981 0.008 0.000 0.992 0.000
#&gt; GSM317684     1  0.1557     0.8341 0.944 0.000 0.000 0.056
#&gt; GSM317685     1  0.2530     0.8543 0.896 0.000 0.100 0.004
#&gt; GSM317694     1  0.2450     0.8622 0.912 0.000 0.072 0.016
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-4'>
<p><a id='tab-MAD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     3  0.0404     0.9800 0.000 0.000 0.988 0.000 0.012
#&gt; GSM317652     5  0.6740     0.4949 0.140 0.000 0.356 0.024 0.480
#&gt; GSM317666     4  0.2342     0.7935 0.020 0.024 0.000 0.916 0.040
#&gt; GSM317672     2  0.5465     0.3877 0.008 0.600 0.008 0.040 0.344
#&gt; GSM317679     3  0.0000     0.9922 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317681     5  0.5849     0.3899 0.008 0.256 0.016 0.080 0.640
#&gt; GSM317682     1  0.4954     0.0891 0.568 0.004 0.016 0.004 0.408
#&gt; GSM317683     2  0.0162     0.8473 0.000 0.996 0.000 0.004 0.000
#&gt; GSM317689     2  0.2416     0.8005 0.000 0.888 0.000 0.100 0.012
#&gt; GSM317691     1  0.4766     0.7032 0.732 0.000 0.000 0.136 0.132
#&gt; GSM317692     1  0.5624     0.6286 0.648 0.004 0.000 0.140 0.208
#&gt; GSM317693     1  0.2825     0.7811 0.860 0.000 0.000 0.016 0.124
#&gt; GSM317696     1  0.0992     0.7993 0.968 0.000 0.024 0.000 0.008
#&gt; GSM317697     1  0.1197     0.7961 0.952 0.000 0.000 0.000 0.048
#&gt; GSM317698     1  0.0290     0.8004 0.992 0.000 0.008 0.000 0.000
#&gt; GSM317650     2  0.0162     0.8473 0.000 0.996 0.000 0.004 0.000
#&gt; GSM317651     5  0.4607     0.4324 0.320 0.000 0.020 0.004 0.656
#&gt; GSM317657     4  0.2635     0.7981 0.016 0.088 0.000 0.888 0.008
#&gt; GSM317667     4  0.4793     0.7250 0.000 0.076 0.000 0.708 0.216
#&gt; GSM317670     2  0.6891     0.3629 0.180 0.524 0.000 0.264 0.032
#&gt; GSM317674     1  0.0992     0.7993 0.968 0.000 0.024 0.000 0.008
#&gt; GSM317675     1  0.0992     0.7993 0.968 0.000 0.024 0.000 0.008
#&gt; GSM317677     1  0.3003     0.7793 0.864 0.000 0.000 0.044 0.092
#&gt; GSM317678     2  0.0404     0.8437 0.000 0.988 0.000 0.000 0.012
#&gt; GSM317687     1  0.5345     0.6467 0.668 0.000 0.000 0.196 0.136
#&gt; GSM317695     3  0.0404     0.9767 0.012 0.000 0.988 0.000 0.000
#&gt; GSM317653     5  0.5406     0.5302 0.056 0.056 0.012 0.136 0.740
#&gt; GSM317656     1  0.5654     0.4913 0.672 0.000 0.212 0.028 0.088
#&gt; GSM317658     1  0.6360     0.3860 0.576 0.300 0.000 0.064 0.060
#&gt; GSM317660     5  0.4908     0.5471 0.004 0.012 0.380 0.008 0.596
#&gt; GSM317663     4  0.2052     0.8037 0.004 0.080 0.000 0.912 0.004
#&gt; GSM317664     1  0.0992     0.7993 0.968 0.000 0.024 0.000 0.008
#&gt; GSM317665     5  0.4757     0.5660 0.024 0.000 0.380 0.000 0.596
#&gt; GSM317673     1  0.1356     0.7957 0.956 0.000 0.028 0.004 0.012
#&gt; GSM317686     4  0.5223     0.7183 0.000 0.108 0.000 0.672 0.220
#&gt; GSM317688     1  0.2772     0.7774 0.896 0.000 0.028 0.032 0.044
#&gt; GSM317690     2  0.2825     0.7718 0.000 0.860 0.000 0.124 0.016
#&gt; GSM317654     5  0.4526     0.6200 0.028 0.000 0.300 0.000 0.672
#&gt; GSM317655     4  0.2623     0.7944 0.004 0.096 0.000 0.884 0.016
#&gt; GSM317659     1  0.3918     0.7503 0.804 0.000 0.000 0.096 0.100
#&gt; GSM317661     2  0.0162     0.8473 0.000 0.996 0.000 0.004 0.000
#&gt; GSM317662     2  0.0162     0.8473 0.000 0.996 0.000 0.004 0.000
#&gt; GSM317668     1  0.6808     0.2803 0.536 0.000 0.284 0.040 0.140
#&gt; GSM317669     3  0.0000     0.9922 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317671     3  0.0000     0.9922 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317676     4  0.5250     0.4723 0.224 0.000 0.000 0.668 0.108
#&gt; GSM317680     3  0.0000     0.9922 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317684     1  0.2879     0.7814 0.868 0.000 0.000 0.032 0.100
#&gt; GSM317685     1  0.1653     0.7961 0.944 0.000 0.028 0.004 0.024
#&gt; GSM317694     1  0.2664     0.7867 0.884 0.000 0.004 0.020 0.092
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-5'>
<p><a id='tab-MAD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM317649     3  0.1015      0.969 0.004 0.000 0.968 0.004 0.012 0.012
#&gt; GSM317652     5  0.6936      0.412 0.284 0.000 0.124 0.004 0.472 0.116
#&gt; GSM317666     4  0.3728      0.392 0.004 0.000 0.000 0.788 0.068 0.140
#&gt; GSM317672     5  0.6959      0.105 0.004 0.320 0.000 0.116 0.444 0.116
#&gt; GSM317679     3  0.0146      0.989 0.004 0.000 0.996 0.000 0.000 0.000
#&gt; GSM317681     5  0.4343      0.620 0.004 0.064 0.000 0.056 0.780 0.096
#&gt; GSM317682     1  0.5610     -0.217 0.440 0.000 0.000 0.000 0.416 0.144
#&gt; GSM317683     2  0.0146      0.861 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM317689     2  0.4844      0.489 0.000 0.620 0.000 0.320 0.020 0.040
#&gt; GSM317691     1  0.7110      0.314 0.456 0.000 0.000 0.184 0.132 0.228
#&gt; GSM317692     6  0.7619     -0.281 0.292 0.000 0.000 0.224 0.180 0.304
#&gt; GSM317693     1  0.4586      0.600 0.692 0.000 0.000 0.004 0.088 0.216
#&gt; GSM317696     1  0.0993      0.698 0.964 0.000 0.000 0.000 0.012 0.024
#&gt; GSM317697     1  0.2380      0.682 0.892 0.000 0.000 0.004 0.036 0.068
#&gt; GSM317698     1  0.0000      0.701 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317650     2  0.0000      0.861 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317651     5  0.3641      0.650 0.140 0.000 0.000 0.000 0.788 0.072
#&gt; GSM317657     4  0.2665      0.567 0.000 0.024 0.000 0.884 0.032 0.060
#&gt; GSM317667     6  0.4544      0.213 0.000 0.036 0.000 0.416 0.000 0.548
#&gt; GSM317670     4  0.6163      0.155 0.036 0.304 0.004 0.552 0.012 0.092
#&gt; GSM317674     1  0.0146      0.701 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; GSM317675     1  0.0146      0.701 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; GSM317677     1  0.4400      0.608 0.732 0.000 0.000 0.008 0.096 0.164
#&gt; GSM317678     2  0.1434      0.836 0.000 0.940 0.000 0.000 0.012 0.048
#&gt; GSM317687     1  0.7395      0.189 0.392 0.000 0.000 0.244 0.148 0.216
#&gt; GSM317695     3  0.0547      0.973 0.020 0.000 0.980 0.000 0.000 0.000
#&gt; GSM317653     5  0.1180      0.670 0.016 0.000 0.000 0.012 0.960 0.012
#&gt; GSM317656     1  0.4297      0.588 0.784 0.000 0.052 0.004 0.072 0.088
#&gt; GSM317658     1  0.7374      0.277 0.508 0.208 0.000 0.140 0.056 0.088
#&gt; GSM317660     5  0.3403      0.701 0.020 0.004 0.148 0.004 0.816 0.008
#&gt; GSM317663     4  0.1542      0.554 0.000 0.016 0.000 0.944 0.024 0.016
#&gt; GSM317664     1  0.0146      0.701 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; GSM317665     5  0.3210      0.705 0.036 0.000 0.152 0.000 0.812 0.000
#&gt; GSM317673     1  0.1745      0.686 0.924 0.000 0.000 0.000 0.020 0.056
#&gt; GSM317686     6  0.4544      0.213 0.000 0.036 0.000 0.416 0.000 0.548
#&gt; GSM317688     1  0.2988      0.647 0.824 0.000 0.000 0.000 0.024 0.152
#&gt; GSM317690     2  0.4363      0.608 0.000 0.688 0.004 0.264 0.004 0.040
#&gt; GSM317654     5  0.2110      0.708 0.012 0.000 0.084 0.004 0.900 0.000
#&gt; GSM317655     4  0.1743      0.553 0.000 0.028 0.004 0.936 0.008 0.024
#&gt; GSM317659     1  0.6586      0.405 0.548 0.000 0.000 0.160 0.120 0.172
#&gt; GSM317661     2  0.0146      0.860 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM317662     2  0.0000      0.861 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317668     1  0.7144      0.371 0.564 0.000 0.120 0.076 0.136 0.104
#&gt; GSM317669     3  0.0146      0.989 0.004 0.000 0.996 0.000 0.000 0.000
#&gt; GSM317671     3  0.0146      0.989 0.004 0.000 0.996 0.000 0.000 0.000
#&gt; GSM317676     4  0.6631      0.168 0.172 0.000 0.000 0.540 0.116 0.172
#&gt; GSM317680     3  0.0146      0.989 0.004 0.000 0.996 0.000 0.000 0.000
#&gt; GSM317684     1  0.5273      0.564 0.648 0.000 0.000 0.020 0.124 0.208
#&gt; GSM317685     1  0.1983      0.685 0.908 0.000 0.000 0.000 0.020 0.072
#&gt; GSM317694     1  0.4355      0.610 0.736 0.000 0.000 0.008 0.092 0.164
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-kmeans-signature_compare](figure_cola/MAD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-kmeans-collect-classes](figure_cola/MAD-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> MAD:kmeans 48            0.719 2
#> MAD:kmeans 44            0.823 3
#> MAD:kmeans 45            0.878 4
#> MAD:kmeans 40            0.474 5
#> MAD:kmeans 35            0.869 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "skmeans"]
# you can also extract it by
# res = res_list["MAD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-skmeans-collect-plots](figure_cola/MAD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-skmeans-select-partition-number](figure_cola/MAD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.916           0.960       0.980         0.4921 0.510   0.510
#> 3 3 1.000           0.968       0.983         0.3604 0.805   0.624
#> 4 4 0.774           0.830       0.907         0.1290 0.875   0.643
#> 5 5 0.677           0.587       0.795         0.0605 0.969   0.873
#> 6 6 0.700           0.592       0.765         0.0404 0.906   0.610
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-skmeans-get-classes-1'>
<p><a id='tab-MAD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     1  0.0000      0.978 1.000 0.000
#&gt; GSM317652     1  0.0000      0.978 1.000 0.000
#&gt; GSM317666     2  0.0000      0.981 0.000 1.000
#&gt; GSM317672     2  0.0000      0.981 0.000 1.000
#&gt; GSM317679     1  0.7056      0.776 0.808 0.192
#&gt; GSM317681     2  0.0000      0.981 0.000 1.000
#&gt; GSM317682     1  0.0000      0.978 1.000 0.000
#&gt; GSM317683     2  0.0000      0.981 0.000 1.000
#&gt; GSM317689     2  0.0000      0.981 0.000 1.000
#&gt; GSM317691     1  0.4939      0.886 0.892 0.108
#&gt; GSM317692     2  0.0000      0.981 0.000 1.000
#&gt; GSM317693     1  0.0000      0.978 1.000 0.000
#&gt; GSM317696     1  0.0000      0.978 1.000 0.000
#&gt; GSM317697     1  0.0672      0.972 0.992 0.008
#&gt; GSM317698     1  0.0000      0.978 1.000 0.000
#&gt; GSM317650     2  0.0000      0.981 0.000 1.000
#&gt; GSM317651     1  0.0000      0.978 1.000 0.000
#&gt; GSM317657     2  0.0000      0.981 0.000 1.000
#&gt; GSM317667     2  0.0000      0.981 0.000 1.000
#&gt; GSM317670     2  0.0000      0.981 0.000 1.000
#&gt; GSM317674     1  0.0000      0.978 1.000 0.000
#&gt; GSM317675     1  0.0000      0.978 1.000 0.000
#&gt; GSM317677     1  0.0000      0.978 1.000 0.000
#&gt; GSM317678     2  0.0000      0.981 0.000 1.000
#&gt; GSM317687     1  0.3879      0.915 0.924 0.076
#&gt; GSM317695     1  0.0000      0.978 1.000 0.000
#&gt; GSM317653     2  0.7139      0.762 0.196 0.804
#&gt; GSM317656     1  0.0000      0.978 1.000 0.000
#&gt; GSM317658     2  0.0000      0.981 0.000 1.000
#&gt; GSM317660     2  0.6531      0.807 0.168 0.832
#&gt; GSM317663     2  0.0000      0.981 0.000 1.000
#&gt; GSM317664     1  0.0000      0.978 1.000 0.000
#&gt; GSM317665     1  0.0000      0.978 1.000 0.000
#&gt; GSM317673     1  0.0000      0.978 1.000 0.000
#&gt; GSM317686     2  0.0000      0.981 0.000 1.000
#&gt; GSM317688     1  0.0000      0.978 1.000 0.000
#&gt; GSM317690     2  0.0000      0.981 0.000 1.000
#&gt; GSM317654     1  0.0000      0.978 1.000 0.000
#&gt; GSM317655     2  0.0000      0.981 0.000 1.000
#&gt; GSM317659     1  0.0000      0.978 1.000 0.000
#&gt; GSM317661     2  0.0000      0.981 0.000 1.000
#&gt; GSM317662     2  0.0000      0.981 0.000 1.000
#&gt; GSM317668     1  0.0000      0.978 1.000 0.000
#&gt; GSM317669     1  0.0000      0.978 1.000 0.000
#&gt; GSM317671     1  0.6048      0.833 0.852 0.148
#&gt; GSM317676     1  0.4298      0.903 0.912 0.088
#&gt; GSM317680     1  0.0000      0.978 1.000 0.000
#&gt; GSM317684     1  0.0000      0.978 1.000 0.000
#&gt; GSM317685     1  0.0000      0.978 1.000 0.000
#&gt; GSM317694     1  0.0000      0.978 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-2'>
<p><a id='tab-MAD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM317649     3  0.0000      0.996 0.000 0.000 1.000
#&gt; GSM317652     3  0.0000      0.996 0.000 0.000 1.000
#&gt; GSM317666     2  0.0892      0.968 0.020 0.980 0.000
#&gt; GSM317672     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM317679     3  0.0000      0.996 0.000 0.000 1.000
#&gt; GSM317681     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM317682     1  0.1163      0.969 0.972 0.000 0.028
#&gt; GSM317683     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM317689     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM317691     1  0.0000      0.977 1.000 0.000 0.000
#&gt; GSM317692     2  0.0892      0.966 0.020 0.980 0.000
#&gt; GSM317693     1  0.0000      0.977 1.000 0.000 0.000
#&gt; GSM317696     1  0.0747      0.975 0.984 0.000 0.016
#&gt; GSM317697     1  0.0000      0.977 1.000 0.000 0.000
#&gt; GSM317698     1  0.0237      0.978 0.996 0.000 0.004
#&gt; GSM317650     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM317651     1  0.3619      0.857 0.864 0.000 0.136
#&gt; GSM317657     2  0.0424      0.977 0.008 0.992 0.000
#&gt; GSM317667     2  0.0237      0.979 0.004 0.996 0.000
#&gt; GSM317670     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM317674     1  0.0592      0.977 0.988 0.000 0.012
#&gt; GSM317675     1  0.0592      0.977 0.988 0.000 0.012
#&gt; GSM317677     1  0.0000      0.977 1.000 0.000 0.000
#&gt; GSM317678     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM317687     1  0.0000      0.977 1.000 0.000 0.000
#&gt; GSM317695     3  0.0000      0.996 0.000 0.000 1.000
#&gt; GSM317653     2  0.6301      0.617 0.028 0.712 0.260
#&gt; GSM317656     3  0.0237      0.992 0.004 0.000 0.996
#&gt; GSM317658     2  0.0747      0.969 0.016 0.984 0.000
#&gt; GSM317660     3  0.0000      0.996 0.000 0.000 1.000
#&gt; GSM317663     2  0.0237      0.979 0.004 0.996 0.000
#&gt; GSM317664     1  0.0747      0.975 0.984 0.000 0.016
#&gt; GSM317665     3  0.0000      0.996 0.000 0.000 1.000
#&gt; GSM317673     1  0.1031      0.971 0.976 0.000 0.024
#&gt; GSM317686     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM317688     1  0.3752      0.853 0.856 0.000 0.144
#&gt; GSM317690     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM317654     3  0.0000      0.996 0.000 0.000 1.000
#&gt; GSM317655     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM317659     1  0.0000      0.977 1.000 0.000 0.000
#&gt; GSM317661     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM317662     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM317668     3  0.1643      0.953 0.044 0.000 0.956
#&gt; GSM317669     3  0.0000      0.996 0.000 0.000 1.000
#&gt; GSM317671     3  0.0000      0.996 0.000 0.000 1.000
#&gt; GSM317676     1  0.0000      0.977 1.000 0.000 0.000
#&gt; GSM317680     3  0.0000      0.996 0.000 0.000 1.000
#&gt; GSM317684     1  0.0000      0.977 1.000 0.000 0.000
#&gt; GSM317685     1  0.1031      0.971 0.976 0.000 0.024
#&gt; GSM317694     1  0.0237      0.978 0.996 0.000 0.004
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-3'>
<p><a id='tab-MAD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     3  0.0000      0.948 0.000 0.000 1.000 0.000
#&gt; GSM317652     3  0.1297      0.940 0.020 0.000 0.964 0.016
#&gt; GSM317666     4  0.1302      0.781 0.000 0.044 0.000 0.956
#&gt; GSM317672     2  0.0336      0.922 0.000 0.992 0.000 0.008
#&gt; GSM317679     3  0.0000      0.948 0.000 0.000 1.000 0.000
#&gt; GSM317681     2  0.1302      0.903 0.000 0.956 0.000 0.044
#&gt; GSM317682     1  0.1545      0.880 0.952 0.000 0.008 0.040
#&gt; GSM317683     2  0.0000      0.926 0.000 1.000 0.000 0.000
#&gt; GSM317689     2  0.0000      0.926 0.000 1.000 0.000 0.000
#&gt; GSM317691     4  0.4713      0.231 0.360 0.000 0.000 0.640
#&gt; GSM317692     2  0.4857      0.573 0.016 0.700 0.000 0.284
#&gt; GSM317693     1  0.2281      0.862 0.904 0.000 0.000 0.096
#&gt; GSM317696     1  0.0000      0.898 1.000 0.000 0.000 0.000
#&gt; GSM317697     1  0.0188      0.897 0.996 0.000 0.000 0.004
#&gt; GSM317698     1  0.0000      0.898 1.000 0.000 0.000 0.000
#&gt; GSM317650     2  0.0000      0.926 0.000 1.000 0.000 0.000
#&gt; GSM317651     1  0.5111      0.688 0.740 0.000 0.204 0.056
#&gt; GSM317657     4  0.3610      0.774 0.000 0.200 0.000 0.800
#&gt; GSM317667     4  0.3486      0.783 0.000 0.188 0.000 0.812
#&gt; GSM317670     2  0.3528      0.722 0.000 0.808 0.000 0.192
#&gt; GSM317674     1  0.0000      0.898 1.000 0.000 0.000 0.000
#&gt; GSM317675     1  0.0000      0.898 1.000 0.000 0.000 0.000
#&gt; GSM317677     1  0.3311      0.809 0.828 0.000 0.000 0.172
#&gt; GSM317678     2  0.0188      0.924 0.000 0.996 0.000 0.004
#&gt; GSM317687     4  0.1118      0.768 0.036 0.000 0.000 0.964
#&gt; GSM317695     3  0.0817      0.941 0.024 0.000 0.976 0.000
#&gt; GSM317653     4  0.4406      0.693 0.000 0.192 0.028 0.780
#&gt; GSM317656     3  0.3726      0.755 0.212 0.000 0.788 0.000
#&gt; GSM317658     2  0.3015      0.829 0.092 0.884 0.000 0.024
#&gt; GSM317660     3  0.1610      0.933 0.000 0.016 0.952 0.032
#&gt; GSM317663     4  0.3942      0.757 0.000 0.236 0.000 0.764
#&gt; GSM317664     1  0.0000      0.898 1.000 0.000 0.000 0.000
#&gt; GSM317665     3  0.0817      0.943 0.000 0.000 0.976 0.024
#&gt; GSM317673     1  0.0336      0.896 0.992 0.000 0.000 0.008
#&gt; GSM317686     4  0.4277      0.714 0.000 0.280 0.000 0.720
#&gt; GSM317688     1  0.2706      0.844 0.900 0.000 0.080 0.020
#&gt; GSM317690     2  0.1211      0.901 0.000 0.960 0.000 0.040
#&gt; GSM317654     3  0.1022      0.941 0.000 0.000 0.968 0.032
#&gt; GSM317655     4  0.4008      0.750 0.000 0.244 0.000 0.756
#&gt; GSM317659     1  0.4999      0.217 0.508 0.000 0.000 0.492
#&gt; GSM317661     2  0.0000      0.926 0.000 1.000 0.000 0.000
#&gt; GSM317662     2  0.0000      0.926 0.000 1.000 0.000 0.000
#&gt; GSM317668     3  0.3718      0.803 0.168 0.000 0.820 0.012
#&gt; GSM317669     3  0.0000      0.948 0.000 0.000 1.000 0.000
#&gt; GSM317671     3  0.0000      0.948 0.000 0.000 1.000 0.000
#&gt; GSM317676     4  0.1118      0.768 0.036 0.000 0.000 0.964
#&gt; GSM317680     3  0.0000      0.948 0.000 0.000 1.000 0.000
#&gt; GSM317684     1  0.3764      0.769 0.784 0.000 0.000 0.216
#&gt; GSM317685     1  0.0336      0.897 0.992 0.000 0.000 0.008
#&gt; GSM317694     1  0.2530      0.853 0.888 0.000 0.000 0.112
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-4'>
<p><a id='tab-MAD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     3  0.0404    0.69434 0.000 0.000 0.988 0.000 0.012
#&gt; GSM317652     3  0.4822    0.33330 0.032 0.000 0.616 0.000 0.352
#&gt; GSM317666     4  0.1018    0.67801 0.000 0.016 0.000 0.968 0.016
#&gt; GSM317672     2  0.1410    0.83419 0.000 0.940 0.000 0.000 0.060
#&gt; GSM317679     3  0.0000    0.69757 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317681     2  0.4976    0.58666 0.000 0.696 0.004 0.072 0.228
#&gt; GSM317682     1  0.3796    0.44657 0.700 0.000 0.000 0.000 0.300
#&gt; GSM317683     2  0.0000    0.85769 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317689     2  0.0290    0.85573 0.000 0.992 0.000 0.008 0.000
#&gt; GSM317691     4  0.6763    0.01326 0.288 0.000 0.000 0.400 0.312
#&gt; GSM317692     2  0.6691    0.35555 0.016 0.528 0.000 0.240 0.216
#&gt; GSM317693     1  0.4618    0.65208 0.724 0.000 0.000 0.068 0.208
#&gt; GSM317696     1  0.0162    0.75976 0.996 0.000 0.000 0.000 0.004
#&gt; GSM317697     1  0.1671    0.74398 0.924 0.000 0.000 0.000 0.076
#&gt; GSM317698     1  0.0290    0.76015 0.992 0.000 0.000 0.000 0.008
#&gt; GSM317650     2  0.0000    0.85769 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317651     5  0.5177    0.40897 0.232 0.000 0.076 0.008 0.684
#&gt; GSM317657     4  0.2873    0.71688 0.000 0.120 0.000 0.860 0.020
#&gt; GSM317667     4  0.2329    0.71128 0.000 0.124 0.000 0.876 0.000
#&gt; GSM317670     2  0.5709    0.52116 0.004 0.648 0.016 0.252 0.080
#&gt; GSM317674     1  0.0290    0.76052 0.992 0.000 0.000 0.000 0.008
#&gt; GSM317675     1  0.0290    0.76062 0.992 0.000 0.000 0.000 0.008
#&gt; GSM317677     1  0.5681    0.54456 0.608 0.000 0.000 0.124 0.268
#&gt; GSM317678     2  0.0162    0.85666 0.000 0.996 0.000 0.000 0.004
#&gt; GSM317687     4  0.4465    0.46945 0.024 0.000 0.000 0.672 0.304
#&gt; GSM317695     3  0.1704    0.65419 0.068 0.000 0.928 0.000 0.004
#&gt; GSM317653     5  0.5857    0.29970 0.000 0.092 0.016 0.280 0.612
#&gt; GSM317656     3  0.5533    0.29067 0.336 0.000 0.580 0.000 0.084
#&gt; GSM317658     2  0.4773    0.71990 0.096 0.768 0.000 0.028 0.108
#&gt; GSM317660     3  0.4824    0.00891 0.000 0.020 0.512 0.000 0.468
#&gt; GSM317663     4  0.2561    0.71441 0.000 0.144 0.000 0.856 0.000
#&gt; GSM317664     1  0.0451    0.75850 0.988 0.000 0.004 0.000 0.008
#&gt; GSM317665     3  0.4262    0.11304 0.000 0.000 0.560 0.000 0.440
#&gt; GSM317673     1  0.1502    0.74704 0.940 0.000 0.000 0.004 0.056
#&gt; GSM317686     4  0.3143    0.67312 0.000 0.204 0.000 0.796 0.000
#&gt; GSM317688     1  0.4567    0.58850 0.752 0.000 0.080 0.004 0.164
#&gt; GSM317690     2  0.2423    0.80270 0.000 0.896 0.000 0.080 0.024
#&gt; GSM317654     5  0.4787   -0.08496 0.000 0.000 0.432 0.020 0.548
#&gt; GSM317655     4  0.3183    0.70765 0.000 0.156 0.000 0.828 0.016
#&gt; GSM317659     1  0.6822    0.08060 0.344 0.000 0.000 0.340 0.316
#&gt; GSM317661     2  0.0000    0.85769 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317662     2  0.0000    0.85769 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317668     3  0.6606    0.30565 0.212 0.000 0.564 0.024 0.200
#&gt; GSM317669     3  0.0000    0.69757 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317671     3  0.0000    0.69757 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317676     4  0.3766    0.52541 0.004 0.000 0.000 0.728 0.268
#&gt; GSM317680     3  0.0000    0.69757 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317684     1  0.6254    0.43236 0.500 0.000 0.000 0.160 0.340
#&gt; GSM317685     1  0.2612    0.72833 0.868 0.000 0.000 0.008 0.124
#&gt; GSM317694     1  0.4898    0.61077 0.684 0.000 0.000 0.068 0.248
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-5'>
<p><a id='tab-MAD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM317649     3  0.1268     0.8190 0.000 0.000 0.952 0.004 0.036 0.008
#&gt; GSM317652     5  0.6237     0.2808 0.084 0.000 0.412 0.008 0.448 0.048
#&gt; GSM317666     4  0.2288     0.7662 0.000 0.004 0.000 0.876 0.004 0.116
#&gt; GSM317672     2  0.2393     0.7659 0.000 0.892 0.000 0.004 0.064 0.040
#&gt; GSM317679     3  0.0146     0.8489 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; GSM317681     2  0.6060     0.4290 0.000 0.576 0.008 0.088 0.272 0.056
#&gt; GSM317682     1  0.5365     0.4919 0.612 0.008 0.004 0.000 0.260 0.116
#&gt; GSM317683     2  0.0653     0.7894 0.000 0.980 0.000 0.012 0.004 0.004
#&gt; GSM317689     2  0.2076     0.7734 0.000 0.912 0.000 0.060 0.012 0.016
#&gt; GSM317691     6  0.5665     0.5464 0.148 0.000 0.000 0.140 0.064 0.648
#&gt; GSM317692     2  0.7990     0.1675 0.048 0.376 0.000 0.160 0.132 0.284
#&gt; GSM317693     1  0.4941     0.0169 0.492 0.000 0.000 0.000 0.064 0.444
#&gt; GSM317696     1  0.0520     0.7007 0.984 0.000 0.000 0.000 0.008 0.008
#&gt; GSM317697     1  0.4136     0.5478 0.732 0.000 0.000 0.000 0.076 0.192
#&gt; GSM317698     1  0.0891     0.6983 0.968 0.000 0.000 0.000 0.008 0.024
#&gt; GSM317650     2  0.0767     0.7899 0.000 0.976 0.000 0.012 0.004 0.008
#&gt; GSM317651     5  0.5532     0.4961 0.160 0.000 0.028 0.008 0.656 0.148
#&gt; GSM317657     4  0.3133     0.8708 0.000 0.064 0.000 0.852 0.016 0.068
#&gt; GSM317667     4  0.1657     0.8902 0.000 0.056 0.000 0.928 0.000 0.016
#&gt; GSM317670     2  0.6782     0.3590 0.000 0.504 0.016 0.280 0.064 0.136
#&gt; GSM317674     1  0.0603     0.7009 0.980 0.000 0.000 0.000 0.004 0.016
#&gt; GSM317675     1  0.0603     0.6990 0.980 0.000 0.000 0.000 0.004 0.016
#&gt; GSM317677     6  0.4473     0.1752 0.480 0.000 0.000 0.028 0.000 0.492
#&gt; GSM317678     2  0.0725     0.7877 0.000 0.976 0.000 0.000 0.012 0.012
#&gt; GSM317687     6  0.4090     0.3929 0.004 0.000 0.000 0.384 0.008 0.604
#&gt; GSM317695     3  0.1434     0.7964 0.048 0.000 0.940 0.000 0.012 0.000
#&gt; GSM317653     5  0.6274     0.4358 0.000 0.072 0.008 0.180 0.596 0.144
#&gt; GSM317656     1  0.6706    -0.1068 0.408 0.000 0.392 0.016 0.148 0.036
#&gt; GSM317658     2  0.6385     0.6049 0.084 0.636 0.000 0.056 0.092 0.132
#&gt; GSM317660     5  0.4752     0.6182 0.000 0.028 0.268 0.012 0.672 0.020
#&gt; GSM317663     4  0.1686     0.8977 0.000 0.064 0.000 0.924 0.000 0.012
#&gt; GSM317664     1  0.0858     0.6991 0.968 0.000 0.000 0.000 0.004 0.028
#&gt; GSM317665     5  0.3804     0.5804 0.000 0.000 0.336 0.008 0.656 0.000
#&gt; GSM317673     1  0.2058     0.6854 0.908 0.000 0.000 0.000 0.056 0.036
#&gt; GSM317686     4  0.1765     0.8832 0.000 0.096 0.000 0.904 0.000 0.000
#&gt; GSM317688     1  0.5848     0.5334 0.648 0.000 0.092 0.004 0.140 0.116
#&gt; GSM317690     2  0.4153     0.6477 0.000 0.752 0.000 0.184 0.024 0.040
#&gt; GSM317654     5  0.4470     0.6457 0.000 0.000 0.228 0.004 0.696 0.072
#&gt; GSM317655     4  0.3013     0.8621 0.000 0.064 0.000 0.864 0.028 0.044
#&gt; GSM317659     6  0.5655     0.6057 0.212 0.000 0.000 0.168 0.020 0.600
#&gt; GSM317661     2  0.1750     0.7847 0.000 0.932 0.000 0.040 0.016 0.012
#&gt; GSM317662     2  0.0520     0.7894 0.000 0.984 0.000 0.008 0.000 0.008
#&gt; GSM317668     3  0.7685    -0.0149 0.192 0.000 0.420 0.024 0.232 0.132
#&gt; GSM317669     3  0.0260     0.8479 0.000 0.000 0.992 0.000 0.008 0.000
#&gt; GSM317671     3  0.0146     0.8500 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM317676     6  0.4177     0.2152 0.000 0.000 0.000 0.468 0.012 0.520
#&gt; GSM317680     3  0.0000     0.8500 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317684     6  0.4712     0.4950 0.272 0.000 0.000 0.040 0.024 0.664
#&gt; GSM317685     1  0.3822     0.6200 0.776 0.000 0.000 0.000 0.096 0.128
#&gt; GSM317694     1  0.4002    -0.0096 0.588 0.000 0.000 0.008 0.000 0.404
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-skmeans-signature_compare](figure_cola/MAD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-skmeans-collect-classes](figure_cola/MAD-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>              n disease.state(p) k
#> MAD:skmeans 50            0.448 2
#> MAD:skmeans 50            0.689 3
#> MAD:skmeans 48            0.738 4
#> MAD:skmeans 36            0.857 5
#> MAD:skmeans 35            0.783 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "pam"]
# you can also extract it by
# res = res_list["MAD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-pam-collect-plots](figure_cola/MAD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-pam-select-partition-number](figure_cola/MAD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.999           0.956       0.981         0.4649 0.542   0.542
#> 3 3 0.624           0.865       0.901         0.2984 0.868   0.756
#> 4 4 0.796           0.874       0.934         0.1255 0.926   0.821
#> 5 5 0.794           0.796       0.900         0.0610 0.811   0.529
#> 6 6 0.759           0.719       0.850         0.0591 0.928   0.746
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-classes'>
<ul>
<li><a href='#tab-MAD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-pam-get-classes-1'>
<p><a id='tab-MAD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     1  0.0000      0.976 1.000 0.000
#&gt; GSM317652     1  0.0000      0.976 1.000 0.000
#&gt; GSM317666     2  0.0000      0.987 0.000 1.000
#&gt; GSM317672     1  0.0376      0.973 0.996 0.004
#&gt; GSM317679     1  0.0000      0.976 1.000 0.000
#&gt; GSM317681     1  0.2043      0.949 0.968 0.032
#&gt; GSM317682     1  0.0000      0.976 1.000 0.000
#&gt; GSM317683     2  0.0000      0.987 0.000 1.000
#&gt; GSM317689     2  0.0000      0.987 0.000 1.000
#&gt; GSM317691     2  0.0000      0.987 0.000 1.000
#&gt; GSM317692     1  0.9044      0.546 0.680 0.320
#&gt; GSM317693     1  0.0000      0.976 1.000 0.000
#&gt; GSM317696     1  0.0000      0.976 1.000 0.000
#&gt; GSM317697     1  0.0000      0.976 1.000 0.000
#&gt; GSM317698     1  0.0000      0.976 1.000 0.000
#&gt; GSM317650     2  0.0000      0.987 0.000 1.000
#&gt; GSM317651     1  0.0000      0.976 1.000 0.000
#&gt; GSM317657     2  0.0000      0.987 0.000 1.000
#&gt; GSM317667     2  0.0000      0.987 0.000 1.000
#&gt; GSM317670     2  0.0000      0.987 0.000 1.000
#&gt; GSM317674     1  0.0000      0.976 1.000 0.000
#&gt; GSM317675     1  0.0000      0.976 1.000 0.000
#&gt; GSM317677     1  0.0000      0.976 1.000 0.000
#&gt; GSM317678     2  0.2423      0.950 0.040 0.960
#&gt; GSM317687     2  0.0000      0.987 0.000 1.000
#&gt; GSM317695     1  0.0000      0.976 1.000 0.000
#&gt; GSM317653     1  0.4161      0.898 0.916 0.084
#&gt; GSM317656     1  0.0000      0.976 1.000 0.000
#&gt; GSM317658     1  0.9000      0.548 0.684 0.316
#&gt; GSM317660     1  0.0000      0.976 1.000 0.000
#&gt; GSM317663     2  0.0000      0.987 0.000 1.000
#&gt; GSM317664     1  0.0000      0.976 1.000 0.000
#&gt; GSM317665     1  0.0000      0.976 1.000 0.000
#&gt; GSM317673     1  0.0000      0.976 1.000 0.000
#&gt; GSM317686     2  0.0000      0.987 0.000 1.000
#&gt; GSM317688     1  0.0000      0.976 1.000 0.000
#&gt; GSM317690     2  0.0000      0.987 0.000 1.000
#&gt; GSM317654     1  0.0000      0.976 1.000 0.000
#&gt; GSM317655     2  0.0000      0.987 0.000 1.000
#&gt; GSM317659     1  0.0000      0.976 1.000 0.000
#&gt; GSM317661     2  0.0000      0.987 0.000 1.000
#&gt; GSM317662     2  0.0000      0.987 0.000 1.000
#&gt; GSM317668     1  0.0000      0.976 1.000 0.000
#&gt; GSM317669     1  0.0000      0.976 1.000 0.000
#&gt; GSM317671     1  0.0000      0.976 1.000 0.000
#&gt; GSM317676     2  0.6343      0.806 0.160 0.840
#&gt; GSM317680     1  0.0000      0.976 1.000 0.000
#&gt; GSM317684     1  0.0000      0.976 1.000 0.000
#&gt; GSM317685     1  0.0000      0.976 1.000 0.000
#&gt; GSM317694     1  0.0000      0.976 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-1-a').click(function(){
  $('#tab-MAD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-2'>
<p><a id='tab-MAD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM317649     3  0.4235      0.954 0.176 0.000 0.824
#&gt; GSM317652     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM317666     2  0.0237      0.908 0.000 0.996 0.004
#&gt; GSM317672     1  0.4811      0.804 0.828 0.148 0.024
#&gt; GSM317679     3  0.4723      0.750 0.016 0.160 0.824
#&gt; GSM317681     1  0.5574      0.775 0.784 0.184 0.032
#&gt; GSM317682     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM317683     2  0.4178      0.890 0.000 0.828 0.172
#&gt; GSM317689     2  0.1964      0.915 0.000 0.944 0.056
#&gt; GSM317691     2  0.0237      0.907 0.004 0.996 0.000
#&gt; GSM317692     1  0.5882      0.597 0.652 0.348 0.000
#&gt; GSM317693     1  0.3879      0.814 0.848 0.152 0.000
#&gt; GSM317696     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM317697     1  0.3941      0.811 0.844 0.156 0.000
#&gt; GSM317698     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM317650     2  0.4178      0.890 0.000 0.828 0.172
#&gt; GSM317651     1  0.0237      0.891 0.996 0.000 0.004
#&gt; GSM317657     2  0.0000      0.909 0.000 1.000 0.000
#&gt; GSM317667     2  0.1411      0.914 0.000 0.964 0.036
#&gt; GSM317670     2  0.3116      0.909 0.000 0.892 0.108
#&gt; GSM317674     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM317675     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM317677     1  0.0237      0.891 0.996 0.000 0.004
#&gt; GSM317678     2  0.4676      0.887 0.040 0.848 0.112
#&gt; GSM317687     2  0.0237      0.908 0.000 0.996 0.004
#&gt; GSM317695     3  0.4291      0.951 0.180 0.000 0.820
#&gt; GSM317653     1  0.5070      0.754 0.772 0.224 0.004
#&gt; GSM317656     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM317658     1  0.6881      0.488 0.592 0.388 0.020
#&gt; GSM317660     1  0.4834      0.685 0.792 0.004 0.204
#&gt; GSM317663     2  0.0000      0.909 0.000 1.000 0.000
#&gt; GSM317664     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM317665     1  0.0424      0.888 0.992 0.000 0.008
#&gt; GSM317673     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM317686     2  0.2959      0.910 0.000 0.900 0.100
#&gt; GSM317688     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM317690     2  0.3941      0.892 0.000 0.844 0.156
#&gt; GSM317654     1  0.6317      0.765 0.772 0.124 0.104
#&gt; GSM317655     2  0.1031      0.914 0.000 0.976 0.024
#&gt; GSM317659     1  0.0475      0.891 0.992 0.004 0.004
#&gt; GSM317661     2  0.4178      0.890 0.000 0.828 0.172
#&gt; GSM317662     2  0.4178      0.890 0.000 0.828 0.172
#&gt; GSM317668     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM317669     3  0.4235      0.954 0.176 0.000 0.824
#&gt; GSM317671     3  0.4235      0.954 0.176 0.000 0.824
#&gt; GSM317676     2  0.3193      0.805 0.100 0.896 0.004
#&gt; GSM317680     3  0.4235      0.954 0.176 0.000 0.824
#&gt; GSM317684     1  0.4172      0.810 0.840 0.156 0.004
#&gt; GSM317685     1  0.0237      0.891 0.996 0.000 0.004
#&gt; GSM317694     1  0.0000      0.893 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-2-a').click(function(){
  $('#tab-MAD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-3'>
<p><a id='tab-MAD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     3  0.0817      0.990 0.024 0.000 0.976 0.000
#&gt; GSM317652     1  0.0188      0.900 0.996 0.000 0.004 0.000
#&gt; GSM317666     4  0.0000      0.939 0.000 0.000 0.000 1.000
#&gt; GSM317672     1  0.6739      0.462 0.576 0.304 0.000 0.120
#&gt; GSM317679     3  0.0817      0.955 0.000 0.000 0.976 0.024
#&gt; GSM317681     2  0.2611      0.876 0.008 0.896 0.000 0.096
#&gt; GSM317682     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317683     2  0.0000      0.957 0.000 1.000 0.000 0.000
#&gt; GSM317689     4  0.0707      0.936 0.000 0.020 0.000 0.980
#&gt; GSM317691     4  0.0188      0.939 0.004 0.000 0.000 0.996
#&gt; GSM317692     1  0.4697      0.564 0.644 0.000 0.000 0.356
#&gt; GSM317693     1  0.2589      0.842 0.884 0.000 0.000 0.116
#&gt; GSM317696     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317697     1  0.2647      0.840 0.880 0.000 0.000 0.120
#&gt; GSM317698     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317650     2  0.0000      0.957 0.000 1.000 0.000 0.000
#&gt; GSM317651     1  0.0188      0.901 0.996 0.000 0.000 0.004
#&gt; GSM317657     4  0.0188      0.940 0.000 0.004 0.000 0.996
#&gt; GSM317667     4  0.1733      0.923 0.000 0.028 0.024 0.948
#&gt; GSM317670     4  0.2216      0.896 0.000 0.092 0.000 0.908
#&gt; GSM317674     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317675     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317677     1  0.0188      0.901 0.996 0.000 0.000 0.004
#&gt; GSM317678     2  0.1557      0.923 0.000 0.944 0.000 0.056
#&gt; GSM317687     4  0.0000      0.939 0.000 0.000 0.000 1.000
#&gt; GSM317695     3  0.0921      0.986 0.028 0.000 0.972 0.000
#&gt; GSM317653     1  0.3528      0.787 0.808 0.000 0.000 0.192
#&gt; GSM317656     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317658     1  0.7270      0.356 0.504 0.164 0.000 0.332
#&gt; GSM317660     1  0.4643      0.528 0.656 0.000 0.344 0.000
#&gt; GSM317663     4  0.0188      0.940 0.000 0.004 0.000 0.996
#&gt; GSM317664     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317665     1  0.0336      0.899 0.992 0.000 0.008 0.000
#&gt; GSM317673     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317686     4  0.2949      0.884 0.000 0.088 0.024 0.888
#&gt; GSM317688     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317690     4  0.2704      0.866 0.000 0.124 0.000 0.876
#&gt; GSM317654     1  0.4877      0.712 0.752 0.000 0.204 0.044
#&gt; GSM317655     4  0.0336      0.940 0.000 0.008 0.000 0.992
#&gt; GSM317659     1  0.0469      0.898 0.988 0.000 0.000 0.012
#&gt; GSM317661     2  0.0000      0.957 0.000 1.000 0.000 0.000
#&gt; GSM317662     2  0.0000      0.957 0.000 1.000 0.000 0.000
#&gt; GSM317668     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; GSM317669     3  0.0817      0.990 0.024 0.000 0.976 0.000
#&gt; GSM317671     3  0.0817      0.990 0.024 0.000 0.976 0.000
#&gt; GSM317676     4  0.2814      0.783 0.132 0.000 0.000 0.868
#&gt; GSM317680     3  0.0817      0.990 0.024 0.000 0.976 0.000
#&gt; GSM317684     1  0.2704      0.839 0.876 0.000 0.000 0.124
#&gt; GSM317685     1  0.0188      0.901 0.996 0.000 0.000 0.004
#&gt; GSM317694     1  0.0000      0.902 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-3-a').click(function(){
  $('#tab-MAD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-4'>
<p><a id='tab-MAD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317652     1  0.0162      0.939 0.996 0.000 0.004 0.000 0.000
#&gt; GSM317666     4  0.1270      0.680 0.000 0.000 0.000 0.948 0.052
#&gt; GSM317672     4  0.5938      0.487 0.376 0.112 0.000 0.512 0.000
#&gt; GSM317679     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317681     2  0.3461      0.579 0.004 0.772 0.000 0.224 0.000
#&gt; GSM317682     1  0.0000      0.940 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317683     2  0.0000      0.829 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317689     4  0.3622      0.571 0.000 0.136 0.000 0.816 0.048
#&gt; GSM317691     4  0.0404      0.689 0.000 0.000 0.000 0.988 0.012
#&gt; GSM317692     4  0.5016      0.596 0.348 0.000 0.000 0.608 0.044
#&gt; GSM317693     4  0.4201      0.525 0.408 0.000 0.000 0.592 0.000
#&gt; GSM317696     1  0.0000      0.940 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317697     4  0.4201      0.532 0.408 0.000 0.000 0.592 0.000
#&gt; GSM317698     1  0.0000      0.940 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317650     2  0.0000      0.829 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317651     1  0.1331      0.919 0.952 0.000 0.000 0.040 0.008
#&gt; GSM317657     4  0.1484      0.682 0.000 0.008 0.000 0.944 0.048
#&gt; GSM317667     5  0.0290      1.000 0.000 0.000 0.000 0.008 0.992
#&gt; GSM317670     4  0.1893      0.676 0.000 0.024 0.000 0.928 0.048
#&gt; GSM317674     1  0.0000      0.940 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317675     1  0.0000      0.940 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317677     1  0.1484      0.914 0.944 0.000 0.000 0.048 0.008
#&gt; GSM317678     2  0.0000      0.829 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317687     4  0.0290      0.687 0.000 0.000 0.000 0.992 0.008
#&gt; GSM317695     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317653     4  0.4183      0.604 0.324 0.000 0.000 0.668 0.008
#&gt; GSM317656     1  0.0000      0.940 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317658     4  0.5325      0.626 0.276 0.088 0.000 0.636 0.000
#&gt; GSM317660     1  0.3999      0.533 0.656 0.000 0.344 0.000 0.000
#&gt; GSM317663     4  0.1484      0.682 0.000 0.008 0.000 0.944 0.048
#&gt; GSM317664     1  0.0000      0.940 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317665     1  0.0000      0.940 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317673     1  0.0000      0.940 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317686     5  0.0290      1.000 0.000 0.000 0.000 0.008 0.992
#&gt; GSM317688     1  0.0162      0.938 0.996 0.000 0.000 0.004 0.000
#&gt; GSM317690     2  0.5407      0.260 0.004 0.524 0.000 0.424 0.048
#&gt; GSM317654     1  0.5869      0.460 0.628 0.000 0.148 0.216 0.008
#&gt; GSM317655     4  0.1893      0.676 0.000 0.024 0.000 0.928 0.048
#&gt; GSM317659     1  0.2077      0.882 0.908 0.000 0.000 0.084 0.008
#&gt; GSM317661     2  0.0000      0.829 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317662     2  0.0000      0.829 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317668     1  0.0000      0.940 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317669     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317671     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317676     4  0.0290      0.687 0.000 0.000 0.000 0.992 0.008
#&gt; GSM317680     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317684     4  0.4298      0.566 0.352 0.000 0.000 0.640 0.008
#&gt; GSM317685     1  0.1251      0.921 0.956 0.000 0.000 0.036 0.008
#&gt; GSM317694     1  0.1251      0.921 0.956 0.000 0.000 0.036 0.008
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-4-a').click(function(){
  $('#tab-MAD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-5'>
<p><a id='tab-MAD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM317649     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317652     1  0.0146     0.9033 0.996 0.000 0.004 0.000 0.000 0.000
#&gt; GSM317666     5  0.4569     0.4969 0.008 0.000 0.000 0.408 0.560 0.024
#&gt; GSM317672     5  0.5166     0.4899 0.384 0.092 0.000 0.000 0.524 0.000
#&gt; GSM317679     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317681     2  0.3565     0.5509 0.000 0.692 0.000 0.004 0.304 0.000
#&gt; GSM317682     1  0.0790     0.8944 0.968 0.000 0.000 0.000 0.032 0.000
#&gt; GSM317683     2  0.0000     0.8941 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317689     5  0.5472     0.3846 0.000 0.124 0.000 0.412 0.464 0.000
#&gt; GSM317691     5  0.3634     0.5272 0.000 0.000 0.000 0.356 0.644 0.000
#&gt; GSM317692     5  0.4357     0.5604 0.340 0.000 0.000 0.036 0.624 0.000
#&gt; GSM317693     5  0.3717     0.5352 0.384 0.000 0.000 0.000 0.616 0.000
#&gt; GSM317696     1  0.0713     0.8959 0.972 0.000 0.000 0.000 0.028 0.000
#&gt; GSM317697     5  0.3747     0.5313 0.396 0.000 0.000 0.000 0.604 0.000
#&gt; GSM317698     1  0.0260     0.9018 0.992 0.000 0.000 0.000 0.008 0.000
#&gt; GSM317650     2  0.0000     0.8941 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317651     1  0.1850     0.8718 0.924 0.000 0.000 0.016 0.052 0.008
#&gt; GSM317657     5  0.4242     0.4904 0.000 0.012 0.000 0.412 0.572 0.004
#&gt; GSM317667     6  0.0260     1.0000 0.000 0.000 0.000 0.008 0.000 0.992
#&gt; GSM317670     4  0.0603     0.6781 0.000 0.016 0.000 0.980 0.004 0.000
#&gt; GSM317674     1  0.0000     0.9036 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317675     1  0.0000     0.9036 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317677     1  0.1649     0.8805 0.936 0.000 0.000 0.016 0.040 0.008
#&gt; GSM317678     2  0.0260     0.8906 0.000 0.992 0.000 0.000 0.008 0.000
#&gt; GSM317687     5  0.4101     0.5284 0.008 0.000 0.000 0.352 0.632 0.008
#&gt; GSM317695     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317653     5  0.2745     0.4905 0.112 0.000 0.000 0.020 0.860 0.008
#&gt; GSM317656     1  0.0000     0.9036 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317658     5  0.5748     0.5595 0.296 0.064 0.000 0.064 0.576 0.000
#&gt; GSM317660     1  0.6224    -0.0893 0.368 0.000 0.340 0.004 0.288 0.000
#&gt; GSM317663     5  0.4242     0.4904 0.000 0.012 0.000 0.412 0.572 0.004
#&gt; GSM317664     1  0.0000     0.9036 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317665     1  0.3189     0.6318 0.760 0.000 0.000 0.004 0.236 0.000
#&gt; GSM317673     1  0.0260     0.9018 0.992 0.000 0.000 0.000 0.008 0.000
#&gt; GSM317686     6  0.0260     1.0000 0.000 0.000 0.000 0.008 0.000 0.992
#&gt; GSM317688     1  0.0937     0.8900 0.960 0.000 0.000 0.000 0.040 0.000
#&gt; GSM317690     4  0.0547     0.6752 0.000 0.020 0.000 0.980 0.000 0.000
#&gt; GSM317654     5  0.6089    -0.0537 0.372 0.000 0.156 0.012 0.456 0.004
#&gt; GSM317655     4  0.0603     0.6781 0.000 0.016 0.000 0.980 0.004 0.000
#&gt; GSM317659     1  0.2468     0.8267 0.880 0.000 0.000 0.016 0.096 0.008
#&gt; GSM317661     2  0.2325     0.8313 0.000 0.892 0.000 0.060 0.048 0.000
#&gt; GSM317662     2  0.0000     0.8941 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317668     4  0.3860    -0.0412 0.472 0.000 0.000 0.528 0.000 0.000
#&gt; GSM317669     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317671     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317676     5  0.4183     0.5190 0.008 0.000 0.000 0.380 0.604 0.008
#&gt; GSM317680     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317684     5  0.4278     0.5495 0.352 0.000 0.000 0.016 0.624 0.008
#&gt; GSM317685     1  0.1410     0.8842 0.944 0.000 0.000 0.008 0.044 0.004
#&gt; GSM317694     1  0.1003     0.8939 0.964 0.000 0.000 0.004 0.028 0.004
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-5-a').click(function(){
  $('#tab-MAD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-pam-signature_compare](figure_cola/MAD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-pam-collect-classes](figure_cola/MAD-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n disease.state(p) k
#> MAD:pam 50            0.438 2
#> MAD:pam 49            0.470 3
#> MAD:pam 48            0.671 4
#> MAD:pam 47            0.663 5
#> MAD:pam 41            0.720 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:mclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "mclust"]
# you can also extract it by
# res = res_list["MAD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-mclust-collect-plots](figure_cola/MAD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-mclust-select-partition-number](figure_cola/MAD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.984           0.934       0.969         0.3704 0.628   0.628
#> 3 3 0.907           0.898       0.951         0.7235 0.604   0.431
#> 4 4 0.796           0.830       0.926         0.0640 0.766   0.512
#> 5 5 0.792           0.751       0.882         0.0976 0.869   0.655
#> 6 6 0.793           0.760       0.885         0.0803 0.844   0.499
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-classes'>
<ul>
<li><a href='#tab-MAD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-mclust-get-classes-1'>
<p><a id='tab-MAD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     1  0.0000      0.977 1.000 0.000
#&gt; GSM317652     1  0.0000      0.977 1.000 0.000
#&gt; GSM317666     1  0.4022      0.905 0.920 0.080
#&gt; GSM317672     1  0.0376      0.976 0.996 0.004
#&gt; GSM317679     1  0.0000      0.977 1.000 0.000
#&gt; GSM317681     1  0.2948      0.934 0.948 0.052
#&gt; GSM317682     1  0.1184      0.969 0.984 0.016
#&gt; GSM317683     2  0.1414      0.949 0.020 0.980
#&gt; GSM317689     2  0.3431      0.919 0.064 0.936
#&gt; GSM317691     1  0.0376      0.976 0.996 0.004
#&gt; GSM317692     1  0.0376      0.976 0.996 0.004
#&gt; GSM317693     1  0.0000      0.977 1.000 0.000
#&gt; GSM317696     1  0.1414      0.966 0.980 0.020
#&gt; GSM317697     1  0.0000      0.977 1.000 0.000
#&gt; GSM317698     1  0.0000      0.977 1.000 0.000
#&gt; GSM317650     2  0.1414      0.949 0.020 0.980
#&gt; GSM317651     1  0.0000      0.977 1.000 0.000
#&gt; GSM317657     2  0.9850      0.277 0.428 0.572
#&gt; GSM317667     2  0.1414      0.949 0.020 0.980
#&gt; GSM317670     1  0.9833      0.180 0.576 0.424
#&gt; GSM317674     1  0.1414      0.966 0.980 0.020
#&gt; GSM317675     1  0.0000      0.977 1.000 0.000
#&gt; GSM317677     1  0.0376      0.976 0.996 0.004
#&gt; GSM317678     2  0.1414      0.949 0.020 0.980
#&gt; GSM317687     1  0.0376      0.976 0.996 0.004
#&gt; GSM317695     1  0.0000      0.977 1.000 0.000
#&gt; GSM317653     1  0.2603      0.941 0.956 0.044
#&gt; GSM317656     1  0.0938      0.971 0.988 0.012
#&gt; GSM317658     1  0.0672      0.974 0.992 0.008
#&gt; GSM317660     1  0.0000      0.977 1.000 0.000
#&gt; GSM317663     2  0.4690      0.886 0.100 0.900
#&gt; GSM317664     1  0.1414      0.966 0.980 0.020
#&gt; GSM317665     1  0.0000      0.977 1.000 0.000
#&gt; GSM317673     1  0.1414      0.966 0.980 0.020
#&gt; GSM317686     2  0.1414      0.949 0.020 0.980
#&gt; GSM317688     1  0.1414      0.966 0.980 0.020
#&gt; GSM317690     2  0.1414      0.949 0.020 0.980
#&gt; GSM317654     1  0.0000      0.977 1.000 0.000
#&gt; GSM317655     2  0.1414      0.949 0.020 0.980
#&gt; GSM317659     1  0.0376      0.976 0.996 0.004
#&gt; GSM317661     2  0.1414      0.949 0.020 0.980
#&gt; GSM317662     2  0.1414      0.949 0.020 0.980
#&gt; GSM317668     1  0.0376      0.976 0.996 0.004
#&gt; GSM317669     1  0.0000      0.977 1.000 0.000
#&gt; GSM317671     1  0.0000      0.977 1.000 0.000
#&gt; GSM317676     1  0.0672      0.974 0.992 0.008
#&gt; GSM317680     1  0.0000      0.977 1.000 0.000
#&gt; GSM317684     1  0.0376      0.976 0.996 0.004
#&gt; GSM317685     1  0.0000      0.977 1.000 0.000
#&gt; GSM317694     1  0.0376      0.976 0.996 0.004
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-1-a').click(function(){
  $('#tab-MAD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-2'>
<p><a id='tab-MAD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM317649     3  0.0829      0.970 0.012 0.004 0.984
#&gt; GSM317652     1  0.6516      0.107 0.516 0.004 0.480
#&gt; GSM317666     2  0.1031      0.983 0.000 0.976 0.024
#&gt; GSM317672     2  0.0424      0.984 0.008 0.992 0.000
#&gt; GSM317679     3  0.0829      0.970 0.012 0.004 0.984
#&gt; GSM317681     2  0.1015      0.982 0.008 0.980 0.012
#&gt; GSM317682     1  0.3192      0.822 0.888 0.000 0.112
#&gt; GSM317683     2  0.0237      0.984 0.000 0.996 0.004
#&gt; GSM317689     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM317691     1  0.0661      0.906 0.988 0.008 0.004
#&gt; GSM317692     1  0.5760      0.546 0.672 0.328 0.000
#&gt; GSM317693     1  0.0424      0.906 0.992 0.008 0.000
#&gt; GSM317696     1  0.0000      0.907 1.000 0.000 0.000
#&gt; GSM317697     1  0.0475      0.906 0.992 0.004 0.004
#&gt; GSM317698     1  0.0237      0.907 0.996 0.004 0.000
#&gt; GSM317650     2  0.0237      0.984 0.000 0.996 0.004
#&gt; GSM317651     1  0.5553      0.619 0.724 0.004 0.272
#&gt; GSM317657     2  0.0661      0.984 0.008 0.988 0.004
#&gt; GSM317667     2  0.1031      0.983 0.000 0.976 0.024
#&gt; GSM317670     2  0.0237      0.985 0.004 0.996 0.000
#&gt; GSM317674     1  0.0000      0.907 1.000 0.000 0.000
#&gt; GSM317675     1  0.0475      0.906 0.992 0.004 0.004
#&gt; GSM317677     1  0.0424      0.906 0.992 0.008 0.000
#&gt; GSM317678     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM317687     1  0.6033      0.518 0.660 0.336 0.004
#&gt; GSM317695     1  0.0237      0.906 0.996 0.000 0.004
#&gt; GSM317653     2  0.2680      0.929 0.008 0.924 0.068
#&gt; GSM317656     1  0.0000      0.907 1.000 0.000 0.000
#&gt; GSM317658     2  0.1129      0.976 0.020 0.976 0.004
#&gt; GSM317660     3  0.0829      0.970 0.012 0.004 0.984
#&gt; GSM317663     2  0.0892      0.983 0.000 0.980 0.020
#&gt; GSM317664     1  0.0000      0.907 1.000 0.000 0.000
#&gt; GSM317665     3  0.0829      0.970 0.012 0.004 0.984
#&gt; GSM317673     1  0.0000      0.907 1.000 0.000 0.000
#&gt; GSM317686     2  0.1031      0.983 0.000 0.976 0.024
#&gt; GSM317688     1  0.0000      0.907 1.000 0.000 0.000
#&gt; GSM317690     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM317654     3  0.4968      0.744 0.188 0.012 0.800
#&gt; GSM317655     2  0.0892      0.983 0.000 0.980 0.020
#&gt; GSM317659     1  0.0829      0.904 0.984 0.012 0.004
#&gt; GSM317661     2  0.0237      0.984 0.000 0.996 0.004
#&gt; GSM317662     2  0.0237      0.984 0.000 0.996 0.004
#&gt; GSM317668     1  0.4555      0.717 0.800 0.000 0.200
#&gt; GSM317669     3  0.0829      0.970 0.012 0.004 0.984
#&gt; GSM317671     3  0.0829      0.970 0.012 0.004 0.984
#&gt; GSM317676     2  0.1170      0.978 0.016 0.976 0.008
#&gt; GSM317680     3  0.0829      0.970 0.012 0.004 0.984
#&gt; GSM317684     1  0.0661      0.906 0.988 0.008 0.004
#&gt; GSM317685     1  0.0237      0.907 0.996 0.004 0.000
#&gt; GSM317694     1  0.0237      0.907 0.996 0.004 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-2-a').click(function(){
  $('#tab-MAD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-3'>
<p><a id='tab-MAD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     3  0.0000      0.924 0.000 0.000 1.000 0.000
#&gt; GSM317652     3  0.1637      0.875 0.060 0.000 0.940 0.000
#&gt; GSM317666     4  0.0188      1.000 0.000 0.004 0.000 0.996
#&gt; GSM317672     1  0.6310      0.204 0.512 0.428 0.060 0.000
#&gt; GSM317679     3  0.0000      0.924 0.000 0.000 1.000 0.000
#&gt; GSM317681     3  0.4010      0.803 0.064 0.100 0.836 0.000
#&gt; GSM317682     1  0.2530      0.797 0.888 0.000 0.112 0.000
#&gt; GSM317683     2  0.0000      0.994 0.000 1.000 0.000 0.000
#&gt; GSM317689     2  0.0817      0.963 0.024 0.976 0.000 0.000
#&gt; GSM317691     1  0.0188      0.873 0.996 0.000 0.000 0.004
#&gt; GSM317692     1  0.0188      0.873 0.996 0.004 0.000 0.000
#&gt; GSM317693     1  0.0188      0.873 0.996 0.000 0.000 0.004
#&gt; GSM317696     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM317697     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM317698     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM317650     2  0.0000      0.994 0.000 1.000 0.000 0.000
#&gt; GSM317651     1  0.4817      0.328 0.612 0.000 0.388 0.000
#&gt; GSM317657     1  0.5716      0.271 0.552 0.028 0.000 0.420
#&gt; GSM317667     4  0.0188      1.000 0.000 0.004 0.000 0.996
#&gt; GSM317670     1  0.4948      0.279 0.560 0.440 0.000 0.000
#&gt; GSM317674     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM317675     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM317677     1  0.0188      0.873 0.996 0.000 0.000 0.004
#&gt; GSM317678     2  0.0000      0.994 0.000 1.000 0.000 0.000
#&gt; GSM317687     1  0.4193      0.618 0.732 0.000 0.000 0.268
#&gt; GSM317695     1  0.1940      0.829 0.924 0.000 0.076 0.000
#&gt; GSM317653     3  0.5175      0.492 0.328 0.004 0.656 0.012
#&gt; GSM317656     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM317658     1  0.4193      0.622 0.732 0.268 0.000 0.000
#&gt; GSM317660     3  0.0000      0.924 0.000 0.000 1.000 0.000
#&gt; GSM317663     4  0.0188      1.000 0.000 0.004 0.000 0.996
#&gt; GSM317664     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM317665     3  0.0000      0.924 0.000 0.000 1.000 0.000
#&gt; GSM317673     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM317686     4  0.0188      1.000 0.000 0.004 0.000 0.996
#&gt; GSM317688     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM317690     2  0.0000      0.994 0.000 1.000 0.000 0.000
#&gt; GSM317654     3  0.0707      0.913 0.020 0.000 0.980 0.000
#&gt; GSM317655     4  0.0188      1.000 0.000 0.004 0.000 0.996
#&gt; GSM317659     1  0.0188      0.873 0.996 0.000 0.000 0.004
#&gt; GSM317661     2  0.0000      0.994 0.000 1.000 0.000 0.000
#&gt; GSM317662     2  0.0000      0.994 0.000 1.000 0.000 0.000
#&gt; GSM317668     1  0.3610      0.717 0.800 0.000 0.200 0.000
#&gt; GSM317669     3  0.0000      0.924 0.000 0.000 1.000 0.000
#&gt; GSM317671     3  0.0000      0.924 0.000 0.000 1.000 0.000
#&gt; GSM317676     1  0.4564      0.528 0.672 0.000 0.000 0.328
#&gt; GSM317680     3  0.0000      0.924 0.000 0.000 1.000 0.000
#&gt; GSM317684     1  0.0188      0.873 0.996 0.000 0.000 0.004
#&gt; GSM317685     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM317694     1  0.0188      0.873 0.996 0.000 0.000 0.004
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-3-a').click(function(){
  $('#tab-MAD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-4'>
<p><a id='tab-MAD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     3  0.0000      0.973 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317652     3  0.0609      0.949 0.020 0.000 0.980 0.000 0.000
#&gt; GSM317666     4  0.0510      0.441 0.000 0.000 0.000 0.984 0.016
#&gt; GSM317672     2  0.6961      0.452 0.216 0.576 0.160 0.020 0.028
#&gt; GSM317679     3  0.0000      0.973 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317681     3  0.3634      0.770 0.000 0.136 0.820 0.040 0.004
#&gt; GSM317682     1  0.2488      0.764 0.872 0.000 0.124 0.000 0.004
#&gt; GSM317683     2  0.0162      0.833 0.000 0.996 0.000 0.000 0.004
#&gt; GSM317689     2  0.1399      0.812 0.000 0.952 0.000 0.020 0.028
#&gt; GSM317691     1  0.0290      0.862 0.992 0.000 0.000 0.000 0.008
#&gt; GSM317692     1  0.1106      0.847 0.964 0.000 0.000 0.024 0.012
#&gt; GSM317693     1  0.0000      0.864 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317696     1  0.0000      0.864 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317697     1  0.0000      0.864 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317698     1  0.0000      0.864 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317650     2  0.0162      0.833 0.000 0.996 0.000 0.000 0.004
#&gt; GSM317651     1  0.4283      0.237 0.544 0.000 0.456 0.000 0.000
#&gt; GSM317657     4  0.2928      0.480 0.004 0.032 0.000 0.872 0.092
#&gt; GSM317667     5  0.4268      1.000 0.000 0.000 0.000 0.444 0.556
#&gt; GSM317670     2  0.5464      0.183 0.424 0.528 0.000 0.020 0.028
#&gt; GSM317674     1  0.0000      0.864 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317675     1  0.0000      0.864 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317677     1  0.4201      0.516 0.592 0.000 0.000 0.000 0.408
#&gt; GSM317678     2  0.0000      0.833 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317687     4  0.5236      0.505 0.048 0.000 0.000 0.544 0.408
#&gt; GSM317695     1  0.0404      0.860 0.988 0.000 0.012 0.000 0.000
#&gt; GSM317653     4  0.6687      0.348 0.000 0.000 0.248 0.420 0.332
#&gt; GSM317656     1  0.0162      0.863 0.996 0.000 0.000 0.000 0.004
#&gt; GSM317658     1  0.5240      0.364 0.616 0.336 0.000 0.020 0.028
#&gt; GSM317660     3  0.0000      0.973 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317663     4  0.0000      0.452 0.000 0.000 0.000 1.000 0.000
#&gt; GSM317664     1  0.0000      0.864 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317665     3  0.0000      0.973 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317673     1  0.0000      0.864 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317686     5  0.4268      1.000 0.000 0.000 0.000 0.444 0.556
#&gt; GSM317688     1  0.0162      0.863 0.996 0.000 0.000 0.000 0.004
#&gt; GSM317690     2  0.0865      0.823 0.000 0.972 0.000 0.004 0.024
#&gt; GSM317654     3  0.0000      0.973 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317655     4  0.0404      0.446 0.000 0.000 0.000 0.988 0.012
#&gt; GSM317659     1  0.4464      0.504 0.584 0.000 0.000 0.008 0.408
#&gt; GSM317661     2  0.0162      0.833 0.000 0.996 0.000 0.000 0.004
#&gt; GSM317662     2  0.0162      0.833 0.000 0.996 0.000 0.000 0.004
#&gt; GSM317668     1  0.3210      0.713 0.788 0.000 0.212 0.000 0.000
#&gt; GSM317669     3  0.0000      0.973 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317671     3  0.0000      0.973 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317676     4  0.5236      0.505 0.048 0.000 0.000 0.544 0.408
#&gt; GSM317680     3  0.0000      0.973 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317684     1  0.4201      0.516 0.592 0.000 0.000 0.000 0.408
#&gt; GSM317685     1  0.0000      0.864 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317694     1  0.3143      0.740 0.796 0.000 0.000 0.000 0.204
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-4-a').click(function(){
  $('#tab-MAD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-5'>
<p><a id='tab-MAD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM317649     5  0.0000     0.8539 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM317652     5  0.0508     0.8446 0.012 0.000 0.000 0.000 0.984 0.004
#&gt; GSM317666     4  0.4252     0.6054 0.000 0.000 0.372 0.604 0.000 0.024
#&gt; GSM317672     6  0.2287     0.7056 0.012 0.036 0.000 0.000 0.048 0.904
#&gt; GSM317679     5  0.0000     0.8539 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM317681     5  0.3838     0.2573 0.000 0.000 0.000 0.000 0.552 0.448
#&gt; GSM317682     1  0.2135     0.7950 0.872 0.000 0.000 0.000 0.128 0.000
#&gt; GSM317683     2  0.0000     0.9773 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317689     6  0.2003     0.7184 0.000 0.116 0.000 0.000 0.000 0.884
#&gt; GSM317691     1  0.4843     0.5380 0.652 0.000 0.232 0.000 0.000 0.116
#&gt; GSM317692     6  0.3587     0.6091 0.140 0.000 0.068 0.000 0.000 0.792
#&gt; GSM317693     1  0.3354     0.7715 0.812 0.000 0.128 0.000 0.000 0.060
#&gt; GSM317696     1  0.0000     0.9140 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317697     1  0.1267     0.8828 0.940 0.000 0.000 0.000 0.000 0.060
#&gt; GSM317698     1  0.0146     0.9124 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM317650     2  0.0000     0.9773 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317651     5  0.3930     0.2126 0.420 0.000 0.000 0.000 0.576 0.004
#&gt; GSM317657     6  0.4301     0.4128 0.000 0.000 0.240 0.064 0.000 0.696
#&gt; GSM317667     4  0.0000     0.6777 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317670     6  0.2300     0.7090 0.000 0.144 0.000 0.000 0.000 0.856
#&gt; GSM317674     1  0.0000     0.9140 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317675     1  0.0000     0.9140 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317677     3  0.2442     0.8126 0.144 0.000 0.852 0.000 0.000 0.004
#&gt; GSM317678     2  0.1444     0.9025 0.000 0.928 0.000 0.000 0.000 0.072
#&gt; GSM317687     3  0.0000     0.7919 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317695     1  0.0260     0.9101 0.992 0.000 0.000 0.000 0.008 0.000
#&gt; GSM317653     5  0.6195     0.2540 0.000 0.000 0.292 0.016 0.476 0.216
#&gt; GSM317656     1  0.0000     0.9140 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317658     6  0.2112     0.6904 0.088 0.016 0.000 0.000 0.000 0.896
#&gt; GSM317660     5  0.0000     0.8539 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM317663     4  0.5648     0.6215 0.000 0.000 0.240 0.536 0.000 0.224
#&gt; GSM317664     1  0.0000     0.9140 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317665     5  0.0146     0.8529 0.000 0.000 0.000 0.000 0.996 0.004
#&gt; GSM317673     1  0.0000     0.9140 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317686     4  0.0000     0.6777 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317688     1  0.0000     0.9140 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317690     6  0.3862     0.0723 0.000 0.476 0.000 0.000 0.000 0.524
#&gt; GSM317654     5  0.0146     0.8529 0.000 0.000 0.000 0.000 0.996 0.004
#&gt; GSM317655     4  0.5259     0.6902 0.000 0.000 0.240 0.600 0.000 0.160
#&gt; GSM317659     3  0.1588     0.8382 0.072 0.000 0.924 0.000 0.000 0.004
#&gt; GSM317661     2  0.0000     0.9773 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317662     2  0.0000     0.9773 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317668     1  0.3023     0.7131 0.784 0.000 0.000 0.000 0.212 0.004
#&gt; GSM317669     5  0.0000     0.8539 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM317671     5  0.0000     0.8539 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM317676     3  0.0458     0.7796 0.000 0.000 0.984 0.016 0.000 0.000
#&gt; GSM317680     5  0.0000     0.8539 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM317684     3  0.2668     0.7859 0.168 0.000 0.828 0.000 0.000 0.004
#&gt; GSM317685     1  0.0000     0.9140 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317694     1  0.2793     0.7347 0.800 0.000 0.200 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-5-a').click(function(){
  $('#tab-MAD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-mclust-signature_compare](figure_cola/MAD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-mclust-collect-classes](figure_cola/MAD-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> MAD:mclust 48            0.694 2
#> MAD:mclust 49            0.593 3
#> MAD:mclust 45            0.877 4
#> MAD:mclust 41            0.739 5
#> MAD:mclust 45            0.427 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "NMF"]
# you can also extract it by
# res = res_list["MAD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-NMF-collect-plots](figure_cola/MAD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-NMF-select-partition-number](figure_cola/MAD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.939       0.976         0.4660 0.530   0.530
#> 3 3 0.545           0.652       0.794         0.3880 0.735   0.538
#> 4 4 0.825           0.852       0.929         0.1408 0.747   0.415
#> 5 5 0.736           0.696       0.831         0.0716 0.922   0.715
#> 6 6 0.796           0.722       0.853         0.0409 0.945   0.756
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-classes'>
<ul>
<li><a href='#tab-MAD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-NMF-get-classes-1'>
<p><a id='tab-MAD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     1  0.0000      0.986 1.000 0.000
#&gt; GSM317652     1  0.0000      0.986 1.000 0.000
#&gt; GSM317666     2  0.0000      0.954 0.000 1.000
#&gt; GSM317672     2  0.0938      0.946 0.012 0.988
#&gt; GSM317679     1  0.1633      0.964 0.976 0.024
#&gt; GSM317681     2  0.0000      0.954 0.000 1.000
#&gt; GSM317682     1  0.0000      0.986 1.000 0.000
#&gt; GSM317683     2  0.0000      0.954 0.000 1.000
#&gt; GSM317689     2  0.0000      0.954 0.000 1.000
#&gt; GSM317691     1  0.0000      0.986 1.000 0.000
#&gt; GSM317692     1  0.9248      0.457 0.660 0.340
#&gt; GSM317693     1  0.0000      0.986 1.000 0.000
#&gt; GSM317696     1  0.0000      0.986 1.000 0.000
#&gt; GSM317697     1  0.0000      0.986 1.000 0.000
#&gt; GSM317698     1  0.0000      0.986 1.000 0.000
#&gt; GSM317650     2  0.0000      0.954 0.000 1.000
#&gt; GSM317651     1  0.0000      0.986 1.000 0.000
#&gt; GSM317657     2  0.0000      0.954 0.000 1.000
#&gt; GSM317667     2  0.0000      0.954 0.000 1.000
#&gt; GSM317670     2  0.1633      0.936 0.024 0.976
#&gt; GSM317674     1  0.0000      0.986 1.000 0.000
#&gt; GSM317675     1  0.0000      0.986 1.000 0.000
#&gt; GSM317677     1  0.0000      0.986 1.000 0.000
#&gt; GSM317678     2  0.0000      0.954 0.000 1.000
#&gt; GSM317687     1  0.0000      0.986 1.000 0.000
#&gt; GSM317695     1  0.0000      0.986 1.000 0.000
#&gt; GSM317653     2  0.8555      0.613 0.280 0.720
#&gt; GSM317656     1  0.0000      0.986 1.000 0.000
#&gt; GSM317658     2  0.9909      0.195 0.444 0.556
#&gt; GSM317660     1  0.2948      0.935 0.948 0.052
#&gt; GSM317663     2  0.0000      0.954 0.000 1.000
#&gt; GSM317664     1  0.0000      0.986 1.000 0.000
#&gt; GSM317665     1  0.0000      0.986 1.000 0.000
#&gt; GSM317673     1  0.0000      0.986 1.000 0.000
#&gt; GSM317686     2  0.0000      0.954 0.000 1.000
#&gt; GSM317688     1  0.0000      0.986 1.000 0.000
#&gt; GSM317690     2  0.0000      0.954 0.000 1.000
#&gt; GSM317654     1  0.0000      0.986 1.000 0.000
#&gt; GSM317655     2  0.0000      0.954 0.000 1.000
#&gt; GSM317659     1  0.0000      0.986 1.000 0.000
#&gt; GSM317661     2  0.0000      0.954 0.000 1.000
#&gt; GSM317662     2  0.0000      0.954 0.000 1.000
#&gt; GSM317668     1  0.0000      0.986 1.000 0.000
#&gt; GSM317669     1  0.0000      0.986 1.000 0.000
#&gt; GSM317671     1  0.0672      0.979 0.992 0.008
#&gt; GSM317676     1  0.0000      0.986 1.000 0.000
#&gt; GSM317680     1  0.0000      0.986 1.000 0.000
#&gt; GSM317684     1  0.0000      0.986 1.000 0.000
#&gt; GSM317685     1  0.0000      0.986 1.000 0.000
#&gt; GSM317694     1  0.0000      0.986 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-1-a').click(function(){
  $('#tab-MAD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-2'>
<p><a id='tab-MAD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM317649     1  0.0424      0.778 0.992 0.008 0.000
#&gt; GSM317652     1  0.0892      0.787 0.980 0.000 0.020
#&gt; GSM317666     3  0.5058      0.594 0.000 0.244 0.756
#&gt; GSM317672     2  0.3267      0.722 0.116 0.884 0.000
#&gt; GSM317679     1  0.2356      0.723 0.928 0.072 0.000
#&gt; GSM317681     2  0.4121      0.686 0.168 0.832 0.000
#&gt; GSM317682     1  0.0424      0.778 0.992 0.008 0.000
#&gt; GSM317683     2  0.0000      0.766 0.000 1.000 0.000
#&gt; GSM317689     2  0.0424      0.762 0.000 0.992 0.008
#&gt; GSM317691     3  0.4121      0.539 0.168 0.000 0.832
#&gt; GSM317692     1  0.9191      0.287 0.428 0.148 0.424
#&gt; GSM317693     3  0.3941      0.562 0.156 0.000 0.844
#&gt; GSM317696     1  0.5560      0.765 0.700 0.000 0.300
#&gt; GSM317697     1  0.5810      0.739 0.664 0.000 0.336
#&gt; GSM317698     1  0.5835      0.735 0.660 0.000 0.340
#&gt; GSM317650     2  0.0592      0.766 0.012 0.988 0.000
#&gt; GSM317651     1  0.5291      0.776 0.732 0.000 0.268
#&gt; GSM317657     3  0.5465      0.564 0.000 0.288 0.712
#&gt; GSM317667     3  0.5785      0.516 0.000 0.332 0.668
#&gt; GSM317670     2  0.6357      0.461 0.020 0.684 0.296
#&gt; GSM317674     1  0.5706      0.752 0.680 0.000 0.320
#&gt; GSM317675     1  0.5760      0.746 0.672 0.000 0.328
#&gt; GSM317677     3  0.2261      0.672 0.068 0.000 0.932
#&gt; GSM317678     2  0.0892      0.765 0.020 0.980 0.000
#&gt; GSM317687     3  0.0237      0.690 0.004 0.000 0.996
#&gt; GSM317695     1  0.1031      0.788 0.976 0.000 0.024
#&gt; GSM317653     2  0.6955     -0.276 0.016 0.496 0.488
#&gt; GSM317656     1  0.1031      0.788 0.976 0.000 0.024
#&gt; GSM317658     2  0.9020      0.261 0.220 0.560 0.220
#&gt; GSM317660     2  0.6308      0.274 0.492 0.508 0.000
#&gt; GSM317663     3  0.6126      0.427 0.000 0.400 0.600
#&gt; GSM317664     1  0.5497      0.768 0.708 0.000 0.292
#&gt; GSM317665     1  0.0424      0.778 0.992 0.008 0.000
#&gt; GSM317673     1  0.4974      0.784 0.764 0.000 0.236
#&gt; GSM317686     3  0.6286      0.291 0.000 0.464 0.536
#&gt; GSM317688     1  0.4605      0.788 0.796 0.000 0.204
#&gt; GSM317690     2  0.0892      0.755 0.000 0.980 0.020
#&gt; GSM317654     1  0.0892      0.779 0.980 0.000 0.020
#&gt; GSM317655     3  0.5988      0.478 0.000 0.368 0.632
#&gt; GSM317659     3  0.0747      0.689 0.016 0.000 0.984
#&gt; GSM317661     2  0.0000      0.766 0.000 1.000 0.000
#&gt; GSM317662     2  0.0000      0.766 0.000 1.000 0.000
#&gt; GSM317668     1  0.5465      0.770 0.712 0.000 0.288
#&gt; GSM317669     1  0.0424      0.778 0.992 0.008 0.000
#&gt; GSM317671     1  0.0892      0.773 0.980 0.020 0.000
#&gt; GSM317676     3  0.0237      0.690 0.004 0.000 0.996
#&gt; GSM317680     1  0.0424      0.778 0.992 0.008 0.000
#&gt; GSM317684     3  0.3482      0.607 0.128 0.000 0.872
#&gt; GSM317685     1  0.5678      0.755 0.684 0.000 0.316
#&gt; GSM317694     1  0.6204      0.623 0.576 0.000 0.424
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-2-a').click(function(){
  $('#tab-MAD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-3'>
<p><a id='tab-MAD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     3  0.0592      0.931 0.016 0.000 0.984 0.000
#&gt; GSM317652     3  0.2281      0.901 0.096 0.000 0.904 0.000
#&gt; GSM317666     4  0.0000      0.924 0.000 0.000 0.000 1.000
#&gt; GSM317672     2  0.0336      0.888 0.000 0.992 0.008 0.000
#&gt; GSM317679     3  0.0592      0.931 0.016 0.000 0.984 0.000
#&gt; GSM317681     3  0.3718      0.808 0.000 0.168 0.820 0.012
#&gt; GSM317682     1  0.5453      0.449 0.648 0.032 0.320 0.000
#&gt; GSM317683     2  0.0188      0.891 0.000 0.996 0.000 0.004
#&gt; GSM317689     2  0.0469      0.888 0.000 0.988 0.000 0.012
#&gt; GSM317691     1  0.0336      0.914 0.992 0.000 0.000 0.008
#&gt; GSM317692     1  0.1807      0.882 0.940 0.052 0.000 0.008
#&gt; GSM317693     1  0.0592      0.910 0.984 0.000 0.000 0.016
#&gt; GSM317696     1  0.0000      0.915 1.000 0.000 0.000 0.000
#&gt; GSM317697     1  0.0188      0.915 0.996 0.000 0.000 0.004
#&gt; GSM317698     1  0.0188      0.915 0.996 0.000 0.000 0.004
#&gt; GSM317650     2  0.0000      0.891 0.000 1.000 0.000 0.000
#&gt; GSM317651     3  0.2921      0.861 0.140 0.000 0.860 0.000
#&gt; GSM317657     4  0.5994      0.614 0.152 0.156 0.000 0.692
#&gt; GSM317667     4  0.0376      0.924 0.000 0.004 0.004 0.992
#&gt; GSM317670     2  0.5769      0.677 0.180 0.736 0.048 0.036
#&gt; GSM317674     1  0.0000      0.915 1.000 0.000 0.000 0.000
#&gt; GSM317675     1  0.0000      0.915 1.000 0.000 0.000 0.000
#&gt; GSM317677     1  0.1211      0.897 0.960 0.000 0.000 0.040
#&gt; GSM317678     2  0.0000      0.891 0.000 1.000 0.000 0.000
#&gt; GSM317687     4  0.1557      0.898 0.056 0.000 0.000 0.944
#&gt; GSM317695     3  0.2647      0.863 0.120 0.000 0.880 0.000
#&gt; GSM317653     4  0.3679      0.831 0.000 0.084 0.060 0.856
#&gt; GSM317656     1  0.4454      0.575 0.692 0.000 0.308 0.000
#&gt; GSM317658     2  0.5581      0.181 0.448 0.532 0.000 0.020
#&gt; GSM317660     3  0.1716      0.911 0.000 0.064 0.936 0.000
#&gt; GSM317663     4  0.0188      0.924 0.000 0.000 0.004 0.996
#&gt; GSM317664     1  0.0188      0.914 0.996 0.000 0.004 0.000
#&gt; GSM317665     3  0.1389      0.919 0.000 0.048 0.952 0.000
#&gt; GSM317673     1  0.0000      0.915 1.000 0.000 0.000 0.000
#&gt; GSM317686     4  0.0336      0.924 0.000 0.008 0.000 0.992
#&gt; GSM317688     1  0.0000      0.915 1.000 0.000 0.000 0.000
#&gt; GSM317690     2  0.1557      0.856 0.000 0.944 0.000 0.056
#&gt; GSM317654     3  0.1820      0.923 0.036 0.020 0.944 0.000
#&gt; GSM317655     4  0.0188      0.924 0.000 0.004 0.000 0.996
#&gt; GSM317659     1  0.4193      0.627 0.732 0.000 0.000 0.268
#&gt; GSM317661     2  0.0524      0.890 0.000 0.988 0.008 0.004
#&gt; GSM317662     2  0.0188      0.891 0.000 0.996 0.000 0.004
#&gt; GSM317668     1  0.4222      0.648 0.728 0.000 0.272 0.000
#&gt; GSM317669     3  0.0336      0.930 0.008 0.000 0.992 0.000
#&gt; GSM317671     3  0.0707      0.930 0.020 0.000 0.980 0.000
#&gt; GSM317676     4  0.1389      0.905 0.048 0.000 0.000 0.952
#&gt; GSM317680     3  0.0000      0.927 0.000 0.000 1.000 0.000
#&gt; GSM317684     1  0.1637      0.883 0.940 0.000 0.000 0.060
#&gt; GSM317685     1  0.0000      0.915 1.000 0.000 0.000 0.000
#&gt; GSM317694     1  0.0188      0.915 0.996 0.000 0.000 0.004
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-3-a').click(function(){
  $('#tab-MAD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-4'>
<p><a id='tab-MAD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     3  0.2280   0.681417 0.000 0.000 0.880 0.000 0.120
#&gt; GSM317652     3  0.5151  -0.000488 0.044 0.000 0.560 0.000 0.396
#&gt; GSM317666     4  0.0404   0.880315 0.000 0.000 0.000 0.988 0.012
#&gt; GSM317672     2  0.3336   0.710576 0.000 0.772 0.000 0.000 0.228
#&gt; GSM317679     3  0.2230   0.683614 0.000 0.000 0.884 0.000 0.116
#&gt; GSM317681     5  0.5612   0.519340 0.000 0.032 0.120 0.152 0.696
#&gt; GSM317682     1  0.3837   0.448391 0.692 0.000 0.000 0.000 0.308
#&gt; GSM317683     2  0.0000   0.880084 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317689     2  0.0000   0.880084 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317691     1  0.3767   0.787081 0.800 0.000 0.008 0.024 0.168
#&gt; GSM317692     1  0.0771   0.865583 0.976 0.004 0.000 0.000 0.020
#&gt; GSM317693     1  0.0000   0.870973 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317696     1  0.0451   0.871420 0.988 0.000 0.004 0.000 0.008
#&gt; GSM317697     1  0.0404   0.871889 0.988 0.000 0.000 0.000 0.012
#&gt; GSM317698     1  0.1270   0.867249 0.948 0.000 0.000 0.000 0.052
#&gt; GSM317650     2  0.0000   0.880084 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317651     5  0.5713   0.185260 0.416 0.000 0.084 0.000 0.500
#&gt; GSM317657     4  0.5421   0.709545 0.024 0.104 0.004 0.716 0.152
#&gt; GSM317667     4  0.1908   0.858545 0.000 0.000 0.000 0.908 0.092
#&gt; GSM317670     2  0.6910   0.428685 0.008 0.532 0.144 0.028 0.288
#&gt; GSM317674     1  0.2249   0.851578 0.896 0.000 0.008 0.000 0.096
#&gt; GSM317675     1  0.1638   0.863930 0.932 0.000 0.004 0.000 0.064
#&gt; GSM317677     1  0.5708   0.647049 0.640 0.000 0.016 0.092 0.252
#&gt; GSM317678     2  0.1671   0.858116 0.000 0.924 0.000 0.000 0.076
#&gt; GSM317687     4  0.2915   0.792398 0.116 0.000 0.000 0.860 0.024
#&gt; GSM317695     3  0.0000   0.668589 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317653     5  0.4350   0.126051 0.000 0.004 0.000 0.408 0.588
#&gt; GSM317656     3  0.5365   0.399401 0.204 0.000 0.664 0.000 0.132
#&gt; GSM317658     2  0.1282   0.857511 0.044 0.952 0.000 0.000 0.004
#&gt; GSM317660     5  0.4276   0.400994 0.000 0.004 0.380 0.000 0.616
#&gt; GSM317663     4  0.1544   0.869762 0.000 0.000 0.000 0.932 0.068
#&gt; GSM317664     1  0.4272   0.756584 0.752 0.000 0.052 0.000 0.196
#&gt; GSM317665     5  0.4249   0.301833 0.000 0.000 0.432 0.000 0.568
#&gt; GSM317673     1  0.0290   0.870885 0.992 0.000 0.000 0.000 0.008
#&gt; GSM317686     4  0.0671   0.880367 0.000 0.004 0.000 0.980 0.016
#&gt; GSM317688     1  0.0404   0.868380 0.988 0.000 0.000 0.000 0.012
#&gt; GSM317690     2  0.0880   0.872190 0.000 0.968 0.000 0.000 0.032
#&gt; GSM317654     5  0.4425   0.492210 0.000 0.000 0.296 0.024 0.680
#&gt; GSM317655     4  0.2377   0.860212 0.000 0.000 0.000 0.872 0.128
#&gt; GSM317659     1  0.5938   0.252077 0.512 0.000 0.000 0.376 0.112
#&gt; GSM317661     2  0.2848   0.801807 0.000 0.840 0.000 0.004 0.156
#&gt; GSM317662     2  0.0963   0.875489 0.000 0.964 0.000 0.000 0.036
#&gt; GSM317668     3  0.6596   0.292968 0.164 0.000 0.524 0.016 0.296
#&gt; GSM317669     3  0.2561   0.659016 0.000 0.000 0.856 0.000 0.144
#&gt; GSM317671     3  0.0703   0.658432 0.000 0.000 0.976 0.000 0.024
#&gt; GSM317676     4  0.1952   0.853976 0.004 0.000 0.000 0.912 0.084
#&gt; GSM317680     3  0.2230   0.683614 0.000 0.000 0.884 0.000 0.116
#&gt; GSM317684     1  0.0671   0.867861 0.980 0.000 0.000 0.004 0.016
#&gt; GSM317685     1  0.0703   0.865842 0.976 0.000 0.000 0.000 0.024
#&gt; GSM317694     1  0.2136   0.853973 0.904 0.000 0.008 0.000 0.088
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-4-a').click(function(){
  $('#tab-MAD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-5'>
<p><a id='tab-MAD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM317649     3  0.0909     0.9667 0.000 0.000 0.968 0.000 0.020 0.012
#&gt; GSM317652     5  0.5521     0.5554 0.032 0.000 0.264 0.000 0.608 0.096
#&gt; GSM317666     4  0.0632     0.7675 0.000 0.000 0.000 0.976 0.000 0.024
#&gt; GSM317672     2  0.2933     0.7359 0.000 0.796 0.000 0.000 0.200 0.004
#&gt; GSM317679     3  0.0146     0.9853 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM317681     5  0.4156     0.7577 0.004 0.016 0.032 0.120 0.796 0.032
#&gt; GSM317682     1  0.2437     0.8070 0.888 0.004 0.000 0.000 0.072 0.036
#&gt; GSM317683     2  0.0000     0.8659 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317689     2  0.1297     0.8585 0.000 0.948 0.000 0.000 0.012 0.040
#&gt; GSM317691     1  0.3172     0.7723 0.816 0.000 0.000 0.036 0.000 0.148
#&gt; GSM317692     1  0.2359     0.8236 0.904 0.024 0.000 0.004 0.016 0.052
#&gt; GSM317693     1  0.0935     0.8507 0.964 0.000 0.000 0.004 0.000 0.032
#&gt; GSM317696     1  0.0146     0.8563 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM317697     1  0.0603     0.8558 0.980 0.004 0.000 0.000 0.000 0.016
#&gt; GSM317698     1  0.2257     0.8311 0.876 0.000 0.000 0.000 0.008 0.116
#&gt; GSM317650     2  0.0363     0.8666 0.000 0.988 0.000 0.000 0.000 0.012
#&gt; GSM317651     5  0.2344     0.8153 0.048 0.000 0.004 0.000 0.896 0.052
#&gt; GSM317657     4  0.5337     0.3544 0.004 0.100 0.004 0.576 0.000 0.316
#&gt; GSM317667     4  0.1462     0.7552 0.000 0.000 0.000 0.936 0.056 0.008
#&gt; GSM317670     6  0.3124     0.4413 0.000 0.164 0.012 0.004 0.004 0.816
#&gt; GSM317674     1  0.3340     0.7620 0.784 0.000 0.004 0.000 0.016 0.196
#&gt; GSM317675     1  0.2714     0.8131 0.848 0.000 0.004 0.000 0.012 0.136
#&gt; GSM317677     1  0.5420     0.1711 0.476 0.000 0.000 0.080 0.012 0.432
#&gt; GSM317678     2  0.0363     0.8678 0.000 0.988 0.000 0.000 0.012 0.000
#&gt; GSM317687     4  0.2321     0.7207 0.052 0.000 0.000 0.900 0.008 0.040
#&gt; GSM317695     3  0.0000     0.9856 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317653     5  0.1531     0.8231 0.000 0.000 0.000 0.068 0.928 0.004
#&gt; GSM317656     6  0.7145     0.1905 0.160 0.000 0.292 0.000 0.128 0.420
#&gt; GSM317658     2  0.1152     0.8418 0.044 0.952 0.000 0.000 0.004 0.000
#&gt; GSM317660     5  0.2065     0.8423 0.000 0.004 0.052 0.000 0.912 0.032
#&gt; GSM317663     4  0.1434     0.7663 0.000 0.000 0.008 0.948 0.024 0.020
#&gt; GSM317664     1  0.4260     0.6556 0.692 0.000 0.024 0.000 0.016 0.268
#&gt; GSM317665     5  0.2540     0.8282 0.004 0.000 0.104 0.000 0.872 0.020
#&gt; GSM317673     1  0.1588     0.8547 0.924 0.000 0.000 0.000 0.004 0.072
#&gt; GSM317686     4  0.0777     0.7677 0.000 0.004 0.000 0.972 0.000 0.024
#&gt; GSM317688     1  0.1333     0.8570 0.944 0.000 0.000 0.000 0.008 0.048
#&gt; GSM317690     2  0.1152     0.8550 0.000 0.952 0.000 0.000 0.004 0.044
#&gt; GSM317654     5  0.1989     0.8456 0.000 0.000 0.052 0.028 0.916 0.004
#&gt; GSM317655     6  0.4653    -0.0695 0.000 0.000 0.000 0.360 0.052 0.588
#&gt; GSM317659     6  0.6144     0.1261 0.252 0.000 0.000 0.332 0.004 0.412
#&gt; GSM317661     2  0.4705     0.1150 0.000 0.484 0.000 0.000 0.472 0.044
#&gt; GSM317662     2  0.1644     0.8463 0.000 0.920 0.000 0.000 0.076 0.004
#&gt; GSM317668     6  0.2917     0.4934 0.032 0.000 0.072 0.000 0.028 0.868
#&gt; GSM317669     3  0.0458     0.9805 0.000 0.000 0.984 0.000 0.016 0.000
#&gt; GSM317671     3  0.0260     0.9827 0.000 0.000 0.992 0.000 0.000 0.008
#&gt; GSM317676     4  0.3991     0.1454 0.000 0.000 0.000 0.524 0.004 0.472
#&gt; GSM317680     3  0.0260     0.9849 0.000 0.000 0.992 0.000 0.000 0.008
#&gt; GSM317684     1  0.2263     0.8234 0.900 0.000 0.000 0.036 0.004 0.060
#&gt; GSM317685     1  0.0937     0.8539 0.960 0.000 0.000 0.000 0.000 0.040
#&gt; GSM317694     1  0.1818     0.8528 0.920 0.000 0.004 0.004 0.004 0.068
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-5-a').click(function(){
  $('#tab-MAD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-NMF-signature_compare](figure_cola/MAD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-NMF-collect-classes](figure_cola/MAD-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n disease.state(p) k
#> MAD:NMF 48            0.696 2
#> MAD:NMF 42            0.612 3
#> MAD:NMF 48            0.902 4
#> MAD:NMF 39            0.846 5
#> MAD:NMF 41            0.638 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:hclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "hclust"]
# you can also extract it by
# res = res_list["ATC:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-hclust-collect-plots](figure_cola/ATC-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-hclust-select-partition-number](figure_cola/ATC-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.483           0.805       0.903          0.322 0.754   0.754
#> 3 3 0.489           0.769       0.806          0.694 0.620   0.506
#> 4 4 0.509           0.716       0.815          0.126 0.993   0.984
#> 5 5 0.557           0.705       0.801          0.115 0.937   0.841
#> 6 6 0.619           0.710       0.763          0.132 0.860   0.588
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-classes'>
<ul>
<li><a href='#tab-ATC-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-hclust-get-classes-1'>
<p><a id='tab-ATC-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     1   0.000      0.880 1.000 0.000
#&gt; GSM317652     1   0.000      0.880 1.000 0.000
#&gt; GSM317666     1   0.866      0.686 0.712 0.288
#&gt; GSM317672     1   0.969      0.505 0.604 0.396
#&gt; GSM317679     1   0.000      0.880 1.000 0.000
#&gt; GSM317681     1   0.973      0.493 0.596 0.404
#&gt; GSM317682     1   0.634      0.802 0.840 0.160
#&gt; GSM317683     2   0.000      0.922 0.000 1.000
#&gt; GSM317689     1   0.971      0.497 0.600 0.400
#&gt; GSM317691     1   0.000      0.880 1.000 0.000
#&gt; GSM317692     1   0.866      0.653 0.712 0.288
#&gt; GSM317693     1   0.000      0.880 1.000 0.000
#&gt; GSM317696     1   0.000      0.880 1.000 0.000
#&gt; GSM317697     1   0.000      0.880 1.000 0.000
#&gt; GSM317698     1   0.000      0.880 1.000 0.000
#&gt; GSM317650     2   0.000      0.922 0.000 1.000
#&gt; GSM317651     1   0.634      0.802 0.840 0.160
#&gt; GSM317657     1   0.866      0.686 0.712 0.288
#&gt; GSM317667     2   0.000      0.922 0.000 1.000
#&gt; GSM317670     1   0.494      0.822 0.892 0.108
#&gt; GSM317674     1   0.000      0.880 1.000 0.000
#&gt; GSM317675     1   0.000      0.880 1.000 0.000
#&gt; GSM317677     1   0.000      0.880 1.000 0.000
#&gt; GSM317678     2   0.952      0.184 0.372 0.628
#&gt; GSM317687     1   0.000      0.880 1.000 0.000
#&gt; GSM317695     1   0.000      0.880 1.000 0.000
#&gt; GSM317653     1   0.973      0.493 0.596 0.404
#&gt; GSM317656     1   0.000      0.880 1.000 0.000
#&gt; GSM317658     1   0.760      0.754 0.780 0.220
#&gt; GSM317660     1   0.653      0.797 0.832 0.168
#&gt; GSM317663     1   0.866      0.686 0.712 0.288
#&gt; GSM317664     1   0.000      0.880 1.000 0.000
#&gt; GSM317665     1   0.634      0.802 0.840 0.160
#&gt; GSM317673     1   0.000      0.880 1.000 0.000
#&gt; GSM317686     2   0.000      0.922 0.000 1.000
#&gt; GSM317688     1   0.000      0.880 1.000 0.000
#&gt; GSM317690     1   0.881      0.671 0.700 0.300
#&gt; GSM317654     1   0.634      0.802 0.840 0.160
#&gt; GSM317655     1   0.866      0.686 0.712 0.288
#&gt; GSM317659     1   0.000      0.880 1.000 0.000
#&gt; GSM317661     2   0.000      0.922 0.000 1.000
#&gt; GSM317662     2   0.000      0.922 0.000 1.000
#&gt; GSM317668     1   0.000      0.880 1.000 0.000
#&gt; GSM317669     1   0.000      0.880 1.000 0.000
#&gt; GSM317671     1   0.000      0.880 1.000 0.000
#&gt; GSM317676     1   0.000      0.880 1.000 0.000
#&gt; GSM317680     1   0.000      0.880 1.000 0.000
#&gt; GSM317684     1   0.000      0.880 1.000 0.000
#&gt; GSM317685     1   0.000      0.880 1.000 0.000
#&gt; GSM317694     1   0.000      0.880 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-1-a').click(function(){
  $('#tab-ATC-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-2'>
<p><a id='tab-ATC-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM317649     1  0.0000      0.920 1.000 0.000 0.000
#&gt; GSM317652     1  0.0237      0.918 0.996 0.004 0.000
#&gt; GSM317666     2  0.3359      0.696 0.084 0.900 0.016
#&gt; GSM317672     2  0.5710      0.663 0.080 0.804 0.116
#&gt; GSM317679     1  0.0000      0.920 1.000 0.000 0.000
#&gt; GSM317681     2  0.4768      0.642 0.052 0.848 0.100
#&gt; GSM317682     2  0.6126      0.523 0.400 0.600 0.000
#&gt; GSM317683     3  0.4399      0.899 0.000 0.188 0.812
#&gt; GSM317689     2  0.5695      0.660 0.076 0.804 0.120
#&gt; GSM317691     1  0.3619      0.840 0.864 0.136 0.000
#&gt; GSM317692     2  0.7199      0.662 0.180 0.712 0.108
#&gt; GSM317693     1  0.3551      0.844 0.868 0.132 0.000
#&gt; GSM317696     1  0.0000      0.920 1.000 0.000 0.000
#&gt; GSM317697     1  0.3551      0.844 0.868 0.132 0.000
#&gt; GSM317698     1  0.0000      0.920 1.000 0.000 0.000
#&gt; GSM317650     3  0.4399      0.899 0.000 0.188 0.812
#&gt; GSM317651     2  0.6126      0.523 0.400 0.600 0.000
#&gt; GSM317657     2  0.3359      0.696 0.084 0.900 0.016
#&gt; GSM317667     3  0.1753      0.818 0.000 0.048 0.952
#&gt; GSM317670     2  0.6225      0.313 0.432 0.568 0.000
#&gt; GSM317674     1  0.0000      0.920 1.000 0.000 0.000
#&gt; GSM317675     1  0.0000      0.920 1.000 0.000 0.000
#&gt; GSM317677     1  0.2878      0.872 0.904 0.096 0.000
#&gt; GSM317678     2  0.7580      0.206 0.056 0.604 0.340
#&gt; GSM317687     1  0.4931      0.673 0.768 0.232 0.000
#&gt; GSM317695     1  0.0000      0.920 1.000 0.000 0.000
#&gt; GSM317653     2  0.4768      0.642 0.052 0.848 0.100
#&gt; GSM317656     1  0.0000      0.920 1.000 0.000 0.000
#&gt; GSM317658     2  0.6168      0.442 0.412 0.588 0.000
#&gt; GSM317660     2  0.5948      0.577 0.360 0.640 0.000
#&gt; GSM317663     2  0.3359      0.696 0.084 0.900 0.016
#&gt; GSM317664     1  0.0000      0.920 1.000 0.000 0.000
#&gt; GSM317665     2  0.6126      0.523 0.400 0.600 0.000
#&gt; GSM317673     1  0.0000      0.920 1.000 0.000 0.000
#&gt; GSM317686     3  0.1753      0.818 0.000 0.048 0.952
#&gt; GSM317688     1  0.0000      0.920 1.000 0.000 0.000
#&gt; GSM317690     2  0.3765      0.690 0.084 0.888 0.028
#&gt; GSM317654     2  0.6126      0.523 0.400 0.600 0.000
#&gt; GSM317655     2  0.3359      0.696 0.084 0.900 0.016
#&gt; GSM317659     1  0.3038      0.867 0.896 0.104 0.000
#&gt; GSM317661     3  0.4399      0.899 0.000 0.188 0.812
#&gt; GSM317662     3  0.4399      0.899 0.000 0.188 0.812
#&gt; GSM317668     1  0.4399      0.754 0.812 0.188 0.000
#&gt; GSM317669     1  0.0000      0.920 1.000 0.000 0.000
#&gt; GSM317671     1  0.0000      0.920 1.000 0.000 0.000
#&gt; GSM317676     1  0.4931      0.673 0.768 0.232 0.000
#&gt; GSM317680     1  0.0000      0.920 1.000 0.000 0.000
#&gt; GSM317684     1  0.3038      0.867 0.896 0.104 0.000
#&gt; GSM317685     1  0.0000      0.920 1.000 0.000 0.000
#&gt; GSM317694     1  0.2878      0.872 0.904 0.096 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-2-a').click(function(){
  $('#tab-ATC-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-3'>
<p><a id='tab-ATC-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     1  0.0000      0.852 1.000 0.000 0.000 0.000
#&gt; GSM317652     1  0.0188      0.852 0.996 0.000 0.000 0.004
#&gt; GSM317666     4  0.2469      0.622 0.000 0.000 0.108 0.892
#&gt; GSM317672     4  0.2859      0.620 0.008 0.112 0.000 0.880
#&gt; GSM317679     1  0.1211      0.836 0.960 0.000 0.040 0.000
#&gt; GSM317681     4  0.5399      0.595 0.052 0.140 0.036 0.772
#&gt; GSM317682     4  0.7018      0.465 0.368 0.024 0.068 0.540
#&gt; GSM317683     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM317689     4  0.2773      0.617 0.004 0.116 0.000 0.880
#&gt; GSM317691     1  0.5585      0.706 0.712 0.000 0.084 0.204
#&gt; GSM317692     4  0.4549      0.618 0.100 0.096 0.000 0.804
#&gt; GSM317693     1  0.5448      0.717 0.724 0.000 0.080 0.196
#&gt; GSM317696     1  0.0000      0.852 1.000 0.000 0.000 0.000
#&gt; GSM317697     1  0.5448      0.717 0.724 0.000 0.080 0.196
#&gt; GSM317698     1  0.0921      0.847 0.972 0.000 0.028 0.000
#&gt; GSM317650     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM317651     4  0.7018      0.465 0.368 0.024 0.068 0.540
#&gt; GSM317657     4  0.2469      0.622 0.000 0.000 0.108 0.892
#&gt; GSM317667     3  0.4868      1.000 0.000 0.304 0.684 0.012
#&gt; GSM317670     4  0.7249      0.226 0.348 0.000 0.156 0.496
#&gt; GSM317674     1  0.0188      0.852 0.996 0.000 0.004 0.000
#&gt; GSM317675     1  0.0000      0.852 1.000 0.000 0.000 0.000
#&gt; GSM317677     1  0.5208      0.740 0.748 0.000 0.080 0.172
#&gt; GSM317678     4  0.4819      0.286 0.004 0.344 0.000 0.652
#&gt; GSM317687     1  0.6383      0.523 0.612 0.000 0.096 0.292
#&gt; GSM317695     1  0.1211      0.836 0.960 0.000 0.040 0.000
#&gt; GSM317653     4  0.5399      0.595 0.052 0.140 0.036 0.772
#&gt; GSM317656     1  0.0000      0.852 1.000 0.000 0.000 0.000
#&gt; GSM317658     4  0.6462      0.383 0.332 0.000 0.088 0.580
#&gt; GSM317660     4  0.7519      0.532 0.280 0.032 0.120 0.568
#&gt; GSM317663     4  0.2469      0.622 0.000 0.000 0.108 0.892
#&gt; GSM317664     1  0.0000      0.852 1.000 0.000 0.000 0.000
#&gt; GSM317665     4  0.7018      0.465 0.368 0.024 0.068 0.540
#&gt; GSM317673     1  0.0921      0.847 0.972 0.000 0.028 0.000
#&gt; GSM317686     3  0.4868      1.000 0.000 0.304 0.684 0.012
#&gt; GSM317688     1  0.0817      0.850 0.976 0.000 0.024 0.000
#&gt; GSM317690     4  0.3695      0.600 0.000 0.016 0.156 0.828
#&gt; GSM317654     4  0.7018      0.465 0.368 0.024 0.068 0.540
#&gt; GSM317655     4  0.2469      0.622 0.000 0.000 0.108 0.892
#&gt; GSM317659     1  0.5292      0.738 0.744 0.000 0.088 0.168
#&gt; GSM317661     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM317662     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM317668     1  0.4222      0.652 0.728 0.000 0.000 0.272
#&gt; GSM317669     1  0.1211      0.836 0.960 0.000 0.040 0.000
#&gt; GSM317671     1  0.1211      0.836 0.960 0.000 0.040 0.000
#&gt; GSM317676     1  0.6383      0.523 0.612 0.000 0.096 0.292
#&gt; GSM317680     1  0.1211      0.836 0.960 0.000 0.040 0.000
#&gt; GSM317684     1  0.5292      0.738 0.744 0.000 0.088 0.168
#&gt; GSM317685     1  0.0000      0.852 1.000 0.000 0.000 0.000
#&gt; GSM317694     1  0.3636      0.772 0.820 0.000 0.008 0.172
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-3-a').click(function(){
  $('#tab-ATC-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-4'>
<p><a id='tab-ATC-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     1  0.0000      0.782 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317652     1  0.0880      0.778 0.968 0.000 0.032 0.000 0.000
#&gt; GSM317666     4  0.0162      0.715 0.000 0.000 0.000 0.996 0.004
#&gt; GSM317672     4  0.4098      0.652 0.000 0.064 0.156 0.780 0.000
#&gt; GSM317679     1  0.1792      0.744 0.916 0.000 0.000 0.000 0.084
#&gt; GSM317681     3  0.4337      0.490 0.000 0.052 0.744 0.204 0.000
#&gt; GSM317682     3  0.3305      0.792 0.224 0.000 0.776 0.000 0.000
#&gt; GSM317683     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317689     4  0.4159      0.651 0.000 0.068 0.156 0.776 0.000
#&gt; GSM317691     1  0.6151      0.610 0.604 0.000 0.176 0.208 0.012
#&gt; GSM317692     4  0.5634      0.607 0.092 0.052 0.152 0.704 0.000
#&gt; GSM317693     1  0.6097      0.619 0.612 0.000 0.176 0.200 0.012
#&gt; GSM317696     1  0.0000      0.782 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317697     1  0.6097      0.619 0.612 0.000 0.176 0.200 0.012
#&gt; GSM317698     1  0.0963      0.777 0.964 0.000 0.036 0.000 0.000
#&gt; GSM317650     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317651     3  0.3305      0.792 0.224 0.000 0.776 0.000 0.000
#&gt; GSM317657     4  0.0162      0.715 0.000 0.000 0.000 0.996 0.004
#&gt; GSM317667     5  0.3109      1.000 0.000 0.200 0.000 0.000 0.800
#&gt; GSM317670     4  0.7371      0.179 0.312 0.000 0.100 0.480 0.108
#&gt; GSM317674     1  0.0162      0.782 0.996 0.000 0.004 0.000 0.000
#&gt; GSM317675     1  0.0000      0.782 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317677     1  0.5949      0.641 0.632 0.000 0.180 0.176 0.012
#&gt; GSM317678     4  0.6088      0.336 0.000 0.296 0.156 0.548 0.000
#&gt; GSM317687     1  0.6680      0.425 0.496 0.000 0.184 0.308 0.012
#&gt; GSM317695     1  0.1792      0.744 0.916 0.000 0.000 0.000 0.084
#&gt; GSM317653     3  0.4337      0.490 0.000 0.052 0.744 0.204 0.000
#&gt; GSM317656     1  0.0000      0.782 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317658     4  0.7500      0.316 0.256 0.000 0.136 0.500 0.108
#&gt; GSM317660     3  0.2377      0.729 0.128 0.000 0.872 0.000 0.000
#&gt; GSM317663     4  0.0162      0.715 0.000 0.000 0.000 0.996 0.004
#&gt; GSM317664     1  0.0000      0.782 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317665     3  0.3305      0.792 0.224 0.000 0.776 0.000 0.000
#&gt; GSM317673     1  0.0963      0.777 0.964 0.000 0.036 0.000 0.000
#&gt; GSM317686     5  0.3109      1.000 0.000 0.200 0.000 0.000 0.800
#&gt; GSM317688     1  0.2127      0.753 0.892 0.000 0.108 0.000 0.000
#&gt; GSM317690     4  0.4121      0.605 0.000 0.016 0.072 0.808 0.104
#&gt; GSM317654     3  0.3305      0.792 0.224 0.000 0.776 0.000 0.000
#&gt; GSM317655     4  0.0162      0.715 0.000 0.000 0.000 0.996 0.004
#&gt; GSM317659     1  0.5980      0.638 0.628 0.000 0.184 0.176 0.012
#&gt; GSM317661     2  0.0162      0.994 0.000 0.996 0.004 0.000 0.000
#&gt; GSM317662     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317668     1  0.4397      0.612 0.696 0.000 0.028 0.276 0.000
#&gt; GSM317669     1  0.1792      0.744 0.916 0.000 0.000 0.000 0.084
#&gt; GSM317671     1  0.1792      0.744 0.916 0.000 0.000 0.000 0.084
#&gt; GSM317676     1  0.6680      0.425 0.496 0.000 0.184 0.308 0.012
#&gt; GSM317680     1  0.1792      0.744 0.916 0.000 0.000 0.000 0.084
#&gt; GSM317684     1  0.5980      0.638 0.628 0.000 0.184 0.176 0.012
#&gt; GSM317685     1  0.0000      0.782 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317694     1  0.4683      0.688 0.732 0.000 0.092 0.176 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-4-a').click(function(){
  $('#tab-ATC-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-5'>
<p><a id='tab-ATC-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM317649     3  0.0000     0.7946 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317652     3  0.1092     0.7833 0.020 0.000 0.960 0.000 0.020 0.000
#&gt; GSM317666     4  0.2562     0.7066 0.172 0.000 0.000 0.828 0.000 0.000
#&gt; GSM317672     4  0.3661     0.6512 0.020 0.012 0.000 0.768 0.200 0.000
#&gt; GSM317679     3  0.3877     0.7200 0.136 0.000 0.792 0.008 0.056 0.008
#&gt; GSM317681     5  0.2762     0.5300 0.000 0.000 0.000 0.196 0.804 0.000
#&gt; GSM317682     5  0.4566     0.7914 0.140 0.000 0.160 0.000 0.700 0.000
#&gt; GSM317683     2  0.0000     0.9980 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317689     4  0.3666     0.6506 0.016 0.016 0.000 0.768 0.200 0.000
#&gt; GSM317691     1  0.4917     0.8087 0.656 0.000 0.228 0.112 0.004 0.000
#&gt; GSM317692     4  0.5705     0.5681 0.120 0.000 0.052 0.628 0.200 0.000
#&gt; GSM317693     1  0.5128     0.8026 0.636 0.000 0.240 0.116 0.008 0.000
#&gt; GSM317696     3  0.1387     0.7923 0.068 0.000 0.932 0.000 0.000 0.000
#&gt; GSM317697     1  0.5128     0.8026 0.636 0.000 0.240 0.116 0.008 0.000
#&gt; GSM317698     3  0.2176     0.7727 0.080 0.000 0.896 0.000 0.024 0.000
#&gt; GSM317650     2  0.0000     0.9980 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317651     5  0.4566     0.7914 0.140 0.000 0.160 0.000 0.700 0.000
#&gt; GSM317657     4  0.2562     0.7066 0.172 0.000 0.000 0.828 0.000 0.000
#&gt; GSM317667     6  0.2527     1.0000 0.000 0.168 0.000 0.000 0.000 0.832
#&gt; GSM317670     1  0.7138     0.1946 0.416 0.000 0.064 0.340 0.020 0.160
#&gt; GSM317674     3  0.1531     0.7911 0.068 0.000 0.928 0.000 0.004 0.000
#&gt; GSM317675     3  0.1387     0.7923 0.068 0.000 0.932 0.000 0.000 0.000
#&gt; GSM317677     1  0.3302     0.8206 0.760 0.000 0.232 0.004 0.004 0.000
#&gt; GSM317678     4  0.5671     0.4780 0.004 0.244 0.000 0.552 0.200 0.000
#&gt; GSM317687     1  0.3254     0.7390 0.816 0.000 0.136 0.048 0.000 0.000
#&gt; GSM317695     3  0.3877     0.7200 0.136 0.000 0.792 0.008 0.056 0.008
#&gt; GSM317653     5  0.2762     0.5300 0.000 0.000 0.000 0.196 0.804 0.000
#&gt; GSM317656     3  0.0000     0.7946 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317658     4  0.6278     0.0445 0.364 0.000 0.008 0.452 0.016 0.160
#&gt; GSM317660     5  0.2762     0.6915 0.196 0.000 0.000 0.000 0.804 0.000
#&gt; GSM317663     4  0.2562     0.7066 0.172 0.000 0.000 0.828 0.000 0.000
#&gt; GSM317664     3  0.1387     0.7923 0.068 0.000 0.932 0.000 0.000 0.000
#&gt; GSM317665     5  0.4566     0.7914 0.140 0.000 0.160 0.000 0.700 0.000
#&gt; GSM317673     3  0.2176     0.7727 0.080 0.000 0.896 0.000 0.024 0.000
#&gt; GSM317686     6  0.2527     1.0000 0.000 0.168 0.000 0.000 0.000 0.832
#&gt; GSM317688     3  0.4924     0.3866 0.268 0.000 0.636 0.004 0.092 0.000
#&gt; GSM317690     4  0.4172     0.5650 0.032 0.016 0.000 0.772 0.020 0.160
#&gt; GSM317654     5  0.4566     0.7914 0.140 0.000 0.160 0.000 0.700 0.000
#&gt; GSM317655     4  0.2562     0.7066 0.172 0.000 0.000 0.828 0.000 0.000
#&gt; GSM317659     1  0.2969     0.8209 0.776 0.000 0.224 0.000 0.000 0.000
#&gt; GSM317661     2  0.0146     0.9941 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; GSM317662     2  0.0000     0.9980 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317668     3  0.5595    -0.3295 0.392 0.000 0.464 0.144 0.000 0.000
#&gt; GSM317669     3  0.3877     0.7200 0.136 0.000 0.792 0.008 0.056 0.008
#&gt; GSM317671     3  0.3877     0.7200 0.136 0.000 0.792 0.008 0.056 0.008
#&gt; GSM317676     1  0.3254     0.7390 0.816 0.000 0.136 0.048 0.000 0.000
#&gt; GSM317680     3  0.3877     0.7200 0.136 0.000 0.792 0.008 0.056 0.008
#&gt; GSM317684     1  0.2969     0.8209 0.776 0.000 0.224 0.000 0.000 0.000
#&gt; GSM317685     3  0.1387     0.7923 0.068 0.000 0.932 0.000 0.000 0.000
#&gt; GSM317694     1  0.3728     0.7128 0.652 0.000 0.344 0.004 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-5-a').click(function(){
  $('#tab-ATC-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-hclust-signature_compare](figure_cola/ATC-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-hclust-collect-classes](figure_cola/ATC-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> ATC:hclust 46            0.826 2
#> ATC:hclust 47            0.570 3
#> ATC:hclust 43            0.406 4
#> ATC:hclust 43            0.772 5
#> ATC:hclust 45            0.707 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "kmeans"]
# you can also extract it by
# res = res_list["ATC:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-kmeans-collect-plots](figure_cola/ATC-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-kmeans-select-partition-number](figure_cola/ATC-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.990       0.995         0.4478 0.556   0.556
#> 3 3 0.488           0.649       0.707         0.3560 1.000   1.000
#> 4 4 0.486           0.593       0.743         0.1482 0.684   0.463
#> 5 5 0.572           0.702       0.780         0.0948 0.878   0.622
#> 6 6 0.739           0.737       0.794         0.0573 1.000   1.000
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-kmeans-get-classes-1'>
<p><a id='tab-ATC-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     1   0.000      0.993 1.000 0.000
#&gt; GSM317652     1   0.000      0.993 1.000 0.000
#&gt; GSM317666     2   0.000      1.000 0.000 1.000
#&gt; GSM317672     2   0.000      1.000 0.000 1.000
#&gt; GSM317679     1   0.000      0.993 1.000 0.000
#&gt; GSM317681     2   0.000      1.000 0.000 1.000
#&gt; GSM317682     1   0.000      0.993 1.000 0.000
#&gt; GSM317683     2   0.000      1.000 0.000 1.000
#&gt; GSM317689     2   0.000      1.000 0.000 1.000
#&gt; GSM317691     1   0.000      0.993 1.000 0.000
#&gt; GSM317692     1   0.775      0.705 0.772 0.228
#&gt; GSM317693     1   0.000      0.993 1.000 0.000
#&gt; GSM317696     1   0.000      0.993 1.000 0.000
#&gt; GSM317697     1   0.000      0.993 1.000 0.000
#&gt; GSM317698     1   0.000      0.993 1.000 0.000
#&gt; GSM317650     2   0.000      1.000 0.000 1.000
#&gt; GSM317651     1   0.000      0.993 1.000 0.000
#&gt; GSM317657     2   0.000      1.000 0.000 1.000
#&gt; GSM317667     2   0.000      1.000 0.000 1.000
#&gt; GSM317670     1   0.000      0.993 1.000 0.000
#&gt; GSM317674     1   0.000      0.993 1.000 0.000
#&gt; GSM317675     1   0.000      0.993 1.000 0.000
#&gt; GSM317677     1   0.000      0.993 1.000 0.000
#&gt; GSM317678     2   0.000      1.000 0.000 1.000
#&gt; GSM317687     1   0.000      0.993 1.000 0.000
#&gt; GSM317695     1   0.000      0.993 1.000 0.000
#&gt; GSM317653     2   0.000      1.000 0.000 1.000
#&gt; GSM317656     1   0.000      0.993 1.000 0.000
#&gt; GSM317658     1   0.000      0.993 1.000 0.000
#&gt; GSM317660     1   0.000      0.993 1.000 0.000
#&gt; GSM317663     2   0.000      1.000 0.000 1.000
#&gt; GSM317664     1   0.000      0.993 1.000 0.000
#&gt; GSM317665     1   0.000      0.993 1.000 0.000
#&gt; GSM317673     1   0.000      0.993 1.000 0.000
#&gt; GSM317686     2   0.000      1.000 0.000 1.000
#&gt; GSM317688     1   0.000      0.993 1.000 0.000
#&gt; GSM317690     2   0.000      1.000 0.000 1.000
#&gt; GSM317654     1   0.000      0.993 1.000 0.000
#&gt; GSM317655     2   0.000      1.000 0.000 1.000
#&gt; GSM317659     1   0.000      0.993 1.000 0.000
#&gt; GSM317661     2   0.000      1.000 0.000 1.000
#&gt; GSM317662     2   0.000      1.000 0.000 1.000
#&gt; GSM317668     1   0.000      0.993 1.000 0.000
#&gt; GSM317669     1   0.000      0.993 1.000 0.000
#&gt; GSM317671     1   0.000      0.993 1.000 0.000
#&gt; GSM317676     1   0.000      0.993 1.000 0.000
#&gt; GSM317680     1   0.000      0.993 1.000 0.000
#&gt; GSM317684     1   0.000      0.993 1.000 0.000
#&gt; GSM317685     1   0.000      0.993 1.000 0.000
#&gt; GSM317694     1   0.000      0.993 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-2'>
<p><a id='tab-ATC-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2 p3
#&gt; GSM317649     1   0.553      0.582 0.704 0.000 NA
#&gt; GSM317652     1   0.254      0.707 0.920 0.000 NA
#&gt; GSM317666     2   0.412      0.702 0.000 0.832 NA
#&gt; GSM317672     2   0.558      0.570 0.024 0.772 NA
#&gt; GSM317679     1   0.553      0.582 0.704 0.000 NA
#&gt; GSM317681     2   0.525      0.763 0.000 0.736 NA
#&gt; GSM317682     1   0.496      0.707 0.792 0.008 NA
#&gt; GSM317683     2   0.601      0.755 0.000 0.628 NA
#&gt; GSM317689     2   0.000      0.743 0.000 1.000 NA
#&gt; GSM317691     1   0.897      0.559 0.564 0.240 NA
#&gt; GSM317692     1   0.979      0.321 0.384 0.380 NA
#&gt; GSM317693     1   0.897      0.559 0.564 0.240 NA
#&gt; GSM317696     1   0.000      0.728 1.000 0.000 NA
#&gt; GSM317697     1   0.865      0.589 0.600 0.204 NA
#&gt; GSM317698     1   0.271      0.728 0.912 0.000 NA
#&gt; GSM317650     2   0.601      0.755 0.000 0.628 NA
#&gt; GSM317651     1   0.571      0.698 0.768 0.028 NA
#&gt; GSM317657     2   0.543      0.584 0.000 0.716 NA
#&gt; GSM317667     2   0.629      0.744 0.000 0.532 NA
#&gt; GSM317670     1   0.955      0.384 0.436 0.368 NA
#&gt; GSM317674     1   0.000      0.728 1.000 0.000 NA
#&gt; GSM317675     1   0.000      0.728 1.000 0.000 NA
#&gt; GSM317677     1   0.750      0.648 0.696 0.140 NA
#&gt; GSM317678     2   0.502      0.770 0.000 0.760 NA
#&gt; GSM317687     1   0.956      0.383 0.432 0.372 NA
#&gt; GSM317695     1   0.543      0.587 0.716 0.000 NA
#&gt; GSM317653     2   0.493      0.624 0.000 0.768 NA
#&gt; GSM317656     1   0.236      0.709 0.928 0.000 NA
#&gt; GSM317658     1   0.955      0.384 0.436 0.368 NA
#&gt; GSM317660     1   0.754      0.659 0.640 0.068 NA
#&gt; GSM317663     2   0.400      0.703 0.000 0.840 NA
#&gt; GSM317664     1   0.000      0.728 1.000 0.000 NA
#&gt; GSM317665     1   0.645      0.688 0.684 0.024 NA
#&gt; GSM317673     1   0.271      0.728 0.912 0.000 NA
#&gt; GSM317686     2   0.629      0.744 0.000 0.532 NA
#&gt; GSM317688     1   0.175      0.730 0.952 0.000 NA
#&gt; GSM317690     2   0.271      0.765 0.000 0.912 NA
#&gt; GSM317654     1   0.637      0.693 0.704 0.028 NA
#&gt; GSM317655     2   0.355      0.717 0.000 0.868 NA
#&gt; GSM317659     1   0.897      0.559 0.564 0.240 NA
#&gt; GSM317661     2   0.601      0.755 0.000 0.628 NA
#&gt; GSM317662     2   0.601      0.755 0.000 0.628 NA
#&gt; GSM317668     1   0.000      0.728 1.000 0.000 NA
#&gt; GSM317669     1   0.553      0.582 0.704 0.000 NA
#&gt; GSM317671     1   0.553      0.582 0.704 0.000 NA
#&gt; GSM317676     1   0.958      0.377 0.428 0.372 NA
#&gt; GSM317680     1   0.553      0.582 0.704 0.000 NA
#&gt; GSM317684     1   0.826      0.618 0.636 0.168 NA
#&gt; GSM317685     1   0.000      0.728 1.000 0.000 NA
#&gt; GSM317694     1   0.000      0.728 1.000 0.000 NA
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-3'>
<p><a id='tab-ATC-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     3  0.4522     0.9923 0.320 0.000 0.680 0.000
#&gt; GSM317652     1  0.3649     0.4913 0.796 0.000 0.204 0.000
#&gt; GSM317666     4  0.2965     0.5817 0.000 0.072 0.036 0.892
#&gt; GSM317672     4  0.8202     0.3716 0.056 0.176 0.228 0.540
#&gt; GSM317679     3  0.4522     0.9923 0.320 0.000 0.680 0.000
#&gt; GSM317681     2  0.7955     0.0791 0.008 0.432 0.224 0.336
#&gt; GSM317682     1  0.5522     0.5472 0.716 0.000 0.204 0.080
#&gt; GSM317683     2  0.0000     0.7767 0.000 1.000 0.000 0.000
#&gt; GSM317689     4  0.5473     0.4118 0.000 0.324 0.032 0.644
#&gt; GSM317691     1  0.5487     0.1773 0.580 0.000 0.020 0.400
#&gt; GSM317692     4  0.5604     0.5890 0.160 0.000 0.116 0.724
#&gt; GSM317693     1  0.4642     0.5489 0.740 0.000 0.020 0.240
#&gt; GSM317696     1  0.2973     0.5762 0.856 0.000 0.144 0.000
#&gt; GSM317697     1  0.4284     0.6027 0.780 0.000 0.020 0.200
#&gt; GSM317698     1  0.1042     0.6464 0.972 0.000 0.008 0.020
#&gt; GSM317650     2  0.0000     0.7767 0.000 1.000 0.000 0.000
#&gt; GSM317651     1  0.6194     0.5278 0.668 0.000 0.200 0.132
#&gt; GSM317657     4  0.1743     0.6051 0.004 0.056 0.000 0.940
#&gt; GSM317667     2  0.5321     0.6651 0.000 0.748 0.112 0.140
#&gt; GSM317670     4  0.4313     0.5861 0.260 0.000 0.004 0.736
#&gt; GSM317674     1  0.2973     0.5762 0.856 0.000 0.144 0.000
#&gt; GSM317675     1  0.2973     0.5762 0.856 0.000 0.144 0.000
#&gt; GSM317677     1  0.3351     0.6302 0.844 0.000 0.008 0.148
#&gt; GSM317678     2  0.5579     0.4370 0.000 0.688 0.060 0.252
#&gt; GSM317687     4  0.5428     0.3952 0.380 0.000 0.020 0.600
#&gt; GSM317695     3  0.4624     0.9611 0.340 0.000 0.660 0.000
#&gt; GSM317653     4  0.8291     0.3359 0.052 0.200 0.224 0.524
#&gt; GSM317656     1  0.3486     0.5091 0.812 0.000 0.188 0.000
#&gt; GSM317658     4  0.4908     0.5580 0.292 0.000 0.016 0.692
#&gt; GSM317660     1  0.8096     0.3390 0.512 0.052 0.308 0.128
#&gt; GSM317663     4  0.2965     0.5817 0.000 0.072 0.036 0.892
#&gt; GSM317664     1  0.2973     0.5762 0.856 0.000 0.144 0.000
#&gt; GSM317665     1  0.6592     0.4503 0.600 0.000 0.284 0.116
#&gt; GSM317673     1  0.0376     0.6461 0.992 0.000 0.004 0.004
#&gt; GSM317686     2  0.5321     0.6651 0.000 0.748 0.112 0.140
#&gt; GSM317688     1  0.0469     0.6435 0.988 0.000 0.012 0.000
#&gt; GSM317690     4  0.5582     0.2677 0.000 0.400 0.024 0.576
#&gt; GSM317654     1  0.6394     0.4940 0.636 0.000 0.244 0.120
#&gt; GSM317655     4  0.3243     0.5669 0.000 0.088 0.036 0.876
#&gt; GSM317659     1  0.4675     0.5468 0.736 0.000 0.020 0.244
#&gt; GSM317661     2  0.0000     0.7767 0.000 1.000 0.000 0.000
#&gt; GSM317662     2  0.0336     0.7756 0.000 0.992 0.008 0.000
#&gt; GSM317668     1  0.2973     0.5762 0.856 0.000 0.144 0.000
#&gt; GSM317669     3  0.4522     0.9923 0.320 0.000 0.680 0.000
#&gt; GSM317671     3  0.4522     0.9923 0.320 0.000 0.680 0.000
#&gt; GSM317676     4  0.4837     0.4646 0.348 0.000 0.004 0.648
#&gt; GSM317680     3  0.4522     0.9923 0.320 0.000 0.680 0.000
#&gt; GSM317684     1  0.4079     0.6199 0.800 0.000 0.020 0.180
#&gt; GSM317685     1  0.2973     0.5762 0.856 0.000 0.144 0.000
#&gt; GSM317694     1  0.3324     0.5834 0.852 0.000 0.136 0.012
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-4'>
<p><a id='tab-ATC-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     3  0.2439      0.998 0.120 0.000 0.876 0.004 0.000
#&gt; GSM317652     1  0.3171      0.752 0.816 0.000 0.176 0.008 0.000
#&gt; GSM317666     4  0.2359      0.646 0.000 0.060 0.000 0.904 0.036
#&gt; GSM317672     5  0.2746      0.656 0.008 0.000 0.008 0.112 0.872
#&gt; GSM317679     3  0.2280      0.999 0.120 0.000 0.880 0.000 0.000
#&gt; GSM317681     5  0.3459      0.589 0.000 0.052 0.000 0.116 0.832
#&gt; GSM317682     5  0.3838      0.737 0.280 0.000 0.004 0.000 0.716
#&gt; GSM317683     2  0.2497      0.796 0.000 0.880 0.004 0.004 0.112
#&gt; GSM317689     4  0.5976      0.429 0.000 0.228 0.012 0.620 0.140
#&gt; GSM317691     1  0.5484      0.392 0.640 0.000 0.000 0.240 0.120
#&gt; GSM317692     5  0.5100      0.450 0.056 0.000 0.004 0.288 0.652
#&gt; GSM317693     1  0.4871      0.573 0.732 0.000 0.004 0.144 0.120
#&gt; GSM317696     1  0.2773      0.763 0.836 0.000 0.164 0.000 0.000
#&gt; GSM317697     1  0.4557      0.604 0.760 0.000 0.004 0.132 0.104
#&gt; GSM317698     1  0.0579      0.762 0.984 0.000 0.008 0.000 0.008
#&gt; GSM317650     2  0.2497      0.796 0.000 0.880 0.004 0.004 0.112
#&gt; GSM317651     5  0.3508      0.757 0.252 0.000 0.000 0.000 0.748
#&gt; GSM317657     4  0.2409      0.665 0.020 0.012 0.000 0.908 0.060
#&gt; GSM317667     2  0.5252      0.622 0.000 0.724 0.096 0.152 0.028
#&gt; GSM317670     4  0.4675      0.627 0.196 0.000 0.008 0.736 0.060
#&gt; GSM317674     1  0.2732      0.764 0.840 0.000 0.160 0.000 0.000
#&gt; GSM317675     1  0.2773      0.763 0.836 0.000 0.164 0.000 0.000
#&gt; GSM317677     1  0.2727      0.686 0.868 0.000 0.000 0.116 0.016
#&gt; GSM317678     2  0.6575      0.359 0.000 0.484 0.012 0.152 0.352
#&gt; GSM317687     4  0.6076      0.473 0.320 0.000 0.000 0.536 0.144
#&gt; GSM317695     3  0.2280      0.999 0.120 0.000 0.880 0.000 0.000
#&gt; GSM317653     5  0.2136      0.675 0.008 0.000 0.000 0.088 0.904
#&gt; GSM317656     1  0.3381      0.747 0.808 0.000 0.176 0.016 0.000
#&gt; GSM317658     4  0.5807      0.542 0.300 0.000 0.012 0.600 0.088
#&gt; GSM317660     5  0.3612      0.765 0.172 0.000 0.028 0.000 0.800
#&gt; GSM317663     4  0.2209      0.646 0.000 0.056 0.000 0.912 0.032
#&gt; GSM317664     1  0.2773      0.763 0.836 0.000 0.164 0.000 0.000
#&gt; GSM317665     5  0.4113      0.763 0.232 0.000 0.028 0.000 0.740
#&gt; GSM317673     1  0.2104      0.768 0.916 0.000 0.060 0.000 0.024
#&gt; GSM317686     2  0.5252      0.622 0.000 0.724 0.096 0.152 0.028
#&gt; GSM317688     1  0.3135      0.762 0.868 0.000 0.088 0.020 0.024
#&gt; GSM317690     4  0.5818      0.408 0.000 0.264 0.012 0.620 0.104
#&gt; GSM317654     5  0.3878      0.763 0.236 0.000 0.016 0.000 0.748
#&gt; GSM317655     4  0.2209      0.646 0.000 0.056 0.000 0.912 0.032
#&gt; GSM317659     1  0.4835      0.560 0.724 0.000 0.000 0.156 0.120
#&gt; GSM317661     2  0.2338      0.796 0.000 0.884 0.000 0.004 0.112
#&gt; GSM317662     2  0.2722      0.794 0.000 0.868 0.008 0.004 0.120
#&gt; GSM317668     1  0.3211      0.758 0.824 0.000 0.164 0.008 0.004
#&gt; GSM317669     3  0.2439      0.998 0.120 0.000 0.876 0.004 0.000
#&gt; GSM317671     3  0.2280      0.999 0.120 0.000 0.880 0.000 0.000
#&gt; GSM317676     4  0.5905      0.513 0.292 0.000 0.000 0.572 0.136
#&gt; GSM317680     3  0.2280      0.999 0.120 0.000 0.880 0.000 0.000
#&gt; GSM317684     1  0.4458      0.609 0.760 0.000 0.000 0.120 0.120
#&gt; GSM317685     1  0.2773      0.763 0.836 0.000 0.164 0.000 0.000
#&gt; GSM317694     1  0.3204      0.763 0.860 0.000 0.100 0.024 0.016
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-5'>
<p><a id='tab-ATC-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM317649     3  0.0777      0.996 0.024 0.000 0.972 0.000 0.000 NA
#&gt; GSM317652     1  0.3483      0.753 0.820 0.000 0.120 0.000 0.020 NA
#&gt; GSM317666     4  0.1010      0.712 0.000 0.000 0.000 0.960 0.004 NA
#&gt; GSM317672     5  0.4429      0.748 0.000 0.016 0.000 0.080 0.736 NA
#&gt; GSM317679     3  0.0547      0.995 0.020 0.000 0.980 0.000 0.000 NA
#&gt; GSM317681     5  0.4260      0.776 0.000 0.036 0.000 0.072 0.772 NA
#&gt; GSM317682     5  0.1918      0.837 0.088 0.000 0.000 0.000 0.904 NA
#&gt; GSM317683     2  0.0000      0.783 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM317689     4  0.4952      0.611 0.000 0.084 0.000 0.688 0.028 NA
#&gt; GSM317691     1  0.5892      0.491 0.572 0.000 0.000 0.068 0.076 NA
#&gt; GSM317692     5  0.5476      0.574 0.008 0.000 0.000 0.136 0.580 NA
#&gt; GSM317693     1  0.5016      0.575 0.636 0.000 0.000 0.016 0.072 NA
#&gt; GSM317696     1  0.2092      0.775 0.876 0.000 0.124 0.000 0.000 NA
#&gt; GSM317697     1  0.4707      0.605 0.672 0.000 0.000 0.012 0.064 NA
#&gt; GSM317698     1  0.0363      0.773 0.988 0.000 0.000 0.000 0.012 NA
#&gt; GSM317650     2  0.0000      0.783 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM317651     5  0.1858      0.838 0.076 0.000 0.000 0.000 0.912 NA
#&gt; GSM317657     4  0.1686      0.729 0.008 0.004 0.000 0.932 0.004 NA
#&gt; GSM317667     2  0.5924      0.582 0.000 0.496 0.016 0.088 0.016 NA
#&gt; GSM317670     4  0.5019      0.687 0.068 0.000 0.004 0.664 0.020 NA
#&gt; GSM317674     1  0.2003      0.778 0.884 0.000 0.116 0.000 0.000 NA
#&gt; GSM317675     1  0.2092      0.775 0.876 0.000 0.124 0.000 0.000 NA
#&gt; GSM317677     1  0.3608      0.655 0.736 0.000 0.000 0.004 0.012 NA
#&gt; GSM317678     2  0.6282      0.386 0.000 0.580 0.000 0.108 0.116 NA
#&gt; GSM317687     4  0.6624      0.520 0.140 0.000 0.000 0.488 0.080 NA
#&gt; GSM317695     3  0.0632      0.996 0.024 0.000 0.976 0.000 0.000 NA
#&gt; GSM317653     5  0.3075      0.809 0.000 0.016 0.000 0.040 0.852 NA
#&gt; GSM317656     1  0.2972      0.760 0.836 0.000 0.128 0.000 0.000 NA
#&gt; GSM317658     4  0.6448      0.564 0.196 0.000 0.004 0.476 0.028 NA
#&gt; GSM317660     5  0.1219      0.845 0.048 0.000 0.004 0.000 0.948 NA
#&gt; GSM317663     4  0.0146      0.712 0.000 0.004 0.000 0.996 0.000 NA
#&gt; GSM317664     1  0.2092      0.775 0.876 0.000 0.124 0.000 0.000 NA
#&gt; GSM317665     5  0.1531      0.845 0.068 0.000 0.004 0.000 0.928 NA
#&gt; GSM317673     1  0.1801      0.771 0.924 0.000 0.016 0.000 0.056 NA
#&gt; GSM317686     2  0.5924      0.582 0.000 0.496 0.016 0.088 0.016 NA
#&gt; GSM317688     1  0.3526      0.730 0.828 0.000 0.028 0.000 0.056 NA
#&gt; GSM317690     4  0.4805      0.637 0.000 0.096 0.004 0.716 0.020 NA
#&gt; GSM317654     5  0.1728      0.841 0.064 0.000 0.004 0.000 0.924 NA
#&gt; GSM317655     4  0.0260      0.711 0.000 0.008 0.000 0.992 0.000 NA
#&gt; GSM317659     1  0.5294      0.557 0.612 0.000 0.000 0.028 0.072 NA
#&gt; GSM317661     2  0.0000      0.783 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM317662     2  0.0508      0.781 0.000 0.984 0.000 0.000 0.004 NA
#&gt; GSM317668     1  0.2667      0.771 0.852 0.000 0.128 0.000 0.000 NA
#&gt; GSM317669     3  0.0777      0.996 0.024 0.000 0.972 0.000 0.000 NA
#&gt; GSM317671     3  0.0547      0.995 0.020 0.000 0.980 0.000 0.000 NA
#&gt; GSM317676     4  0.6162      0.550 0.128 0.000 0.000 0.532 0.048 NA
#&gt; GSM317680     3  0.0632      0.996 0.024 0.000 0.976 0.000 0.000 NA
#&gt; GSM317684     1  0.5015      0.575 0.628 0.000 0.000 0.012 0.076 NA
#&gt; GSM317685     1  0.2445      0.775 0.868 0.000 0.120 0.000 0.004 NA
#&gt; GSM317694     1  0.3370      0.765 0.828 0.000 0.072 0.000 0.008 NA
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-kmeans-signature_compare](figure_cola/ATC-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-kmeans-collect-classes](figure_cola/ATC-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> ATC:kmeans 50            0.878 2
#> ATC:kmeans 45            0.806 3
#> ATC:kmeans 37            0.924 4
#> ATC:kmeans 44            0.907 5
#> ATC:kmeans 48            0.844 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "skmeans"]
# you can also extract it by
# res = res_list["ATC:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-skmeans-collect-plots](figure_cola/ATC-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-skmeans-select-partition-number](figure_cola/ATC-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.973       0.990         0.4856 0.519   0.519
#> 3 3 0.819           0.826       0.907         0.3550 0.806   0.626
#> 4 4 0.642           0.689       0.794         0.1213 0.847   0.585
#> 5 5 0.649           0.576       0.758         0.0657 0.869   0.562
#> 6 6 0.711           0.611       0.796         0.0451 0.936   0.704
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-skmeans-get-classes-1'>
<p><a id='tab-ATC-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     1  0.0000      0.984 1.000 0.000
#&gt; GSM317652     1  0.0000      0.984 1.000 0.000
#&gt; GSM317666     2  0.0000      1.000 0.000 1.000
#&gt; GSM317672     2  0.0000      1.000 0.000 1.000
#&gt; GSM317679     1  0.0000      0.984 1.000 0.000
#&gt; GSM317681     2  0.0000      1.000 0.000 1.000
#&gt; GSM317682     1  0.0000      0.984 1.000 0.000
#&gt; GSM317683     2  0.0000      1.000 0.000 1.000
#&gt; GSM317689     2  0.0000      1.000 0.000 1.000
#&gt; GSM317691     1  0.0000      0.984 1.000 0.000
#&gt; GSM317692     2  0.0000      1.000 0.000 1.000
#&gt; GSM317693     1  0.0000      0.984 1.000 0.000
#&gt; GSM317696     1  0.0000      0.984 1.000 0.000
#&gt; GSM317697     1  0.0000      0.984 1.000 0.000
#&gt; GSM317698     1  0.0000      0.984 1.000 0.000
#&gt; GSM317650     2  0.0000      1.000 0.000 1.000
#&gt; GSM317651     1  0.0000      0.984 1.000 0.000
#&gt; GSM317657     2  0.0000      1.000 0.000 1.000
#&gt; GSM317667     2  0.0000      1.000 0.000 1.000
#&gt; GSM317670     2  0.0000      1.000 0.000 1.000
#&gt; GSM317674     1  0.0000      0.984 1.000 0.000
#&gt; GSM317675     1  0.0000      0.984 1.000 0.000
#&gt; GSM317677     1  0.0000      0.984 1.000 0.000
#&gt; GSM317678     2  0.0000      1.000 0.000 1.000
#&gt; GSM317687     1  0.0938      0.973 0.988 0.012
#&gt; GSM317695     1  0.0000      0.984 1.000 0.000
#&gt; GSM317653     2  0.0000      1.000 0.000 1.000
#&gt; GSM317656     1  0.0000      0.984 1.000 0.000
#&gt; GSM317658     2  0.0000      1.000 0.000 1.000
#&gt; GSM317660     1  0.9954      0.148 0.540 0.460
#&gt; GSM317663     2  0.0000      1.000 0.000 1.000
#&gt; GSM317664     1  0.0000      0.984 1.000 0.000
#&gt; GSM317665     1  0.0000      0.984 1.000 0.000
#&gt; GSM317673     1  0.0000      0.984 1.000 0.000
#&gt; GSM317686     2  0.0000      1.000 0.000 1.000
#&gt; GSM317688     1  0.0000      0.984 1.000 0.000
#&gt; GSM317690     2  0.0000      1.000 0.000 1.000
#&gt; GSM317654     1  0.0000      0.984 1.000 0.000
#&gt; GSM317655     2  0.0000      1.000 0.000 1.000
#&gt; GSM317659     1  0.0000      0.984 1.000 0.000
#&gt; GSM317661     2  0.0000      1.000 0.000 1.000
#&gt; GSM317662     2  0.0000      1.000 0.000 1.000
#&gt; GSM317668     1  0.0000      0.984 1.000 0.000
#&gt; GSM317669     1  0.0000      0.984 1.000 0.000
#&gt; GSM317671     1  0.0000      0.984 1.000 0.000
#&gt; GSM317676     1  0.0376      0.981 0.996 0.004
#&gt; GSM317680     1  0.0000      0.984 1.000 0.000
#&gt; GSM317684     1  0.0000      0.984 1.000 0.000
#&gt; GSM317685     1  0.0000      0.984 1.000 0.000
#&gt; GSM317694     1  0.0000      0.984 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-2'>
<p><a id='tab-ATC-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM317649     3  0.0424     0.8995 0.008 0.000 0.992
#&gt; GSM317652     3  0.0424     0.8995 0.008 0.000 0.992
#&gt; GSM317666     2  0.1753     0.9631 0.048 0.952 0.000
#&gt; GSM317672     2  0.0237     0.9801 0.000 0.996 0.004
#&gt; GSM317679     3  0.0424     0.8995 0.008 0.000 0.992
#&gt; GSM317681     2  0.0424     0.9786 0.000 0.992 0.008
#&gt; GSM317682     3  0.6154    -0.0665 0.408 0.000 0.592
#&gt; GSM317683     2  0.0000     0.9813 0.000 1.000 0.000
#&gt; GSM317689     2  0.0000     0.9813 0.000 1.000 0.000
#&gt; GSM317691     1  0.0000     0.7526 1.000 0.000 0.000
#&gt; GSM317692     2  0.0000     0.9813 0.000 1.000 0.000
#&gt; GSM317693     1  0.1529     0.7763 0.960 0.000 0.040
#&gt; GSM317696     1  0.5926     0.6845 0.644 0.000 0.356
#&gt; GSM317697     1  0.2066     0.7783 0.940 0.000 0.060
#&gt; GSM317698     1  0.5138     0.7454 0.748 0.000 0.252
#&gt; GSM317650     2  0.0000     0.9813 0.000 1.000 0.000
#&gt; GSM317651     1  0.5926     0.6735 0.644 0.000 0.356
#&gt; GSM317657     2  0.2066     0.9564 0.060 0.940 0.000
#&gt; GSM317667     2  0.0000     0.9813 0.000 1.000 0.000
#&gt; GSM317670     2  0.3340     0.9056 0.120 0.880 0.000
#&gt; GSM317674     1  0.5926     0.6845 0.644 0.000 0.356
#&gt; GSM317675     1  0.5926     0.6845 0.644 0.000 0.356
#&gt; GSM317677     1  0.1529     0.7763 0.960 0.000 0.040
#&gt; GSM317678     2  0.0000     0.9813 0.000 1.000 0.000
#&gt; GSM317687     1  0.0000     0.7526 1.000 0.000 0.000
#&gt; GSM317695     3  0.0424     0.8995 0.008 0.000 0.992
#&gt; GSM317653     2  0.0424     0.9786 0.000 0.992 0.008
#&gt; GSM317656     3  0.0424     0.8995 0.008 0.000 0.992
#&gt; GSM317658     2  0.2356     0.9328 0.072 0.928 0.000
#&gt; GSM317660     3  0.0424     0.8867 0.000 0.008 0.992
#&gt; GSM317663     2  0.1529     0.9665 0.040 0.960 0.000
#&gt; GSM317664     1  0.5926     0.6845 0.644 0.000 0.356
#&gt; GSM317665     3  0.0000     0.8942 0.000 0.000 1.000
#&gt; GSM317673     1  0.5926     0.6845 0.644 0.000 0.356
#&gt; GSM317686     2  0.0000     0.9813 0.000 1.000 0.000
#&gt; GSM317688     3  0.2537     0.8231 0.080 0.000 0.920
#&gt; GSM317690     2  0.0000     0.9813 0.000 1.000 0.000
#&gt; GSM317654     3  0.0000     0.8942 0.000 0.000 1.000
#&gt; GSM317655     2  0.1529     0.9665 0.040 0.960 0.000
#&gt; GSM317659     1  0.1411     0.7745 0.964 0.000 0.036
#&gt; GSM317661     2  0.0000     0.9813 0.000 1.000 0.000
#&gt; GSM317662     2  0.0000     0.9813 0.000 1.000 0.000
#&gt; GSM317668     3  0.6244    -0.1819 0.440 0.000 0.560
#&gt; GSM317669     3  0.0424     0.8995 0.008 0.000 0.992
#&gt; GSM317671     3  0.0424     0.8995 0.008 0.000 0.992
#&gt; GSM317676     1  0.0000     0.7526 1.000 0.000 0.000
#&gt; GSM317680     3  0.0424     0.8995 0.008 0.000 0.992
#&gt; GSM317684     1  0.1643     0.7772 0.956 0.000 0.044
#&gt; GSM317685     1  0.5926     0.6845 0.644 0.000 0.356
#&gt; GSM317694     1  0.4654     0.7596 0.792 0.000 0.208
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-3'>
<p><a id='tab-ATC-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     3  0.0921      0.839 0.028 0.000 0.972 0.000
#&gt; GSM317652     3  0.2589      0.755 0.116 0.000 0.884 0.000
#&gt; GSM317666     4  0.4978      0.719 0.004 0.384 0.000 0.612
#&gt; GSM317672     2  0.2345      0.775 0.000 0.900 0.000 0.100
#&gt; GSM317679     3  0.0921      0.839 0.028 0.000 0.972 0.000
#&gt; GSM317681     2  0.3444      0.690 0.000 0.816 0.000 0.184
#&gt; GSM317682     1  0.6693      0.537 0.624 0.028 0.064 0.284
#&gt; GSM317683     2  0.0000      0.837 0.000 1.000 0.000 0.000
#&gt; GSM317689     2  0.0000      0.837 0.000 1.000 0.000 0.000
#&gt; GSM317691     1  0.4989     -0.113 0.528 0.000 0.000 0.472
#&gt; GSM317692     2  0.1557      0.814 0.000 0.944 0.000 0.056
#&gt; GSM317693     1  0.2149      0.663 0.912 0.000 0.000 0.088
#&gt; GSM317696     1  0.4103      0.733 0.744 0.000 0.256 0.000
#&gt; GSM317697     1  0.2670      0.722 0.904 0.000 0.072 0.024
#&gt; GSM317698     1  0.3831      0.741 0.792 0.000 0.204 0.004
#&gt; GSM317650     2  0.0000      0.837 0.000 1.000 0.000 0.000
#&gt; GSM317651     1  0.6380      0.544 0.632 0.016 0.060 0.292
#&gt; GSM317657     4  0.4978      0.719 0.004 0.384 0.000 0.612
#&gt; GSM317667     2  0.3610      0.638 0.000 0.800 0.000 0.200
#&gt; GSM317670     4  0.5214      0.706 0.004 0.364 0.008 0.624
#&gt; GSM317674     1  0.4103      0.733 0.744 0.000 0.256 0.000
#&gt; GSM317675     1  0.4103      0.733 0.744 0.000 0.256 0.000
#&gt; GSM317677     1  0.1824      0.674 0.936 0.000 0.004 0.060
#&gt; GSM317678     2  0.0000      0.837 0.000 1.000 0.000 0.000
#&gt; GSM317687     4  0.4522      0.497 0.320 0.000 0.000 0.680
#&gt; GSM317695     3  0.0921      0.839 0.028 0.000 0.972 0.000
#&gt; GSM317653     2  0.4188      0.614 0.000 0.752 0.004 0.244
#&gt; GSM317656     3  0.3024      0.711 0.148 0.000 0.852 0.000
#&gt; GSM317658     2  0.4764      0.556 0.032 0.748 0.000 0.220
#&gt; GSM317660     3  0.6669      0.574 0.004 0.108 0.604 0.284
#&gt; GSM317663     4  0.4817      0.715 0.000 0.388 0.000 0.612
#&gt; GSM317664     1  0.4103      0.733 0.744 0.000 0.256 0.000
#&gt; GSM317665     3  0.4799      0.655 0.004 0.008 0.704 0.284
#&gt; GSM317673     1  0.4072      0.734 0.748 0.000 0.252 0.000
#&gt; GSM317686     2  0.3569      0.644 0.000 0.804 0.000 0.196
#&gt; GSM317688     1  0.5080      0.518 0.576 0.000 0.420 0.004
#&gt; GSM317690     2  0.3219      0.691 0.000 0.836 0.000 0.164
#&gt; GSM317654     3  0.4483      0.661 0.004 0.000 0.712 0.284
#&gt; GSM317655     4  0.4830      0.709 0.000 0.392 0.000 0.608
#&gt; GSM317659     1  0.3400      0.555 0.820 0.000 0.000 0.180
#&gt; GSM317661     2  0.0000      0.837 0.000 1.000 0.000 0.000
#&gt; GSM317662     2  0.0000      0.837 0.000 1.000 0.000 0.000
#&gt; GSM317668     1  0.4989      0.402 0.528 0.000 0.472 0.000
#&gt; GSM317669     3  0.0921      0.839 0.028 0.000 0.972 0.000
#&gt; GSM317671     3  0.0921      0.839 0.028 0.000 0.972 0.000
#&gt; GSM317676     4  0.4522      0.497 0.320 0.000 0.000 0.680
#&gt; GSM317680     3  0.0921      0.839 0.028 0.000 0.972 0.000
#&gt; GSM317684     1  0.2401      0.652 0.904 0.000 0.004 0.092
#&gt; GSM317685     1  0.4103      0.733 0.744 0.000 0.256 0.000
#&gt; GSM317694     1  0.4079      0.739 0.800 0.000 0.180 0.020
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-4'>
<p><a id='tab-ATC-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     3  0.3266     0.9263 0.200 0.000 0.796 0.000 0.004
#&gt; GSM317652     3  0.5896     0.3926 0.448 0.000 0.452 0.000 0.100
#&gt; GSM317666     4  0.7040     0.3433 0.000 0.244 0.080 0.552 0.124
#&gt; GSM317672     2  0.1952     0.7752 0.000 0.912 0.004 0.000 0.084
#&gt; GSM317679     3  0.3109     0.9274 0.200 0.000 0.800 0.000 0.000
#&gt; GSM317681     2  0.3980     0.4988 0.000 0.708 0.008 0.000 0.284
#&gt; GSM317682     5  0.3333     0.6930 0.208 0.004 0.000 0.000 0.788
#&gt; GSM317683     2  0.0000     0.8230 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317689     2  0.0324     0.8209 0.000 0.992 0.004 0.000 0.004
#&gt; GSM317691     4  0.5197     0.1573 0.296 0.000 0.040 0.648 0.016
#&gt; GSM317692     2  0.3205     0.7665 0.000 0.864 0.008 0.072 0.056
#&gt; GSM317693     1  0.5821     0.1875 0.524 0.000 0.052 0.404 0.020
#&gt; GSM317696     1  0.0609     0.7546 0.980 0.000 0.020 0.000 0.000
#&gt; GSM317697     1  0.4383     0.6153 0.792 0.000 0.108 0.080 0.020
#&gt; GSM317698     1  0.0579     0.7487 0.984 0.000 0.008 0.008 0.000
#&gt; GSM317650     2  0.0000     0.8230 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317651     5  0.3927     0.7441 0.124 0.004 0.020 0.032 0.820
#&gt; GSM317657     4  0.7251     0.3207 0.000 0.264 0.084 0.520 0.132
#&gt; GSM317667     2  0.5387     0.5556 0.000 0.680 0.020 0.228 0.072
#&gt; GSM317670     4  0.8461     0.3157 0.008 0.172 0.260 0.388 0.172
#&gt; GSM317674     1  0.0609     0.7546 0.980 0.000 0.020 0.000 0.000
#&gt; GSM317675     1  0.0609     0.7546 0.980 0.000 0.020 0.000 0.000
#&gt; GSM317677     1  0.4505     0.3373 0.604 0.000 0.012 0.384 0.000
#&gt; GSM317678     2  0.0000     0.8230 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317687     4  0.0510     0.4497 0.016 0.000 0.000 0.984 0.000
#&gt; GSM317695     3  0.3143     0.9240 0.204 0.000 0.796 0.000 0.000
#&gt; GSM317653     5  0.4504     0.1890 0.000 0.428 0.008 0.000 0.564
#&gt; GSM317656     1  0.4561    -0.4397 0.504 0.000 0.488 0.000 0.008
#&gt; GSM317658     2  0.7639     0.4096 0.056 0.572 0.156 0.060 0.156
#&gt; GSM317660     5  0.3578     0.7722 0.000 0.048 0.132 0.000 0.820
#&gt; GSM317663     4  0.7413     0.2547 0.000 0.312 0.084 0.472 0.132
#&gt; GSM317664     1  0.0609     0.7546 0.980 0.000 0.020 0.000 0.000
#&gt; GSM317665     5  0.3003     0.7579 0.000 0.000 0.188 0.000 0.812
#&gt; GSM317673     1  0.1168     0.7463 0.960 0.000 0.008 0.000 0.032
#&gt; GSM317686     2  0.5387     0.5633 0.000 0.684 0.020 0.220 0.076
#&gt; GSM317688     1  0.4250     0.5522 0.784 0.000 0.128 0.004 0.084
#&gt; GSM317690     2  0.4979     0.6535 0.000 0.756 0.052 0.060 0.132
#&gt; GSM317654     5  0.3003     0.7578 0.000 0.000 0.188 0.000 0.812
#&gt; GSM317655     4  0.7380     0.2743 0.000 0.300 0.084 0.484 0.132
#&gt; GSM317659     4  0.4814    -0.0734 0.412 0.000 0.016 0.568 0.004
#&gt; GSM317661     2  0.0000     0.8230 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317662     2  0.0000     0.8230 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317668     1  0.4337     0.3000 0.696 0.000 0.284 0.004 0.016
#&gt; GSM317669     3  0.3266     0.9263 0.200 0.000 0.796 0.000 0.004
#&gt; GSM317671     3  0.3109     0.9274 0.200 0.000 0.800 0.000 0.000
#&gt; GSM317676     4  0.0798     0.4544 0.008 0.000 0.016 0.976 0.000
#&gt; GSM317680     3  0.3109     0.9274 0.200 0.000 0.800 0.000 0.000
#&gt; GSM317684     4  0.4889    -0.2153 0.476 0.000 0.016 0.504 0.004
#&gt; GSM317685     1  0.0898     0.7527 0.972 0.000 0.020 0.000 0.008
#&gt; GSM317694     1  0.2953     0.6893 0.844 0.000 0.012 0.144 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-5'>
<p><a id='tab-ATC-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM317649     3  0.0363    0.83306 0.012 0.000 0.988 0.000 0.000 0.000
#&gt; GSM317652     3  0.5749    0.02658 0.404 0.000 0.476 0.004 0.104 0.012
#&gt; GSM317666     4  0.3272    0.69502 0.000 0.124 0.000 0.824 0.004 0.048
#&gt; GSM317672     2  0.1375    0.78315 0.000 0.952 0.004 0.008 0.028 0.008
#&gt; GSM317679     3  0.0363    0.83306 0.012 0.000 0.988 0.000 0.000 0.000
#&gt; GSM317681     2  0.3061    0.67433 0.000 0.816 0.004 0.004 0.168 0.008
#&gt; GSM317682     5  0.3977    0.58872 0.240 0.008 0.000 0.004 0.728 0.020
#&gt; GSM317683     2  0.0260    0.80272 0.000 0.992 0.000 0.008 0.000 0.000
#&gt; GSM317689     2  0.1204    0.78237 0.000 0.944 0.000 0.056 0.000 0.000
#&gt; GSM317691     6  0.3331    0.65873 0.096 0.000 0.004 0.056 0.008 0.836
#&gt; GSM317692     2  0.4178    0.69635 0.000 0.796 0.008 0.084 0.068 0.044
#&gt; GSM317693     6  0.4830    0.33751 0.416 0.000 0.000 0.020 0.024 0.540
#&gt; GSM317696     1  0.2191    0.79448 0.876 0.000 0.120 0.000 0.004 0.000
#&gt; GSM317697     1  0.4607    0.28691 0.688 0.000 0.000 0.048 0.020 0.244
#&gt; GSM317698     1  0.1829    0.76871 0.920 0.000 0.064 0.000 0.004 0.012
#&gt; GSM317650     2  0.0260    0.80272 0.000 0.992 0.000 0.008 0.000 0.000
#&gt; GSM317651     5  0.1856    0.75703 0.048 0.000 0.000 0.000 0.920 0.032
#&gt; GSM317657     4  0.2301    0.72059 0.000 0.096 0.000 0.884 0.000 0.020
#&gt; GSM317667     2  0.3810    0.27319 0.000 0.572 0.000 0.428 0.000 0.000
#&gt; GSM317670     4  0.6393    0.46085 0.072 0.032 0.028 0.612 0.032 0.224
#&gt; GSM317674     1  0.2048    0.79484 0.880 0.000 0.120 0.000 0.000 0.000
#&gt; GSM317675     1  0.2048    0.79484 0.880 0.000 0.120 0.000 0.000 0.000
#&gt; GSM317677     1  0.4222   -0.16311 0.516 0.000 0.000 0.004 0.008 0.472
#&gt; GSM317678     2  0.0363    0.79948 0.000 0.988 0.000 0.012 0.000 0.000
#&gt; GSM317687     6  0.3565    0.53942 0.000 0.000 0.000 0.304 0.004 0.692
#&gt; GSM317695     3  0.0458    0.83096 0.016 0.000 0.984 0.000 0.000 0.000
#&gt; GSM317653     5  0.4208    0.08660 0.000 0.452 0.008 0.000 0.536 0.004
#&gt; GSM317656     3  0.4400    0.00485 0.456 0.000 0.524 0.008 0.000 0.012
#&gt; GSM317658     4  0.8100    0.31043 0.124 0.220 0.004 0.376 0.040 0.236
#&gt; GSM317660     5  0.1649    0.77970 0.000 0.032 0.036 0.000 0.932 0.000
#&gt; GSM317663     4  0.2191    0.72707 0.000 0.120 0.000 0.876 0.000 0.004
#&gt; GSM317664     1  0.2048    0.79484 0.880 0.000 0.120 0.000 0.000 0.000
#&gt; GSM317665     5  0.1644    0.77938 0.000 0.004 0.076 0.000 0.920 0.000
#&gt; GSM317673     1  0.2807    0.77802 0.868 0.000 0.088 0.000 0.028 0.016
#&gt; GSM317686     2  0.3789    0.30093 0.000 0.584 0.000 0.416 0.000 0.000
#&gt; GSM317688     1  0.5752    0.59144 0.640 0.000 0.204 0.012 0.100 0.044
#&gt; GSM317690     2  0.5992    0.12487 0.016 0.524 0.004 0.360 0.024 0.072
#&gt; GSM317654     5  0.1610    0.77486 0.000 0.000 0.084 0.000 0.916 0.000
#&gt; GSM317655     4  0.2482    0.71468 0.000 0.148 0.000 0.848 0.000 0.004
#&gt; GSM317659     6  0.4315    0.66352 0.224 0.000 0.000 0.048 0.012 0.716
#&gt; GSM317661     2  0.0260    0.80272 0.000 0.992 0.000 0.008 0.000 0.000
#&gt; GSM317662     2  0.0260    0.80272 0.000 0.992 0.000 0.008 0.000 0.000
#&gt; GSM317668     1  0.5421    0.35953 0.564 0.000 0.344 0.012 0.008 0.072
#&gt; GSM317669     3  0.0363    0.83306 0.012 0.000 0.988 0.000 0.000 0.000
#&gt; GSM317671     3  0.0363    0.83306 0.012 0.000 0.988 0.000 0.000 0.000
#&gt; GSM317676     6  0.3955    0.43123 0.000 0.000 0.000 0.384 0.008 0.608
#&gt; GSM317680     3  0.0363    0.83306 0.012 0.000 0.988 0.000 0.000 0.000
#&gt; GSM317684     6  0.3650    0.62374 0.272 0.000 0.000 0.004 0.008 0.716
#&gt; GSM317685     1  0.2798    0.79145 0.856 0.000 0.120 0.004 0.008 0.012
#&gt; GSM317694     1  0.4474    0.64580 0.704 0.000 0.108 0.000 0.000 0.188
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-skmeans-signature_compare](figure_cola/ATC-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-skmeans-collect-classes](figure_cola/ATC-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>              n disease.state(p) k
#> ATC:skmeans 49            0.553 2
#> ATC:skmeans 48            0.632 3
#> ATC:skmeans 46            0.931 4
#> ATC:skmeans 32            0.848 5
#> ATC:skmeans 37            0.765 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:pam*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "pam"]
# you can also extract it by
# res = res_list["ATC:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-pam-collect-plots](figure_cola/ATC-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-pam-select-partition-number](figure_cola/ATC-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.761           0.872       0.943         0.4812 0.497   0.497
#> 3 3 0.920           0.891       0.950         0.2225 0.909   0.816
#> 4 4 0.795           0.729       0.808         0.1214 0.957   0.894
#> 5 5 0.946           0.892       0.933         0.1015 0.878   0.682
#> 6 6 0.801           0.805       0.899         0.0734 0.945   0.805
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 3
```

There is also optional best $k$ = 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-classes'>
<ul>
<li><a href='#tab-ATC-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-pam-get-classes-1'>
<p><a id='tab-ATC-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     1   0.000      1.000 1.000 0.000
#&gt; GSM317652     1   0.000      1.000 1.000 0.000
#&gt; GSM317666     2   0.000      0.856 0.000 1.000
#&gt; GSM317672     2   0.999      0.201 0.480 0.520
#&gt; GSM317679     1   0.000      1.000 1.000 0.000
#&gt; GSM317681     2   0.730      0.726 0.204 0.796
#&gt; GSM317682     1   0.000      1.000 1.000 0.000
#&gt; GSM317683     2   0.000      0.856 0.000 1.000
#&gt; GSM317689     2   0.000      0.856 0.000 1.000
#&gt; GSM317691     2   1.000      0.176 0.496 0.504
#&gt; GSM317692     2   0.634      0.766 0.160 0.840
#&gt; GSM317693     1   0.000      1.000 1.000 0.000
#&gt; GSM317696     1   0.000      1.000 1.000 0.000
#&gt; GSM317697     1   0.000      1.000 1.000 0.000
#&gt; GSM317698     1   0.000      1.000 1.000 0.000
#&gt; GSM317650     2   0.000      0.856 0.000 1.000
#&gt; GSM317651     1   0.000      1.000 1.000 0.000
#&gt; GSM317657     2   0.000      0.856 0.000 1.000
#&gt; GSM317667     2   0.000      0.856 0.000 1.000
#&gt; GSM317670     2   0.697      0.737 0.188 0.812
#&gt; GSM317674     1   0.000      1.000 1.000 0.000
#&gt; GSM317675     1   0.000      1.000 1.000 0.000
#&gt; GSM317677     1   0.000      1.000 1.000 0.000
#&gt; GSM317678     2   0.000      0.856 0.000 1.000
#&gt; GSM317687     2   1.000      0.176 0.496 0.504
#&gt; GSM317695     1   0.000      1.000 1.000 0.000
#&gt; GSM317653     2   0.722      0.731 0.200 0.800
#&gt; GSM317656     1   0.000      1.000 1.000 0.000
#&gt; GSM317658     2   0.563      0.791 0.132 0.868
#&gt; GSM317660     1   0.000      1.000 1.000 0.000
#&gt; GSM317663     2   0.000      0.856 0.000 1.000
#&gt; GSM317664     1   0.000      1.000 1.000 0.000
#&gt; GSM317665     1   0.000      1.000 1.000 0.000
#&gt; GSM317673     1   0.000      1.000 1.000 0.000
#&gt; GSM317686     2   0.000      0.856 0.000 1.000
#&gt; GSM317688     1   0.000      1.000 1.000 0.000
#&gt; GSM317690     2   0.000      0.856 0.000 1.000
#&gt; GSM317654     1   0.000      1.000 1.000 0.000
#&gt; GSM317655     2   0.000      0.856 0.000 1.000
#&gt; GSM317659     1   0.000      1.000 1.000 0.000
#&gt; GSM317661     2   0.000      0.856 0.000 1.000
#&gt; GSM317662     2   0.000      0.856 0.000 1.000
#&gt; GSM317668     1   0.000      1.000 1.000 0.000
#&gt; GSM317669     1   0.000      1.000 1.000 0.000
#&gt; GSM317671     1   0.000      1.000 1.000 0.000
#&gt; GSM317676     2   1.000      0.176 0.496 0.504
#&gt; GSM317680     1   0.000      1.000 1.000 0.000
#&gt; GSM317684     1   0.000      1.000 1.000 0.000
#&gt; GSM317685     1   0.000      1.000 1.000 0.000
#&gt; GSM317694     1   0.000      1.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-1-a').click(function(){
  $('#tab-ATC-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-2'>
<p><a id='tab-ATC-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM317649     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM317652     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM317666     2  0.5859      0.892 0.000 0.656 0.344
#&gt; GSM317672     3  0.1964      0.585 0.000 0.056 0.944
#&gt; GSM317679     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM317681     3  0.0000      0.655 0.000 0.000 1.000
#&gt; GSM317682     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM317683     3  0.5859      0.742 0.000 0.344 0.656
#&gt; GSM317689     2  0.5859      0.892 0.000 0.656 0.344
#&gt; GSM317691     2  0.5859      0.892 0.000 0.656 0.344
#&gt; GSM317692     2  0.9047      0.662 0.148 0.508 0.344
#&gt; GSM317693     1  0.0592      0.987 0.988 0.000 0.012
#&gt; GSM317696     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM317697     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM317698     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM317650     3  0.5859      0.742 0.000 0.344 0.656
#&gt; GSM317651     1  0.0237      0.995 0.996 0.000 0.004
#&gt; GSM317657     2  0.5859      0.892 0.000 0.656 0.344
#&gt; GSM317667     2  0.1964      0.361 0.000 0.944 0.056
#&gt; GSM317670     2  0.5859      0.892 0.000 0.656 0.344
#&gt; GSM317674     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM317675     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM317677     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM317678     3  0.0000      0.655 0.000 0.000 1.000
#&gt; GSM317687     2  0.5859      0.892 0.000 0.656 0.344
#&gt; GSM317695     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM317653     3  0.1753      0.599 0.000 0.048 0.952
#&gt; GSM317656     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM317658     2  0.7023      0.854 0.032 0.624 0.344
#&gt; GSM317660     1  0.0592      0.987 0.988 0.000 0.012
#&gt; GSM317663     2  0.5859      0.892 0.000 0.656 0.344
#&gt; GSM317664     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM317665     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM317673     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM317686     2  0.1964      0.361 0.000 0.944 0.056
#&gt; GSM317688     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM317690     2  0.5859      0.892 0.000 0.656 0.344
#&gt; GSM317654     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM317655     2  0.5859      0.892 0.000 0.656 0.344
#&gt; GSM317659     1  0.0237      0.995 0.996 0.000 0.004
#&gt; GSM317661     3  0.5859      0.742 0.000 0.344 0.656
#&gt; GSM317662     3  0.5859      0.742 0.000 0.344 0.656
#&gt; GSM317668     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM317669     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM317671     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM317676     2  0.5859      0.892 0.000 0.656 0.344
#&gt; GSM317680     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM317684     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM317685     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM317694     1  0.0000      0.999 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-2-a').click(function(){
  $('#tab-ATC-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-3'>
<p><a id='tab-ATC-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     1  0.0707      0.515 0.980 0.000 0.020 0.000
#&gt; GSM317652     1  0.4830      0.838 0.608 0.000 0.392 0.000
#&gt; GSM317666     4  0.0000      0.917 0.000 0.000 0.000 1.000
#&gt; GSM317672     2  0.5820      0.768 0.000 0.696 0.204 0.100
#&gt; GSM317679     1  0.0000      0.497 1.000 0.000 0.000 0.000
#&gt; GSM317681     2  0.5628      0.791 0.000 0.724 0.144 0.132
#&gt; GSM317682     1  0.4967      0.815 0.548 0.000 0.452 0.000
#&gt; GSM317683     2  0.0000      0.804 0.000 1.000 0.000 0.000
#&gt; GSM317689     4  0.0376      0.914 0.000 0.004 0.004 0.992
#&gt; GSM317691     4  0.1867      0.872 0.000 0.000 0.072 0.928
#&gt; GSM317692     4  0.4746      0.482 0.000 0.000 0.368 0.632
#&gt; GSM317693     1  0.4981      0.809 0.536 0.000 0.464 0.000
#&gt; GSM317696     1  0.4830      0.838 0.608 0.000 0.392 0.000
#&gt; GSM317697     1  0.4967      0.815 0.548 0.000 0.452 0.000
#&gt; GSM317698     1  0.4830      0.838 0.608 0.000 0.392 0.000
#&gt; GSM317650     2  0.0000      0.804 0.000 1.000 0.000 0.000
#&gt; GSM317651     1  0.5147      0.807 0.536 0.000 0.460 0.004
#&gt; GSM317657     4  0.0000      0.917 0.000 0.000 0.000 1.000
#&gt; GSM317667     3  0.7870      0.109 0.000 0.304 0.392 0.304
#&gt; GSM317670     4  0.0000      0.917 0.000 0.000 0.000 1.000
#&gt; GSM317674     1  0.4830      0.838 0.608 0.000 0.392 0.000
#&gt; GSM317675     1  0.4830      0.838 0.608 0.000 0.392 0.000
#&gt; GSM317677     1  0.4981      0.809 0.536 0.000 0.464 0.000
#&gt; GSM317678     2  0.5628      0.791 0.000 0.724 0.144 0.132
#&gt; GSM317687     4  0.1867      0.872 0.000 0.000 0.072 0.928
#&gt; GSM317695     1  0.0000      0.497 1.000 0.000 0.000 0.000
#&gt; GSM317653     2  0.5750      0.765 0.000 0.696 0.216 0.088
#&gt; GSM317656     1  0.4830      0.838 0.608 0.000 0.392 0.000
#&gt; GSM317658     4  0.3649      0.755 0.000 0.000 0.204 0.796
#&gt; GSM317660     3  0.5004     -0.721 0.392 0.000 0.604 0.004
#&gt; GSM317663     4  0.0000      0.917 0.000 0.000 0.000 1.000
#&gt; GSM317664     1  0.4830      0.838 0.608 0.000 0.392 0.000
#&gt; GSM317665     1  0.4985      0.806 0.532 0.000 0.468 0.000
#&gt; GSM317673     1  0.4830      0.838 0.608 0.000 0.392 0.000
#&gt; GSM317686     3  0.7870      0.109 0.000 0.304 0.392 0.304
#&gt; GSM317688     1  0.4830      0.838 0.608 0.000 0.392 0.000
#&gt; GSM317690     4  0.0000      0.917 0.000 0.000 0.000 1.000
#&gt; GSM317654     1  0.4981      0.809 0.536 0.000 0.464 0.000
#&gt; GSM317655     4  0.0000      0.917 0.000 0.000 0.000 1.000
#&gt; GSM317659     1  0.4981      0.809 0.536 0.000 0.464 0.000
#&gt; GSM317661     2  0.0000      0.804 0.000 1.000 0.000 0.000
#&gt; GSM317662     2  0.1557      0.768 0.000 0.944 0.056 0.000
#&gt; GSM317668     1  0.4830      0.838 0.608 0.000 0.392 0.000
#&gt; GSM317669     1  0.0000      0.497 1.000 0.000 0.000 0.000
#&gt; GSM317671     1  0.0000      0.497 1.000 0.000 0.000 0.000
#&gt; GSM317676     4  0.0469      0.911 0.000 0.000 0.012 0.988
#&gt; GSM317680     1  0.0000      0.497 1.000 0.000 0.000 0.000
#&gt; GSM317684     1  0.4981      0.809 0.536 0.000 0.464 0.000
#&gt; GSM317685     1  0.4830      0.838 0.608 0.000 0.392 0.000
#&gt; GSM317694     1  0.4830      0.838 0.608 0.000 0.392 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-3-a').click(function(){
  $('#tab-ATC-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-4'>
<p><a id='tab-ATC-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     3  0.2516      0.738 0.140 0.000 0.860 0.000 0.000
#&gt; GSM317652     1  0.1671      0.956 0.924 0.000 0.076 0.000 0.000
#&gt; GSM317666     4  0.0000      0.907 0.000 0.000 0.000 1.000 0.000
#&gt; GSM317672     2  0.0000      0.823 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317679     3  0.0000      0.949 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317681     2  0.0000      0.823 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317682     1  0.1341      0.952 0.944 0.000 0.056 0.000 0.000
#&gt; GSM317683     2  0.3177      0.797 0.000 0.792 0.000 0.000 0.208
#&gt; GSM317689     4  0.1270      0.892 0.000 0.052 0.000 0.948 0.000
#&gt; GSM317691     4  0.1671      0.885 0.076 0.000 0.000 0.924 0.000
#&gt; GSM317692     4  0.5902      0.485 0.192 0.208 0.000 0.600 0.000
#&gt; GSM317693     1  0.0000      0.933 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317696     1  0.1671      0.956 0.924 0.000 0.076 0.000 0.000
#&gt; GSM317697     1  0.1341      0.952 0.944 0.000 0.056 0.000 0.000
#&gt; GSM317698     1  0.1671      0.956 0.924 0.000 0.076 0.000 0.000
#&gt; GSM317650     2  0.3177      0.797 0.000 0.792 0.000 0.000 0.208
#&gt; GSM317651     1  0.0000      0.933 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317657     4  0.0000      0.907 0.000 0.000 0.000 1.000 0.000
#&gt; GSM317667     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM317670     4  0.0000      0.907 0.000 0.000 0.000 1.000 0.000
#&gt; GSM317674     1  0.1671      0.956 0.924 0.000 0.076 0.000 0.000
#&gt; GSM317675     1  0.1671      0.956 0.924 0.000 0.076 0.000 0.000
#&gt; GSM317677     1  0.0000      0.933 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317678     2  0.0000      0.823 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317687     4  0.1671      0.885 0.076 0.000 0.000 0.924 0.000
#&gt; GSM317695     3  0.0000      0.949 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317653     2  0.0703      0.805 0.024 0.976 0.000 0.000 0.000
#&gt; GSM317656     1  0.1671      0.956 0.924 0.000 0.076 0.000 0.000
#&gt; GSM317658     4  0.3883      0.739 0.036 0.184 0.000 0.780 0.000
#&gt; GSM317660     1  0.3177      0.703 0.792 0.208 0.000 0.000 0.000
#&gt; GSM317663     4  0.0000      0.907 0.000 0.000 0.000 1.000 0.000
#&gt; GSM317664     1  0.1671      0.956 0.924 0.000 0.076 0.000 0.000
#&gt; GSM317665     1  0.0000      0.933 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317673     1  0.1671      0.956 0.924 0.000 0.076 0.000 0.000
#&gt; GSM317686     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM317688     1  0.1671      0.956 0.924 0.000 0.076 0.000 0.000
#&gt; GSM317690     4  0.1121      0.895 0.000 0.044 0.000 0.956 0.000
#&gt; GSM317654     1  0.0000      0.933 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317655     4  0.0000      0.907 0.000 0.000 0.000 1.000 0.000
#&gt; GSM317659     1  0.0000      0.933 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317661     2  0.3177      0.797 0.000 0.792 0.000 0.000 0.208
#&gt; GSM317662     2  0.4015      0.634 0.000 0.652 0.000 0.000 0.348
#&gt; GSM317668     1  0.1671      0.956 0.924 0.000 0.076 0.000 0.000
#&gt; GSM317669     3  0.0000      0.949 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317671     3  0.0000      0.949 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317676     4  0.1341      0.893 0.056 0.000 0.000 0.944 0.000
#&gt; GSM317680     3  0.0000      0.949 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317684     1  0.0000      0.933 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317685     1  0.1671      0.956 0.924 0.000 0.076 0.000 0.000
#&gt; GSM317694     1  0.1671      0.956 0.924 0.000 0.076 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-4-a').click(function(){
  $('#tab-ATC-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-5'>
<p><a id='tab-ATC-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM317649     3  0.2823      0.682 0.204 0.000 0.796 0.000 0.000 0.000
#&gt; GSM317652     1  0.0000      0.900 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317666     4  0.0000      0.820 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317672     2  0.2070      0.827 0.000 0.892 0.008 0.000 0.100 0.000
#&gt; GSM317679     3  0.0458      0.940 0.016 0.000 0.984 0.000 0.000 0.000
#&gt; GSM317681     2  0.2070      0.827 0.000 0.892 0.008 0.000 0.100 0.000
#&gt; GSM317682     1  0.1918      0.838 0.904 0.000 0.008 0.000 0.088 0.000
#&gt; GSM317683     2  0.1556      0.846 0.000 0.920 0.000 0.000 0.000 0.080
#&gt; GSM317689     4  0.1765      0.791 0.000 0.096 0.000 0.904 0.000 0.000
#&gt; GSM317691     4  0.3634      0.668 0.000 0.000 0.008 0.696 0.296 0.000
#&gt; GSM317692     4  0.7105      0.262 0.152 0.080 0.016 0.456 0.296 0.000
#&gt; GSM317693     1  0.3634      0.628 0.696 0.000 0.008 0.000 0.296 0.000
#&gt; GSM317696     1  0.0000      0.900 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317697     1  0.2070      0.831 0.892 0.000 0.008 0.000 0.100 0.000
#&gt; GSM317698     1  0.0000      0.900 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317650     2  0.1556      0.846 0.000 0.920 0.000 0.000 0.000 0.080
#&gt; GSM317651     1  0.3547      0.579 0.668 0.000 0.000 0.000 0.332 0.000
#&gt; GSM317657     4  0.0000      0.820 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317667     6  0.0000      1.000 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM317670     4  0.0000      0.820 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317674     1  0.0000      0.900 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317675     1  0.0000      0.900 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317677     1  0.0622      0.890 0.980 0.000 0.008 0.000 0.012 0.000
#&gt; GSM317678     2  0.2070      0.827 0.000 0.892 0.008 0.000 0.100 0.000
#&gt; GSM317687     4  0.3634      0.668 0.000 0.000 0.008 0.696 0.296 0.000
#&gt; GSM317695     3  0.0458      0.940 0.016 0.000 0.984 0.000 0.000 0.000
#&gt; GSM317653     5  0.2212      0.632 0.000 0.112 0.008 0.000 0.880 0.000
#&gt; GSM317656     1  0.0000      0.900 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317658     4  0.4688      0.703 0.024 0.076 0.016 0.748 0.136 0.000
#&gt; GSM317660     5  0.3708      0.662 0.112 0.080 0.008 0.000 0.800 0.000
#&gt; GSM317663     4  0.0000      0.820 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317664     1  0.0000      0.900 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317665     5  0.3050      0.640 0.236 0.000 0.000 0.000 0.764 0.000
#&gt; GSM317673     1  0.0000      0.900 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317686     6  0.0000      1.000 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM317688     1  0.0000      0.900 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317690     4  0.1765      0.791 0.000 0.096 0.000 0.904 0.000 0.000
#&gt; GSM317654     5  0.1957      0.679 0.112 0.000 0.000 0.000 0.888 0.000
#&gt; GSM317655     4  0.0000      0.820 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM317659     1  0.3634      0.628 0.696 0.000 0.008 0.000 0.296 0.000
#&gt; GSM317661     2  0.1556      0.846 0.000 0.920 0.000 0.000 0.000 0.080
#&gt; GSM317662     2  0.3101      0.679 0.000 0.756 0.000 0.000 0.000 0.244
#&gt; GSM317668     1  0.0000      0.900 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317669     3  0.0458      0.940 0.016 0.000 0.984 0.000 0.000 0.000
#&gt; GSM317671     3  0.0458      0.940 0.016 0.000 0.984 0.000 0.000 0.000
#&gt; GSM317676     4  0.2854      0.727 0.000 0.000 0.000 0.792 0.208 0.000
#&gt; GSM317680     3  0.0458      0.940 0.016 0.000 0.984 0.000 0.000 0.000
#&gt; GSM317684     1  0.3634      0.628 0.696 0.000 0.008 0.000 0.296 0.000
#&gt; GSM317685     1  0.0000      0.900 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317694     1  0.0000      0.900 1.000 0.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-5-a').click(function(){
  $('#tab-ATC-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-pam-signature_compare](figure_cola/ATC-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-pam-collect-classes](figure_cola/ATC-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n disease.state(p) k
#> ATC:pam 46            0.639 2
#> ATC:pam 48            0.953 3
#> ATC:pam 41            0.977 4
#> ATC:pam 49            0.874 5
#> ATC:pam 49            0.454 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "mclust"]
# you can also extract it by
# res = res_list["ATC:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-mclust-collect-plots](figure_cola/ATC-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-mclust-select-partition-number](figure_cola/ATC-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.542           0.911       0.918          0.448 0.542   0.542
#> 3 3 0.414           0.697       0.808          0.266 0.846   0.726
#> 4 4 0.399           0.366       0.630          0.162 0.860   0.709
#> 5 5 0.482           0.455       0.692          0.136 0.792   0.517
#> 6 6 0.691           0.801       0.866          0.059 0.845   0.501
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-classes'>
<ul>
<li><a href='#tab-ATC-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-mclust-get-classes-1'>
<p><a id='tab-ATC-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     1  0.3879      0.908 0.924 0.076
#&gt; GSM317652     1  0.0000      0.881 1.000 0.000
#&gt; GSM317666     2  0.1414      0.964 0.020 0.980
#&gt; GSM317672     2  0.4431      0.907 0.092 0.908
#&gt; GSM317679     1  0.7299      0.882 0.796 0.204
#&gt; GSM317681     2  0.2423      0.947 0.040 0.960
#&gt; GSM317682     1  0.6343      0.902 0.840 0.160
#&gt; GSM317683     2  0.0000      0.973 0.000 1.000
#&gt; GSM317689     2  0.0000      0.973 0.000 1.000
#&gt; GSM317691     1  0.7219      0.885 0.800 0.200
#&gt; GSM317692     1  0.8443      0.791 0.728 0.272
#&gt; GSM317693     1  0.5519      0.910 0.872 0.128
#&gt; GSM317696     1  0.0000      0.881 1.000 0.000
#&gt; GSM317697     1  0.4161      0.910 0.916 0.084
#&gt; GSM317698     1  0.0000      0.881 1.000 0.000
#&gt; GSM317650     2  0.0000      0.973 0.000 1.000
#&gt; GSM317651     1  0.6148      0.905 0.848 0.152
#&gt; GSM317657     2  0.2236      0.953 0.036 0.964
#&gt; GSM317667     2  0.0000      0.973 0.000 1.000
#&gt; GSM317670     2  0.4815      0.875 0.104 0.896
#&gt; GSM317674     1  0.0000      0.881 1.000 0.000
#&gt; GSM317675     1  0.0000      0.881 1.000 0.000
#&gt; GSM317677     1  0.6048      0.906 0.852 0.148
#&gt; GSM317678     2  0.0000      0.973 0.000 1.000
#&gt; GSM317687     1  0.7219      0.885 0.800 0.200
#&gt; GSM317695     1  0.7219      0.885 0.800 0.200
#&gt; GSM317653     2  0.3879      0.924 0.076 0.924
#&gt; GSM317656     1  0.3584      0.906 0.932 0.068
#&gt; GSM317658     1  0.8443      0.809 0.728 0.272
#&gt; GSM317660     1  0.7528      0.859 0.784 0.216
#&gt; GSM317663     2  0.0672      0.970 0.008 0.992
#&gt; GSM317664     1  0.0000      0.881 1.000 0.000
#&gt; GSM317665     1  0.2948      0.903 0.948 0.052
#&gt; GSM317673     1  0.0000      0.881 1.000 0.000
#&gt; GSM317686     2  0.0000      0.973 0.000 1.000
#&gt; GSM317688     1  0.1633      0.892 0.976 0.024
#&gt; GSM317690     2  0.0000      0.973 0.000 1.000
#&gt; GSM317654     1  0.3431      0.905 0.936 0.064
#&gt; GSM317655     2  0.0000      0.973 0.000 1.000
#&gt; GSM317659     1  0.6247      0.903 0.844 0.156
#&gt; GSM317661     2  0.0000      0.973 0.000 1.000
#&gt; GSM317662     2  0.0000      0.973 0.000 1.000
#&gt; GSM317668     1  0.5519      0.910 0.872 0.128
#&gt; GSM317669     1  0.3584      0.906 0.932 0.068
#&gt; GSM317671     1  0.7299      0.882 0.796 0.204
#&gt; GSM317676     1  0.7528      0.873 0.784 0.216
#&gt; GSM317680     1  0.7139      0.887 0.804 0.196
#&gt; GSM317684     1  0.5629      0.910 0.868 0.132
#&gt; GSM317685     1  0.0000      0.881 1.000 0.000
#&gt; GSM317694     1  0.5519      0.910 0.872 0.128
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-1-a').click(function(){
  $('#tab-ATC-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-2'>
<p><a id='tab-ATC-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM317649     1  0.4475      0.814 0.840 0.144 0.016
#&gt; GSM317652     1  0.0000      0.866 1.000 0.000 0.000
#&gt; GSM317666     2  0.5760      0.563 0.000 0.672 0.328
#&gt; GSM317672     2  0.9615      0.512 0.220 0.456 0.324
#&gt; GSM317679     1  0.5414      0.797 0.772 0.212 0.016
#&gt; GSM317681     3  0.7956     -0.285 0.060 0.424 0.516
#&gt; GSM317682     1  0.6968      0.700 0.732 0.120 0.148
#&gt; GSM317683     3  0.0000      0.755 0.000 0.000 1.000
#&gt; GSM317689     2  0.7295      0.563 0.036 0.584 0.380
#&gt; GSM317691     1  0.4654      0.800 0.792 0.208 0.000
#&gt; GSM317692     2  0.9659      0.459 0.340 0.440 0.220
#&gt; GSM317693     1  0.3816      0.823 0.852 0.148 0.000
#&gt; GSM317696     1  0.0000      0.866 1.000 0.000 0.000
#&gt; GSM317697     1  0.2261      0.855 0.932 0.068 0.000
#&gt; GSM317698     1  0.0237      0.865 0.996 0.004 0.000
#&gt; GSM317650     3  0.0000      0.755 0.000 0.000 1.000
#&gt; GSM317651     1  0.3713      0.838 0.892 0.076 0.032
#&gt; GSM317657     2  0.6172      0.589 0.012 0.680 0.308
#&gt; GSM317667     3  0.3752      0.668 0.000 0.144 0.856
#&gt; GSM317670     2  0.7901      0.607 0.080 0.608 0.312
#&gt; GSM317674     1  0.0000      0.866 1.000 0.000 0.000
#&gt; GSM317675     1  0.0000      0.866 1.000 0.000 0.000
#&gt; GSM317677     1  0.4887      0.788 0.772 0.228 0.000
#&gt; GSM317678     3  0.5733      0.174 0.000 0.324 0.676
#&gt; GSM317687     1  0.6047      0.676 0.680 0.312 0.008
#&gt; GSM317695     1  0.4702      0.804 0.788 0.212 0.000
#&gt; GSM317653     2  0.9573      0.518 0.212 0.460 0.328
#&gt; GSM317656     1  0.3686      0.822 0.860 0.140 0.000
#&gt; GSM317658     2  0.9553      0.534 0.244 0.484 0.272
#&gt; GSM317660     1  0.8170      0.493 0.624 0.120 0.256
#&gt; GSM317663     2  0.5760      0.563 0.000 0.672 0.328
#&gt; GSM317664     1  0.0237      0.865 0.996 0.004 0.000
#&gt; GSM317665     1  0.6393      0.736 0.768 0.112 0.120
#&gt; GSM317673     1  0.0000      0.866 1.000 0.000 0.000
#&gt; GSM317686     3  0.3686      0.671 0.000 0.140 0.860
#&gt; GSM317688     1  0.0000      0.866 1.000 0.000 0.000
#&gt; GSM317690     2  0.7083      0.538 0.028 0.592 0.380
#&gt; GSM317654     1  0.2550      0.854 0.932 0.056 0.012
#&gt; GSM317655     2  0.5785      0.556 0.000 0.668 0.332
#&gt; GSM317659     1  0.4887      0.788 0.772 0.228 0.000
#&gt; GSM317661     3  0.0000      0.755 0.000 0.000 1.000
#&gt; GSM317662     3  0.0000      0.755 0.000 0.000 1.000
#&gt; GSM317668     1  0.2796      0.853 0.908 0.092 0.000
#&gt; GSM317669     1  0.4615      0.813 0.836 0.144 0.020
#&gt; GSM317671     1  0.4702      0.804 0.788 0.212 0.000
#&gt; GSM317676     2  0.8985      0.299 0.300 0.540 0.160
#&gt; GSM317680     1  0.4702      0.804 0.788 0.212 0.000
#&gt; GSM317684     1  0.4605      0.802 0.796 0.204 0.000
#&gt; GSM317685     1  0.0000      0.866 1.000 0.000 0.000
#&gt; GSM317694     1  0.2625      0.854 0.916 0.084 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-2-a').click(function(){
  $('#tab-ATC-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-3'>
<p><a id='tab-ATC-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     1  0.4610     0.6179 0.744 0.020 0.236 0.000
#&gt; GSM317652     1  0.0188     0.7051 0.996 0.000 0.004 0.000
#&gt; GSM317666     4  0.5350     0.1964 0.008 0.060 0.188 0.744
#&gt; GSM317672     2  0.7819     0.2363 0.344 0.508 0.044 0.104
#&gt; GSM317679     1  0.5273     0.4427 0.536 0.008 0.456 0.000
#&gt; GSM317681     2  0.8066     0.2063 0.232 0.568 0.116 0.084
#&gt; GSM317682     1  0.5231     0.3788 0.604 0.384 0.000 0.012
#&gt; GSM317683     2  0.4998     0.1413 0.000 0.512 0.488 0.000
#&gt; GSM317689     4  0.9676    -0.1713 0.224 0.192 0.200 0.384
#&gt; GSM317691     1  0.5097     0.4488 0.568 0.004 0.000 0.428
#&gt; GSM317692     1  0.7848    -0.1334 0.460 0.404 0.060 0.076
#&gt; GSM317693     1  0.4252     0.6233 0.744 0.004 0.000 0.252
#&gt; GSM317696     1  0.0817     0.7043 0.976 0.000 0.024 0.000
#&gt; GSM317697     1  0.4088     0.6327 0.764 0.004 0.000 0.232
#&gt; GSM317698     1  0.0657     0.7035 0.984 0.004 0.000 0.012
#&gt; GSM317650     2  0.4998     0.1413 0.000 0.512 0.488 0.000
#&gt; GSM317651     1  0.4955     0.5336 0.708 0.268 0.000 0.024
#&gt; GSM317657     4  0.5474     0.1944 0.012 0.060 0.188 0.740
#&gt; GSM317667     3  0.6055     0.2340 0.000 0.044 0.520 0.436
#&gt; GSM317670     3  0.7410     0.0830 0.048 0.060 0.504 0.388
#&gt; GSM317674     1  0.0592     0.7051 0.984 0.000 0.016 0.000
#&gt; GSM317675     1  0.1118     0.7021 0.964 0.000 0.036 0.000
#&gt; GSM317677     1  0.5080     0.4647 0.576 0.004 0.000 0.420
#&gt; GSM317678     2  0.5397     0.0276 0.000 0.716 0.220 0.064
#&gt; GSM317687     4  0.4961    -0.3073 0.448 0.000 0.000 0.552
#&gt; GSM317695     1  0.4972     0.4470 0.544 0.000 0.456 0.000
#&gt; GSM317653     2  0.7646     0.2153 0.364 0.500 0.032 0.104
#&gt; GSM317656     1  0.3688     0.6405 0.792 0.000 0.208 0.000
#&gt; GSM317658     3  0.8655     0.0096 0.164 0.060 0.396 0.380
#&gt; GSM317660     2  0.5942     0.0343 0.412 0.548 0.000 0.040
#&gt; GSM317663     4  0.5030     0.1913 0.000 0.060 0.188 0.752
#&gt; GSM317664     1  0.1389     0.6992 0.952 0.000 0.048 0.000
#&gt; GSM317665     1  0.5093     0.4459 0.640 0.348 0.000 0.012
#&gt; GSM317673     1  0.0657     0.7035 0.984 0.004 0.000 0.012
#&gt; GSM317686     3  0.6875     0.2653 0.000 0.112 0.520 0.368
#&gt; GSM317688     1  0.0524     0.7047 0.988 0.008 0.000 0.004
#&gt; GSM317690     4  0.7685    -0.2543 0.016 0.144 0.360 0.480
#&gt; GSM317654     1  0.4609     0.5830 0.752 0.224 0.000 0.024
#&gt; GSM317655     4  0.5109     0.1768 0.000 0.060 0.196 0.744
#&gt; GSM317659     1  0.5097     0.4523 0.568 0.004 0.000 0.428
#&gt; GSM317661     2  0.4998     0.1413 0.000 0.512 0.488 0.000
#&gt; GSM317662     2  0.4998     0.1413 0.000 0.512 0.488 0.000
#&gt; GSM317668     1  0.3837     0.6455 0.776 0.000 0.224 0.000
#&gt; GSM317669     1  0.4599     0.6277 0.760 0.028 0.212 0.000
#&gt; GSM317671     1  0.4972     0.4470 0.544 0.000 0.456 0.000
#&gt; GSM317676     4  0.2814     0.0827 0.132 0.000 0.000 0.868
#&gt; GSM317680     1  0.4955     0.4604 0.556 0.000 0.444 0.000
#&gt; GSM317684     1  0.4741     0.5698 0.668 0.004 0.000 0.328
#&gt; GSM317685     1  0.0469     0.7053 0.988 0.000 0.012 0.000
#&gt; GSM317694     1  0.3726     0.6458 0.788 0.000 0.000 0.212
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-3-a').click(function(){
  $('#tab-ATC-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-4'>
<p><a id='tab-ATC-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     1  0.4147     -0.130 0.676 0.000 0.316 0.000 0.008
#&gt; GSM317652     1  0.0510      0.505 0.984 0.000 0.000 0.000 0.016
#&gt; GSM317666     4  0.0162      0.567 0.000 0.000 0.004 0.996 0.000
#&gt; GSM317672     5  0.6256      0.655 0.268 0.000 0.032 0.104 0.596
#&gt; GSM317679     3  0.4138      0.996 0.384 0.000 0.616 0.000 0.000
#&gt; GSM317681     5  0.5201      0.596 0.036 0.032 0.032 0.148 0.752
#&gt; GSM317682     5  0.2629      0.730 0.136 0.000 0.004 0.000 0.860
#&gt; GSM317683     2  0.2732      0.912 0.000 0.840 0.000 0.160 0.000
#&gt; GSM317689     4  0.7581      0.268 0.236 0.080 0.056 0.556 0.072
#&gt; GSM317691     1  0.9570      0.242 0.356 0.160 0.184 0.164 0.136
#&gt; GSM317692     5  0.7113      0.427 0.324 0.000 0.040 0.164 0.472
#&gt; GSM317693     1  0.8192      0.349 0.492 0.160 0.184 0.024 0.140
#&gt; GSM317696     1  0.0404      0.486 0.988 0.000 0.012 0.000 0.000
#&gt; GSM317697     1  0.6525      0.417 0.640 0.156 0.128 0.004 0.072
#&gt; GSM317698     1  0.1845      0.510 0.928 0.000 0.016 0.000 0.056
#&gt; GSM317650     2  0.2732      0.912 0.000 0.840 0.000 0.160 0.000
#&gt; GSM317651     5  0.4045      0.696 0.148 0.000 0.056 0.004 0.792
#&gt; GSM317657     4  0.0000      0.568 0.000 0.000 0.000 1.000 0.000
#&gt; GSM317667     4  0.5673      0.315 0.000 0.128 0.188 0.668 0.016
#&gt; GSM317670     4  0.5014      0.373 0.008 0.000 0.412 0.560 0.020
#&gt; GSM317674     1  0.0404      0.504 0.988 0.000 0.000 0.000 0.012
#&gt; GSM317675     1  0.0510      0.481 0.984 0.000 0.016 0.000 0.000
#&gt; GSM317677     1  0.9780      0.169 0.308 0.160 0.184 0.192 0.156
#&gt; GSM317678     2  0.6519      0.576 0.000 0.552 0.032 0.300 0.116
#&gt; GSM317687     4  0.9473      0.163 0.160 0.160 0.184 0.372 0.124
#&gt; GSM317695     3  0.4150      0.991 0.388 0.000 0.612 0.000 0.000
#&gt; GSM317653     5  0.4240      0.671 0.048 0.004 0.032 0.104 0.812
#&gt; GSM317656     1  0.4108     -0.101 0.684 0.000 0.308 0.000 0.008
#&gt; GSM317658     4  0.6198      0.402 0.068 0.000 0.344 0.552 0.036
#&gt; GSM317660     5  0.2660      0.723 0.128 0.000 0.008 0.000 0.864
#&gt; GSM317663     4  0.0000      0.568 0.000 0.000 0.000 1.000 0.000
#&gt; GSM317664     1  0.0963      0.457 0.964 0.000 0.036 0.000 0.000
#&gt; GSM317665     5  0.3093      0.735 0.168 0.000 0.008 0.000 0.824
#&gt; GSM317673     1  0.1571      0.510 0.936 0.000 0.004 0.000 0.060
#&gt; GSM317686     4  0.5673      0.315 0.000 0.128 0.188 0.668 0.016
#&gt; GSM317688     1  0.2795      0.476 0.880 0.000 0.056 0.000 0.064
#&gt; GSM317690     4  0.5218      0.375 0.000 0.072 0.296 0.632 0.000
#&gt; GSM317654     5  0.5477      0.342 0.396 0.000 0.056 0.004 0.544
#&gt; GSM317655     4  0.0000      0.568 0.000 0.000 0.000 1.000 0.000
#&gt; GSM317659     1  0.9842      0.101 0.280 0.160 0.184 0.220 0.156
#&gt; GSM317661     2  0.2732      0.912 0.000 0.840 0.000 0.160 0.000
#&gt; GSM317662     2  0.2732      0.912 0.000 0.840 0.000 0.160 0.000
#&gt; GSM317668     1  0.4564     -0.303 0.612 0.000 0.372 0.000 0.016
#&gt; GSM317669     1  0.4380     -0.111 0.676 0.000 0.304 0.000 0.020
#&gt; GSM317671     3  0.4138      0.996 0.384 0.000 0.616 0.000 0.000
#&gt; GSM317676     4  0.7733      0.355 0.084 0.160 0.156 0.560 0.040
#&gt; GSM317680     1  0.4307     -0.674 0.504 0.000 0.496 0.000 0.000
#&gt; GSM317684     1  0.8748      0.327 0.448 0.160 0.184 0.052 0.156
#&gt; GSM317685     1  0.0566      0.502 0.984 0.000 0.004 0.000 0.012
#&gt; GSM317694     1  0.5319      0.431 0.752 0.036 0.072 0.020 0.120
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-4-a').click(function(){
  $('#tab-ATC-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-5'>
<p><a id='tab-ATC-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM317649     3  0.1615      0.775 0.000 0.000 0.928 0.004 0.064 0.004
#&gt; GSM317652     3  0.2738      0.794 0.176 0.000 0.820 0.000 0.004 0.000
#&gt; GSM317666     4  0.5056      0.621 0.148 0.000 0.000 0.632 0.000 0.220
#&gt; GSM317672     5  0.1644      0.894 0.000 0.004 0.076 0.000 0.920 0.000
#&gt; GSM317679     3  0.3403      0.675 0.000 0.000 0.768 0.212 0.000 0.020
#&gt; GSM317681     5  0.0291      0.892 0.000 0.004 0.004 0.000 0.992 0.000
#&gt; GSM317682     5  0.1297      0.905 0.012 0.000 0.040 0.000 0.948 0.000
#&gt; GSM317683     2  0.0000      0.971 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317689     4  0.3966      0.685 0.000 0.032 0.056 0.792 0.120 0.000
#&gt; GSM317691     1  0.2442      0.796 0.884 0.000 0.000 0.048 0.000 0.068
#&gt; GSM317692     5  0.2318      0.876 0.044 0.000 0.064 0.000 0.892 0.000
#&gt; GSM317693     1  0.3798      0.731 0.788 0.000 0.068 0.008 0.136 0.000
#&gt; GSM317696     3  0.2597      0.793 0.176 0.000 0.824 0.000 0.000 0.000
#&gt; GSM317697     1  0.4315      0.698 0.744 0.000 0.144 0.008 0.104 0.000
#&gt; GSM317698     3  0.3860      0.729 0.236 0.000 0.728 0.000 0.036 0.000
#&gt; GSM317650     2  0.0000      0.971 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317651     5  0.2094      0.870 0.080 0.000 0.020 0.000 0.900 0.000
#&gt; GSM317657     4  0.3287      0.772 0.012 0.000 0.000 0.768 0.000 0.220
#&gt; GSM317667     6  0.0914      1.000 0.000 0.016 0.000 0.016 0.000 0.968
#&gt; GSM317670     4  0.0146      0.743 0.004 0.000 0.000 0.996 0.000 0.000
#&gt; GSM317674     3  0.2597      0.793 0.176 0.000 0.824 0.000 0.000 0.000
#&gt; GSM317675     3  0.2738      0.792 0.176 0.000 0.820 0.000 0.004 0.000
#&gt; GSM317677     1  0.0000      0.811 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317678     2  0.1663      0.882 0.000 0.912 0.000 0.000 0.088 0.000
#&gt; GSM317687     1  0.2212      0.783 0.880 0.000 0.000 0.008 0.000 0.112
#&gt; GSM317695     3  0.3514      0.676 0.004 0.000 0.768 0.208 0.000 0.020
#&gt; GSM317653     5  0.0291      0.892 0.000 0.004 0.004 0.000 0.992 0.000
#&gt; GSM317656     3  0.1387      0.776 0.000 0.000 0.932 0.000 0.068 0.000
#&gt; GSM317658     4  0.1429      0.755 0.004 0.000 0.004 0.940 0.052 0.000
#&gt; GSM317660     5  0.1367      0.906 0.012 0.000 0.044 0.000 0.944 0.000
#&gt; GSM317663     4  0.3287      0.772 0.012 0.000 0.000 0.768 0.000 0.220
#&gt; GSM317664     3  0.2562      0.795 0.172 0.000 0.828 0.000 0.000 0.000
#&gt; GSM317665     5  0.1686      0.905 0.012 0.000 0.064 0.000 0.924 0.000
#&gt; GSM317673     3  0.4002      0.768 0.188 0.000 0.744 0.000 0.068 0.000
#&gt; GSM317686     6  0.0914      1.000 0.000 0.016 0.000 0.016 0.000 0.968
#&gt; GSM317688     3  0.3960      0.775 0.176 0.000 0.752 0.000 0.072 0.000
#&gt; GSM317690     4  0.1498      0.763 0.000 0.032 0.000 0.940 0.028 0.000
#&gt; GSM317654     5  0.3767      0.740 0.132 0.000 0.088 0.000 0.780 0.000
#&gt; GSM317655     4  0.3287      0.772 0.012 0.000 0.000 0.768 0.000 0.220
#&gt; GSM317659     1  0.0000      0.811 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM317661     2  0.0000      0.971 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317662     2  0.0000      0.971 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317668     3  0.4595      0.760 0.096 0.000 0.740 0.140 0.020 0.004
#&gt; GSM317669     3  0.1615      0.775 0.000 0.000 0.928 0.004 0.064 0.004
#&gt; GSM317671     3  0.3403      0.675 0.000 0.000 0.768 0.212 0.000 0.020
#&gt; GSM317676     1  0.3245      0.715 0.800 0.000 0.000 0.028 0.000 0.172
#&gt; GSM317680     3  0.3194      0.709 0.004 0.000 0.808 0.168 0.000 0.020
#&gt; GSM317684     1  0.0146      0.811 0.996 0.000 0.004 0.000 0.000 0.000
#&gt; GSM317685     3  0.2946      0.793 0.176 0.000 0.812 0.000 0.012 0.000
#&gt; GSM317694     1  0.3582      0.506 0.732 0.000 0.252 0.000 0.016 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-5-a').click(function(){
  $('#tab-ATC-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-mclust-signature_compare](figure_cola/ATC-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-mclust-collect-classes](figure_cola/ATC-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> ATC:mclust 50            0.714 2
#> ATC:mclust 45            0.687 3
#> ATC:mclust 19               NA 4
#> ATC:mclust 24            0.827 5
#> ATC:mclust 50            0.656 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "NMF"]
# you can also extract it by
# res = res_list["ATC:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 11993 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-NMF-collect-plots](figure_cola/ATC-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-NMF-select-partition-number](figure_cola/ATC-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.951       0.981         0.4307 0.571   0.571
#> 3 3 0.475           0.650       0.795         0.4308 0.750   0.586
#> 4 4 0.477           0.574       0.744         0.1726 0.784   0.499
#> 5 5 0.707           0.647       0.827         0.0870 0.842   0.501
#> 6 6 0.698           0.628       0.812         0.0449 0.885   0.557
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-classes'>
<ul>
<li><a href='#tab-ATC-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-NMF-get-classes-1'>
<p><a id='tab-ATC-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM317649     1  0.0000      0.983 1.000 0.000
#&gt; GSM317652     1  0.0000      0.983 1.000 0.000
#&gt; GSM317666     2  0.0000      0.972 0.000 1.000
#&gt; GSM317672     1  0.9977      0.054 0.528 0.472
#&gt; GSM317679     1  0.0000      0.983 1.000 0.000
#&gt; GSM317681     2  0.0000      0.972 0.000 1.000
#&gt; GSM317682     1  0.0000      0.983 1.000 0.000
#&gt; GSM317683     2  0.0000      0.972 0.000 1.000
#&gt; GSM317689     2  0.0000      0.972 0.000 1.000
#&gt; GSM317691     1  0.0000      0.983 1.000 0.000
#&gt; GSM317692     1  0.3584      0.912 0.932 0.068
#&gt; GSM317693     1  0.0000      0.983 1.000 0.000
#&gt; GSM317696     1  0.0000      0.983 1.000 0.000
#&gt; GSM317697     1  0.0000      0.983 1.000 0.000
#&gt; GSM317698     1  0.0000      0.983 1.000 0.000
#&gt; GSM317650     2  0.0000      0.972 0.000 1.000
#&gt; GSM317651     1  0.0000      0.983 1.000 0.000
#&gt; GSM317657     2  0.5294      0.857 0.120 0.880
#&gt; GSM317667     2  0.0000      0.972 0.000 1.000
#&gt; GSM317670     1  0.0000      0.983 1.000 0.000
#&gt; GSM317674     1  0.0000      0.983 1.000 0.000
#&gt; GSM317675     1  0.0000      0.983 1.000 0.000
#&gt; GSM317677     1  0.0000      0.983 1.000 0.000
#&gt; GSM317678     2  0.0000      0.972 0.000 1.000
#&gt; GSM317687     1  0.0000      0.983 1.000 0.000
#&gt; GSM317695     1  0.0000      0.983 1.000 0.000
#&gt; GSM317653     2  0.8267      0.652 0.260 0.740
#&gt; GSM317656     1  0.0000      0.983 1.000 0.000
#&gt; GSM317658     1  0.0376      0.980 0.996 0.004
#&gt; GSM317660     1  0.0000      0.983 1.000 0.000
#&gt; GSM317663     2  0.0376      0.969 0.004 0.996
#&gt; GSM317664     1  0.0000      0.983 1.000 0.000
#&gt; GSM317665     1  0.0000      0.983 1.000 0.000
#&gt; GSM317673     1  0.0000      0.983 1.000 0.000
#&gt; GSM317686     2  0.0000      0.972 0.000 1.000
#&gt; GSM317688     1  0.0000      0.983 1.000 0.000
#&gt; GSM317690     2  0.0000      0.972 0.000 1.000
#&gt; GSM317654     1  0.0000      0.983 1.000 0.000
#&gt; GSM317655     2  0.0000      0.972 0.000 1.000
#&gt; GSM317659     1  0.0000      0.983 1.000 0.000
#&gt; GSM317661     2  0.0000      0.972 0.000 1.000
#&gt; GSM317662     2  0.0000      0.972 0.000 1.000
#&gt; GSM317668     1  0.0000      0.983 1.000 0.000
#&gt; GSM317669     1  0.0000      0.983 1.000 0.000
#&gt; GSM317671     1  0.0000      0.983 1.000 0.000
#&gt; GSM317676     1  0.0000      0.983 1.000 0.000
#&gt; GSM317680     1  0.0000      0.983 1.000 0.000
#&gt; GSM317684     1  0.0000      0.983 1.000 0.000
#&gt; GSM317685     1  0.0000      0.983 1.000 0.000
#&gt; GSM317694     1  0.0000      0.983 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-1-a').click(function(){
  $('#tab-ATC-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-2'>
<p><a id='tab-ATC-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM317649     1  0.3644      0.736 0.872 0.124 0.004
#&gt; GSM317652     1  0.1411      0.772 0.964 0.036 0.000
#&gt; GSM317666     3  0.2796      0.509 0.000 0.092 0.908
#&gt; GSM317672     2  0.6225      0.139 0.432 0.568 0.000
#&gt; GSM317679     1  0.4589      0.701 0.820 0.172 0.008
#&gt; GSM317681     2  0.3267      0.681 0.116 0.884 0.000
#&gt; GSM317682     1  0.5216      0.601 0.740 0.260 0.000
#&gt; GSM317683     2  0.3619      0.774 0.000 0.864 0.136
#&gt; GSM317689     2  0.4504      0.754 0.000 0.804 0.196
#&gt; GSM317691     3  0.6062      0.529 0.384 0.000 0.616
#&gt; GSM317692     1  0.6854      0.634 0.740 0.124 0.136
#&gt; GSM317693     1  0.6062      0.257 0.616 0.000 0.384
#&gt; GSM317696     1  0.3116      0.764 0.892 0.000 0.108
#&gt; GSM317697     1  0.4235      0.718 0.824 0.000 0.176
#&gt; GSM317698     1  0.4178      0.722 0.828 0.000 0.172
#&gt; GSM317650     2  0.1964      0.768 0.000 0.944 0.056
#&gt; GSM317651     1  0.2537      0.758 0.920 0.080 0.000
#&gt; GSM317657     3  0.1620      0.595 0.012 0.024 0.964
#&gt; GSM317667     2  0.5835      0.657 0.000 0.660 0.340
#&gt; GSM317670     3  0.5650      0.639 0.312 0.000 0.688
#&gt; GSM317674     1  0.3619      0.751 0.864 0.000 0.136
#&gt; GSM317675     1  0.3941      0.736 0.844 0.000 0.156
#&gt; GSM317677     3  0.5497      0.673 0.292 0.000 0.708
#&gt; GSM317678     2  0.1525      0.735 0.032 0.964 0.004
#&gt; GSM317687     3  0.4702      0.714 0.212 0.000 0.788
#&gt; GSM317695     1  0.3340      0.761 0.880 0.000 0.120
#&gt; GSM317653     2  0.5327      0.498 0.272 0.728 0.000
#&gt; GSM317656     1  0.1031      0.780 0.976 0.000 0.024
#&gt; GSM317658     1  0.4749      0.721 0.816 0.012 0.172
#&gt; GSM317660     1  0.5859      0.449 0.656 0.344 0.000
#&gt; GSM317663     3  0.3412      0.462 0.000 0.124 0.876
#&gt; GSM317664     1  0.3551      0.753 0.868 0.000 0.132
#&gt; GSM317665     1  0.5216      0.601 0.740 0.260 0.000
#&gt; GSM317673     1  0.1031      0.780 0.976 0.000 0.024
#&gt; GSM317686     2  0.5810      0.661 0.000 0.664 0.336
#&gt; GSM317688     1  0.0661      0.778 0.988 0.008 0.004
#&gt; GSM317690     2  0.5363      0.710 0.000 0.724 0.276
#&gt; GSM317654     1  0.3193      0.750 0.896 0.100 0.004
#&gt; GSM317655     3  0.3412      0.461 0.000 0.124 0.876
#&gt; GSM317659     3  0.5431      0.680 0.284 0.000 0.716
#&gt; GSM317661     2  0.3116      0.777 0.000 0.892 0.108
#&gt; GSM317662     2  0.3412      0.776 0.000 0.876 0.124
#&gt; GSM317668     1  0.4605      0.684 0.796 0.000 0.204
#&gt; GSM317669     1  0.3752      0.720 0.856 0.144 0.000
#&gt; GSM317671     1  0.2400      0.773 0.932 0.004 0.064
#&gt; GSM317676     3  0.4555      0.715 0.200 0.000 0.800
#&gt; GSM317680     1  0.2955      0.760 0.912 0.080 0.008
#&gt; GSM317684     3  0.6225      0.407 0.432 0.000 0.568
#&gt; GSM317685     1  0.3116      0.764 0.892 0.000 0.108
#&gt; GSM317694     1  0.6274     -0.059 0.544 0.000 0.456
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-2-a').click(function(){
  $('#tab-ATC-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-3'>
<p><a id='tab-ATC-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM317649     3  0.3668     0.7244 0.188 0.004 0.808 0.000
#&gt; GSM317652     1  0.3853     0.6134 0.820 0.020 0.160 0.000
#&gt; GSM317666     4  0.0844     0.6155 0.004 0.004 0.012 0.980
#&gt; GSM317672     2  0.7248     0.3388 0.184 0.532 0.284 0.000
#&gt; GSM317679     3  0.3950     0.7356 0.184 0.004 0.804 0.008
#&gt; GSM317681     2  0.7632     0.3690 0.132 0.504 0.344 0.020
#&gt; GSM317682     1  0.6350     0.4617 0.612 0.092 0.296 0.000
#&gt; GSM317683     2  0.1557     0.7267 0.000 0.944 0.000 0.056
#&gt; GSM317689     2  0.2345     0.7134 0.000 0.900 0.000 0.100
#&gt; GSM317691     4  0.5558     0.5155 0.432 0.000 0.020 0.548
#&gt; GSM317692     1  0.3619     0.6631 0.860 0.004 0.036 0.100
#&gt; GSM317693     1  0.2973     0.5937 0.856 0.000 0.000 0.144
#&gt; GSM317696     1  0.1661     0.6898 0.944 0.000 0.052 0.004
#&gt; GSM317697     1  0.2124     0.6791 0.924 0.000 0.008 0.068
#&gt; GSM317698     1  0.1302     0.6987 0.956 0.000 0.000 0.044
#&gt; GSM317650     2  0.0921     0.7285 0.000 0.972 0.000 0.028
#&gt; GSM317651     1  0.5110     0.5419 0.688 0.012 0.292 0.008
#&gt; GSM317657     4  0.3903     0.7005 0.156 0.008 0.012 0.824
#&gt; GSM317667     2  0.6134     0.4712 0.000 0.508 0.048 0.444
#&gt; GSM317670     3  0.7381     0.6425 0.228 0.088 0.620 0.064
#&gt; GSM317674     1  0.0804     0.7060 0.980 0.000 0.008 0.012
#&gt; GSM317675     1  0.1059     0.7016 0.972 0.000 0.012 0.016
#&gt; GSM317677     4  0.5257     0.5082 0.444 0.000 0.008 0.548
#&gt; GSM317678     2  0.0336     0.7211 0.000 0.992 0.008 0.000
#&gt; GSM317687     4  0.4008     0.7122 0.244 0.000 0.000 0.756
#&gt; GSM317695     3  0.5331     0.7021 0.332 0.000 0.644 0.024
#&gt; GSM317653     1  0.9045     0.1000 0.384 0.268 0.284 0.064
#&gt; GSM317656     3  0.4776     0.6660 0.376 0.000 0.624 0.000
#&gt; GSM317658     2  0.9651    -0.0426 0.284 0.360 0.196 0.160
#&gt; GSM317660     3  0.7852    -0.1708 0.332 0.276 0.392 0.000
#&gt; GSM317663     4  0.1739     0.6041 0.008 0.024 0.016 0.952
#&gt; GSM317664     1  0.2124     0.6637 0.924 0.000 0.068 0.008
#&gt; GSM317665     1  0.7449     0.2552 0.464 0.180 0.356 0.000
#&gt; GSM317673     1  0.1118     0.7029 0.964 0.000 0.036 0.000
#&gt; GSM317686     2  0.6108     0.4973 0.000 0.528 0.048 0.424
#&gt; GSM317688     1  0.2011     0.6860 0.920 0.000 0.080 0.000
#&gt; GSM317690     2  0.5477     0.6256 0.000 0.728 0.092 0.180
#&gt; GSM317654     1  0.5972     0.4838 0.632 0.064 0.304 0.000
#&gt; GSM317655     4  0.0844     0.6137 0.004 0.012 0.004 0.980
#&gt; GSM317659     4  0.4925     0.5342 0.428 0.000 0.000 0.572
#&gt; GSM317661     2  0.3037     0.7216 0.000 0.888 0.076 0.036
#&gt; GSM317662     2  0.2124     0.7299 0.000 0.932 0.028 0.040
#&gt; GSM317668     3  0.6324     0.6336 0.340 0.000 0.584 0.076
#&gt; GSM317669     3  0.3577     0.6944 0.156 0.012 0.832 0.000
#&gt; GSM317671     3  0.5297     0.7201 0.292 0.000 0.676 0.032
#&gt; GSM317676     4  0.4353     0.7176 0.232 0.000 0.012 0.756
#&gt; GSM317680     3  0.3636     0.7287 0.172 0.000 0.820 0.008
#&gt; GSM317684     1  0.4500     0.2473 0.684 0.000 0.000 0.316
#&gt; GSM317685     1  0.1022     0.7038 0.968 0.000 0.032 0.000
#&gt; GSM317694     1  0.4456     0.2866 0.716 0.000 0.004 0.280
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-3-a').click(function(){
  $('#tab-ATC-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-4'>
<p><a id='tab-ATC-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM317649     3  0.2966     0.5857 0.000 0.000 0.816 0.000 0.184
#&gt; GSM317652     5  0.6281     0.2820 0.388 0.000 0.152 0.000 0.460
#&gt; GSM317666     4  0.1704     0.7939 0.000 0.004 0.000 0.928 0.068
#&gt; GSM317672     5  0.5498     0.3387 0.080 0.340 0.000 0.000 0.580
#&gt; GSM317679     3  0.1908     0.6517 0.000 0.000 0.908 0.000 0.092
#&gt; GSM317681     5  0.0854     0.6894 0.000 0.012 0.004 0.008 0.976
#&gt; GSM317682     1  0.4171     0.1944 0.604 0.000 0.000 0.000 0.396
#&gt; GSM317683     2  0.0000     0.8652 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317689     2  0.0000     0.8652 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317691     1  0.6580     0.3162 0.484 0.000 0.172 0.336 0.008
#&gt; GSM317692     1  0.1836     0.8041 0.932 0.000 0.000 0.036 0.032
#&gt; GSM317693     1  0.1043     0.8126 0.960 0.000 0.000 0.040 0.000
#&gt; GSM317696     1  0.0290     0.8135 0.992 0.000 0.000 0.000 0.008
#&gt; GSM317697     1  0.1043     0.8113 0.960 0.000 0.000 0.040 0.000
#&gt; GSM317698     1  0.0000     0.8151 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317650     2  0.0000     0.8652 0.000 1.000 0.000 0.000 0.000
#&gt; GSM317651     1  0.4546     0.0595 0.532 0.000 0.000 0.008 0.460
#&gt; GSM317657     4  0.4422     0.6497 0.028 0.008 0.188 0.764 0.012
#&gt; GSM317667     4  0.5690     0.5455 0.000 0.224 0.000 0.624 0.152
#&gt; GSM317670     3  0.6494     0.3217 0.028 0.296 0.576 0.088 0.012
#&gt; GSM317674     1  0.0000     0.8151 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317675     1  0.0000     0.8151 1.000 0.000 0.000 0.000 0.000
#&gt; GSM317677     1  0.4583     0.6465 0.704 0.000 0.036 0.256 0.004
#&gt; GSM317678     2  0.1671     0.8343 0.000 0.924 0.000 0.000 0.076
#&gt; GSM317687     4  0.1329     0.7913 0.032 0.000 0.008 0.956 0.004
#&gt; GSM317695     3  0.0290     0.6533 0.000 0.000 0.992 0.008 0.000
#&gt; GSM317653     5  0.2757     0.6736 0.032 0.008 0.000 0.072 0.888
#&gt; GSM317656     3  0.4517     0.1640 0.436 0.000 0.556 0.000 0.008
#&gt; GSM317658     2  0.4507     0.4879 0.292 0.684 0.000 0.012 0.012
#&gt; GSM317660     5  0.3489     0.7056 0.036 0.000 0.144 0.000 0.820
#&gt; GSM317663     4  0.2728     0.7897 0.000 0.004 0.068 0.888 0.040
#&gt; GSM317664     1  0.1399     0.8062 0.952 0.000 0.020 0.028 0.000
#&gt; GSM317665     5  0.3732     0.6834 0.032 0.000 0.176 0.000 0.792
#&gt; GSM317673     1  0.0771     0.8104 0.976 0.000 0.000 0.004 0.020
#&gt; GSM317686     4  0.5920     0.4819 0.000 0.272 0.000 0.580 0.148
#&gt; GSM317688     1  0.0703     0.8077 0.976 0.000 0.000 0.000 0.024
#&gt; GSM317690     2  0.0290     0.8617 0.000 0.992 0.000 0.000 0.008
#&gt; GSM317654     5  0.3934     0.7061 0.076 0.000 0.124 0.000 0.800
#&gt; GSM317655     4  0.0992     0.8039 0.000 0.008 0.000 0.968 0.024
#&gt; GSM317659     1  0.4288     0.5026 0.612 0.000 0.004 0.384 0.000
#&gt; GSM317661     2  0.3796     0.6079 0.000 0.700 0.000 0.000 0.300
#&gt; GSM317662     2  0.1908     0.8316 0.000 0.908 0.000 0.000 0.092
#&gt; GSM317668     3  0.6046     0.2952 0.332 0.000 0.552 0.108 0.008
#&gt; GSM317669     3  0.3796     0.4169 0.000 0.000 0.700 0.000 0.300
#&gt; GSM317671     3  0.0510     0.6585 0.000 0.000 0.984 0.000 0.016
#&gt; GSM317676     4  0.1750     0.7791 0.036 0.000 0.028 0.936 0.000
#&gt; GSM317680     3  0.2230     0.6416 0.000 0.000 0.884 0.000 0.116
#&gt; GSM317684     1  0.3424     0.6842 0.760 0.000 0.000 0.240 0.000
#&gt; GSM317685     1  0.0771     0.8104 0.976 0.000 0.000 0.004 0.020
#&gt; GSM317694     1  0.3991     0.7131 0.780 0.000 0.048 0.172 0.000
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-4-a').click(function(){
  $('#tab-ATC-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-5'>
<p><a id='tab-ATC-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM317649     3  0.2583     0.8173 0.032 0.000 0.896 0.008 0.044 0.020
#&gt; GSM317652     1  0.6927     0.1261 0.436 0.000 0.248 0.012 0.264 0.040
#&gt; GSM317666     6  0.3950     0.1806 0.000 0.000 0.000 0.432 0.004 0.564
#&gt; GSM317672     5  0.4667     0.5674 0.036 0.280 0.000 0.004 0.664 0.016
#&gt; GSM317679     3  0.0862     0.8563 0.000 0.000 0.972 0.016 0.008 0.004
#&gt; GSM317681     5  0.1080     0.8080 0.000 0.004 0.000 0.004 0.960 0.032
#&gt; GSM317682     1  0.4631     0.4926 0.644 0.008 0.000 0.020 0.312 0.016
#&gt; GSM317683     2  0.0260     0.8756 0.000 0.992 0.000 0.000 0.000 0.008
#&gt; GSM317689     2  0.2925     0.7789 0.000 0.832 0.000 0.148 0.016 0.004
#&gt; GSM317691     4  0.4454     0.4464 0.232 0.000 0.048 0.704 0.000 0.016
#&gt; GSM317692     1  0.6439     0.3482 0.516 0.016 0.004 0.292 0.152 0.020
#&gt; GSM317693     1  0.3488     0.6744 0.764 0.000 0.000 0.216 0.004 0.016
#&gt; GSM317696     1  0.1223     0.7866 0.960 0.000 0.012 0.016 0.004 0.008
#&gt; GSM317697     1  0.2762     0.7567 0.864 0.012 0.000 0.108 0.004 0.012
#&gt; GSM317698     1  0.0260     0.7848 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; GSM317650     2  0.0508     0.8762 0.000 0.984 0.000 0.000 0.004 0.012
#&gt; GSM317651     5  0.3593     0.6734 0.164 0.000 0.000 0.044 0.788 0.004
#&gt; GSM317657     4  0.2195     0.5064 0.004 0.004 0.056 0.908 0.000 0.028
#&gt; GSM317667     6  0.2984     0.7110 0.000 0.064 0.000 0.064 0.012 0.860
#&gt; GSM317670     4  0.7015     0.0251 0.016 0.364 0.132 0.428 0.008 0.052
#&gt; GSM317674     1  0.1414     0.7791 0.952 0.000 0.020 0.012 0.004 0.012
#&gt; GSM317675     1  0.1364     0.7773 0.952 0.000 0.020 0.016 0.000 0.012
#&gt; GSM317677     1  0.3688     0.6914 0.768 0.000 0.008 0.196 0.000 0.028
#&gt; GSM317678     2  0.1693     0.8655 0.000 0.932 0.000 0.004 0.044 0.020
#&gt; GSM317687     4  0.4178     0.2132 0.020 0.000 0.012 0.684 0.000 0.284
#&gt; GSM317695     3  0.0692     0.8555 0.000 0.000 0.976 0.020 0.000 0.004
#&gt; GSM317653     5  0.0777     0.8079 0.000 0.000 0.000 0.004 0.972 0.024
#&gt; GSM317656     3  0.6255     0.2692 0.360 0.000 0.504 0.040 0.024 0.072
#&gt; GSM317658     2  0.2946     0.7072 0.176 0.812 0.000 0.000 0.000 0.012
#&gt; GSM317660     5  0.1350     0.8083 0.008 0.000 0.020 0.000 0.952 0.020
#&gt; GSM317663     4  0.5355    -0.2306 0.000 0.000 0.092 0.456 0.004 0.448
#&gt; GSM317664     1  0.1944     0.7680 0.924 0.000 0.036 0.024 0.000 0.016
#&gt; GSM317665     5  0.2169     0.7825 0.008 0.000 0.080 0.000 0.900 0.012
#&gt; GSM317673     1  0.1894     0.7817 0.928 0.004 0.000 0.040 0.016 0.012
#&gt; GSM317686     6  0.3039     0.7049 0.000 0.084 0.000 0.056 0.008 0.852
#&gt; GSM317688     1  0.4273     0.6918 0.800 0.000 0.040 0.060 0.028 0.072
#&gt; GSM317690     2  0.2113     0.8524 0.000 0.912 0.008 0.032 0.000 0.048
#&gt; GSM317654     5  0.1458     0.8098 0.016 0.000 0.016 0.000 0.948 0.020
#&gt; GSM317655     4  0.2573     0.4577 0.000 0.000 0.008 0.856 0.004 0.132
#&gt; GSM317659     4  0.3564     0.4234 0.264 0.000 0.000 0.724 0.000 0.012
#&gt; GSM317661     5  0.5371     0.3008 0.000 0.360 0.000 0.000 0.520 0.120
#&gt; GSM317662     2  0.2563     0.8351 0.000 0.876 0.000 0.000 0.052 0.072
#&gt; GSM317668     4  0.6610     0.3312 0.152 0.000 0.204 0.560 0.016 0.068
#&gt; GSM317669     3  0.1757     0.8306 0.000 0.000 0.916 0.000 0.076 0.008
#&gt; GSM317671     3  0.0547     0.8573 0.000 0.000 0.980 0.020 0.000 0.000
#&gt; GSM317676     4  0.1910     0.4790 0.000 0.000 0.000 0.892 0.000 0.108
#&gt; GSM317680     3  0.1251     0.8562 0.000 0.000 0.956 0.012 0.024 0.008
#&gt; GSM317684     1  0.3954     0.4565 0.620 0.000 0.004 0.372 0.004 0.000
#&gt; GSM317685     1  0.1707     0.7829 0.928 0.000 0.004 0.056 0.012 0.000
#&gt; GSM317694     1  0.2540     0.7667 0.872 0.000 0.020 0.104 0.000 0.004
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-5-a').click(function(){
  $('#tab-ATC-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-NMF-signature_compare](figure_cola/ATC-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-NMF-collect-classes](figure_cola/ATC-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n disease.state(p) k
#> ATC:NMF 49            0.869 2
#> ATC:NMF 42            0.568 3
#> ATC:NMF 38            0.798 4
#> ATC:NMF 39            0.705 5
#> ATC:NMF 35            0.763 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

## Session info


```r
sessionInfo()
```

```
#> R version 3.6.0 (2019-04-26)
#> Platform: x86_64-pc-linux-gnu (64-bit)
#> Running under: CentOS Linux 7 (Core)
#> 
#> Matrix products: default
#> BLAS:   /usr/lib64/libblas.so.3.4.2
#> LAPACK: /usr/lib64/liblapack.so.3.4.2
#> 
#> locale:
#>  [1] LC_CTYPE=en_GB.UTF-8       LC_NUMERIC=C               LC_TIME=en_GB.UTF-8       
#>  [4] LC_COLLATE=en_GB.UTF-8     LC_MONETARY=en_GB.UTF-8    LC_MESSAGES=en_GB.UTF-8   
#>  [7] LC_PAPER=en_GB.UTF-8       LC_NAME=C                  LC_ADDRESS=C              
#> [10] LC_TELEPHONE=C             LC_MEASUREMENT=en_GB.UTF-8 LC_IDENTIFICATION=C       
#> 
#> attached base packages:
#> [1] grid      stats     graphics  grDevices utils     datasets  methods   base     
#> 
#> other attached packages:
#> [1] genefilter_1.66.0    ComplexHeatmap_2.3.1 markdown_1.1         knitr_1.26          
#> [5] GetoptLong_0.1.7     cola_1.3.2          
#> 
#> loaded via a namespace (and not attached):
#>  [1] circlize_0.4.8       shape_1.4.4          xfun_0.11            slam_0.1-46         
#>  [5] lattice_0.20-38      splines_3.6.0        colorspace_1.4-1     vctrs_0.2.0         
#>  [9] stats4_3.6.0         blob_1.2.0           XML_3.98-1.20        survival_2.44-1.1   
#> [13] rlang_0.4.2          pillar_1.4.2         DBI_1.0.0            BiocGenerics_0.30.0 
#> [17] bit64_0.9-7          RColorBrewer_1.1-2   matrixStats_0.55.0   stringr_1.4.0       
#> [21] GlobalOptions_0.1.1  evaluate_0.14        memoise_1.1.0        Biobase_2.44.0      
#> [25] IRanges_2.18.3       parallel_3.6.0       AnnotationDbi_1.46.1 highr_0.8           
#> [29] Rcpp_1.0.3           xtable_1.8-4         backports_1.1.5      S4Vectors_0.22.1    
#> [33] annotate_1.62.0      skmeans_0.2-11       bit_1.1-14           microbenchmark_1.4-7
#> [37] brew_1.0-6           impute_1.58.0        rjson_0.2.20         png_0.1-7           
#> [41] digest_0.6.23        stringi_1.4.3        polyclip_1.10-0      clue_0.3-57         
#> [45] tools_3.6.0          bitops_1.0-6         magrittr_1.5         eulerr_6.0.0        
#> [49] RCurl_1.95-4.12      RSQLite_2.1.4        tibble_2.1.3         cluster_2.1.0       
#> [53] crayon_1.3.4         pkgconfig_2.0.3      zeallot_0.1.0        Matrix_1.2-17       
#> [57] xml2_1.2.2           httr_1.4.1           R6_2.4.1             mclust_5.4.5        
#> [61] compiler_3.6.0
```


