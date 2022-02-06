# fishpond 2.1.6

* New makeTx2Tss() helper function for allelic analysis
* Re-organizing package for new pkgdown homepage
* Adding vignette shell for allelic analysis

# fishpond 2.0.0

* New loadFry() function, written by Dongze He with
  contributions from Steve Lianoglou and Wes Wilson.
  loadFry() helps users to import and process
  alevin-fry quantification results. Can process
  spliced, unspliced and ambiguous counts separately 
  and flexibly. Has specific output formats designed
  for downstream use with scVelo or velocity analysis.
  See ?loadFry for more details.
* Adding correlation tests: Spearman or Pearson
  correlations of a numeric covariate with the
  log counts, or with the log fold changes across
  pairs. The Spearman correlation test with counts
  was already implemented in the original SAMseq
  method as response type = "Quantitative".
  For new functionality see 'cor' argument in the
  ?swish man page.
* Adding importAllelicCounts() to facilitate importing
  Salmon quantification data against a diploid
  transcriptome. Can import either as a 'wide'
  format or as 'assays'. Leverages tximeta().
  For gene-level summarization, importAllelicCounts()
  can create an appropriate tx2gene table
  with the necessary a1 and a2 suffices,
  and it will automatically set txOut=FALSE, see
  ?importAllelicCounts for more details.
* Added a 'q' argument to plotInfReps to change the
  intervals when making point and line plots.
* Switched the legend of plotInfReps so that
  reference levels will now be on the bottom,
  and non-reference (e.g. treatment) on top.

# fishpond 1.99.18

* Added helper functionality to importAllelicCounts,
  so it will create an appropriate tx2gene table
  with the necessary a1 and a2 suffices,
  and it will automatically set txOut=FALSE.
* Added a 'q' argument to plotInfReps to change the
  intervals when making point and line plots.
* Switched the legend of plotInfReps so that
  reference levels will now be on the bottom,
  and non-reference (e.g. treatment) on top.
* Added loadFry() to process alevin-fry 
  quantification result. Can process spliced, 
  unspliced and ambiguous counts separately 
  and flexibly.

# fishpond 1.99.15

* Adding correlation tests: Spearman or Pearson
  correlations of a numeric covariate with the
  log counts, or with the log fold changes across
  pairs. The Spearman correlation test with counts
  was already implemented in the original SAMseq
  method as response type = "Quantitative".
  For new functionality see 'cor' argument in the
  ?swish man page.
* Adding importAllelicCounts() to facilitate importing
  Salmon quantification data against a diploid
  transcriptome. Can import either as a 'wide'
  format or as 'assays'. Leverages tximeta().

# fishpond 1.9.6

* Specifying ties.method in matrixStats::rowRanks.

# fishpond 1.9.1

* Added importAllelicCounts() with options for importing
  Salmon quantification on diploid transcriptomes.

# fishpond 1.8.0

* Added note in vignette about how to deal with estimated
  batch factors, e.g. from RUVSeq or SVA. Two strategies are
  outlined: either discretizing the estimate batch factors
  and performing stratified analysis, or regressing out the
  batch-associated variation using limma's removeBatchEffect.
  Demonstation code is included.

# fishpond 1.6.0

* Added makeInfReps() to create pseudo-inferential replicates
  via negative binomial simulation from mean and variance
  matrices. Note: the mean and the variance provide the
  _inferential_ distribution per element of the count matrix.
  See preprint for details, doi: 10.1101/2020.07.06.189639.
* Added splitSwish() and addStatsFromCSV(), which can be used
  to distribute running of Swish across a number of jobs
  managed by `Snakemake`. See vignette for description of
  a suggested workflow. For a large single-cell dataset
  with mean and variance summaries of inferential uncertainty,
  splitSwish() avoids generating the inferential replicate
  counts until the data has been split into smaller pieces and
  sent to different jobs, then only the necessary summary
  statistics are gathered and q-values computed by
  addStatsFromCSV().
* plotInfReps() gains many new features to facilitate plotting of
  inferential count distributions for single cells, as quantified
  with alevin and imported with tximport. E.g. allow for numeric
  `x` argument plus grouping with `cov` for showing
  counts over pseudotime across groups of cells. Also added
  `applySF` argument which can be used to divide out a
  size factor, and the `reorder` argument which will re-order
  the samples/cells within groups by the count. plotInfReps()
  will draw boxplots with progressively thinner visual features
  as the number of cells grows to make the plots still legible.

# fishpond 1.5.2

* First version of makeInfReps(), to create pseudo-infReps
  via negative binomial simulation from set of mean and
  variance matrices in the assays of the SummarizedExperiment.

# fishpond 1.4.0

* Added isoformProportions(), which can be run after
  scaleInfReps() and optionally after filtering out
  transcripts using labelKeep(). Running swish() after
  isoformProportions() will produce differential transcript
  usage (DTU) results, instead of differential transcript
  expression (DTE) results. Example in vignette.
* Default number of permutations increased from 30 to 100.
  It was observed that there was too much fluctuation in the
  DE called set for nperms=30 across different seeds, and
  setting to 100 helped to stabilize results across seeds,
  without increasing running time too much. For further reduced
  dependence on the seed, even higher values of nperms
  (e.g. 200, 300) can be used.

# fishpond 1.3.8

* Added isoformProportions(), which can be run after
  scaleInfReps() and optionally after filtering out
  transcripts using labelKeep(). Running swish() after
  isoformProportions() will produce differential transcript
  usage (DTU) results, instead of differential transcript
  expression (DTE) results. Example in vignette.

# fishpond 1.3.4

* Default number of permutations increased from 30 to 100.
  It was observed that there was too much fluctuation in the
  DE called set for nperms=30 across different seeds, and
  setting to 100 helped to stabilize results across seeds,
  without increasing running time too much. For further reduced
  dependence on the seed, even higher values of nperms
  (e.g. 200, 300) can be used.

# fishpond 1.2.0

* Switching to a faster version of Swish which only
  computes the ranks of the data once, and then re-uses
  this for the permutation distribution. This bypasses
  the addition of uniform noise per permutation and
  is 10x faster. Two designs which still require
  re-computation of ranks per permutation are the
  paired analysis and the general interaction analysis.
  Two-group, stratified two-group, and the paired
  interaction analysis now default to the new fast
  method, but the original, slower method can be used
  by setting fast=0 in the call to swish().
* Adding Rcpp-based function readEDS() written by
  Avi Srivastava which imports the sparse counts stored
  in Alevin's Efficient Data Storage (EDS) format.
* Changed the vignette so that it (will) use a linkedTxome,
  as sometime the build would break if the Bioc build
  machine couldn't access ftp.ebi.ac.uk.
* Add 'computeInfRV' function. InfRV is not used in the
  Swish methods, only for visualization purposes in the
  Swish paper.
* removed 'samr' from Imports, as it required source
  installation, moved to Suggests, for optional qvalue
  calculation

# fishpond 0.99.30

* added two interaction tests, described in ?swish
* incorporate qvalue package for pvalue, locfdr and qvalue
* added plotMASwish() to facilitate plotting
* wilcoxP is removed, and the mean is used instead

# fishpond 0.99.0

* fishpond getting ready for submission to Bioc