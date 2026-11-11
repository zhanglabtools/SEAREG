Tutorial data
=============

Each single-section example includes a spot-by-peak ``counts.h5ad`` with spatial coordinates
and a peak-by-motif ``motifs.h5`` annotation matrix. Files are prepared at
startup; motif import or scanning follows peak filtering.
The repository includes these five input pairs and the four-section embryo
count matrix as lossless gzip files, plus the specified named output embeddings.
The complete GitHub clone and Download ZIP contain the actual files.
Wheel and source distributions omit tutorials, data and results. Complete model
checkpoints are not distributed. The file layout is listed below; see
:doc:`workflow` to train on these inputs or analyze the provided embeddings.

The developing mouse brain example (T6) includes
``data/Embryo_multislice/annotations.tsv`` and anatomy palettes.
Its 7,118-spot cohort has 7,037 known annotations and 81 Unknowns. Reference maps
appear after reading counts and before training; labels are joined by spot ID
for display. The model uses counts, shared peak identities, section IDs and
native coordinates. Palette source tables and
``annotation_palette_manuscript_source.json`` record the display provenance.
See :doc:`multislice` for the joint model and reference-display conventions.

.. list-table:: Included datasets
   :header-rows: 1
   :widths: 30 35 35

   * - Dataset
     - Sequencing platform
     - Local directory
   * - Mouse embryo (E13)
     - spatial ATAC–RNA-seq (ATAC modality)
     - ``data/ME13_joint_2187/``
   * - Mouse embryo (E13)
     - spatial-ATAC-seq
     - ``data/ME13_ATAC_2275/``
   * - Mouse brain (P22)
     - spatial ATAC–RNA-seq (ATAC modality)
     - ``data/P22_ATAC/``
   * - Mouse brain (P22)
     - spatial CUT&Tag–RNA-seq (H3K27ac modality)
     - ``data/P22_H3K27ac/``
   * - Human tonsil
     - spatial-ATAC-seq
     - ``data/Tonsil/``
   * - Developing mouse brain (E11.0–E18.5)
     - MISAR-seq (ATAC modality; four sections)
     - ``data/Embryo_multislice/``

Original sequencing studies
---------------------------

Tutorial names follow the technologies and samples described in the original
sequencing papers. The directory names above identify the included defaults;
user data does not need to be registered under one of these names.

.. list-table:: Study sources and model inputs
   :header-rows: 1
   :widths: 14 30 28 28

   * - Tutorial
     - Original study
     - Study accession
     - Modality used for SEAREG training
   * - 1
     - Zhang et al., Nature (2023)
     - GSE205055
     - ATAC, with peak–motif associations and spatial coordinates
   * - 2
     - Deng et al., Nature (2022)
     - GSE171943
     - ATAC, with peak–motif associations and spatial coordinates
   * - 3
     - Zhang et al., Nature (2023)
     - GSE205055
     - ATAC, with peak–motif associations and spatial coordinates
   * - 4
     - Zhang et al., Nature (2023)
     - GSE205055
     - CUT&Tag H3K27ac, with peak–motif associations and spatial coordinates
   * - 5
     - Deng et al., Nature (2022)
     - GSE171943
     - ATAC, with peak–motif associations and spatial coordinates
   * - 6
     - Jiang et al., Nature Methods (2023)
     - NODE OEP003285
     - ATAC, with section identities and spatial coordinates

* `Zhang et al., Spatial epigenome–transcriptome co-profiling of mammalian tissues (2023) <https://doi.org/10.1038/s41586-023-05795-1>`_;
  `GEO GSE205055 <https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE205055>`_.
* `Deng et al., Spatial profiling of chromatin accessibility in mouse and human tissues (2022) <https://doi.org/10.1038/s41586-022-05094-1>`_;
  `GEO GSE171943 <https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE171943>`_.
* `Jiang et al., Simultaneous profiling of spatial gene expression and chromatin accessibility during mouse brain development (2023) <https://doi.org/10.1038/s41592-023-01884-1>`_;
  `NODE OEP003285 <https://www.biosino.org/node/project/detail/OEP003285>`_.

The co-profiling studies measured RNA alongside chromatin. T1–T5 use the
indicated chromatin modality, motif associations and spatial coordinates;
their joint UMAPs display spot, peak and motif nodes. T6 uses ATAC counts,
shared peak identities, section IDs and spatial coordinates without motifs;
its joint UMAP contains spot and shared-peak nodes. Tutorials begin
with processed matrices; FASTQ processing and peak calling precede these inputs.

Local input and bundled output layout
-------------------------------------

Each single-section ``data/<dataset>/`` contains compressed counts and motif
matrices; ``Embryo_multislice`` contains counts without motifs.
P22_ATAC counts use numbered gzip parts. Notebooks prepare the selected counts
and optional motif files before constructing the workflow. Actual motif
processing occurs after filtering. Extraction checks the archive and
original-file identities recorded in ``data/manifest.json``.

Existing ``ATAC_PATH`` and ``MOTIF_PATH`` files are used directly and validated
by the workflow's data checks. They require no example hash or manifest entry.
Missing counts raise an error; missing motifs follow the configured read/scan
policy. Only an exact bundled path can trigger automatic extraction. To
explicitly prepare and verify all bundled inputs:

.. code-block:: bash

   python tools/prepare_data.py --dataset all

The provided spot/peak/motif exports are under
``result/<dataset>/training/embeddings/300_epoch/`` (T1–T2) or ``500_epoch/``
(T3–T5). T6's final adaptive_depth_gate spot/peak exports are at
``result/Embryo_multislice/adaptive_depth_gate/500_epoch/``. The same result directory
contains the saved ``analysis/``, ``figures/`` and ``tutorial_execution.json``.
The receipt identifies the selected counts, trained embeddings and fitted
analysis. Reopening ``analysis/`` reads its labels, graph and UMAP coordinates
without fitting.

The tutorial loader uses the selected directory's own ``export.json`` and
matching counts for validation. Both modes write new analysis under
``OUTPUT_DIR/runs/``; new training keeps its model and exports with the run.
See :doc:`workflow` for choosing a source and recovering native training.

Checkpoints, unpacked bundled inputs and generated default runs are ignored
by Git.

.. _tutorial-evidence:

Preparing evidence for T1 and T7
---------------------------------

T1's Fig. 2h cell uses mm10 ATAC fragments from `GSM6801813 <https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSM6801813>`_
and paired gene counts from `GSM6799937 <https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSM6799937>`_.
T1 also shows how to write the exact local link input from ``F2h_P2G_arcs``
in ``Source_Data_Fig2.xlsx``.

T7 uses P22 RNA counts from `GSM6753043 <https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSM6753043>`_
and `GSM6753045 <https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSM6753045>`_,
and ATAC and H3K27ac fragments from `GSM6758285 <https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSM6758285>`_
and `GSM6704979 <https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSM6704979>`_, respectively.
Use coordinate-sorted, bgzip-compressed fragment files with adjacent Tabix
indexes for the locus plots. Use the matching whole-section fragment totals
per barcode for the two library-size tables. See :doc:`installation` for the readers.

T7 input manifest and coordinates
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Place the :download:`T7 evidence manifest example <../tutorials/configs/P22_regulatory_evidence.example.json>`
in ``data/P22_regulatory/`` and edit its paths to identify your actual files.
Relative paths resolve from the manifest's directory. Set
``SEAREG_P22_EVIDENCE`` to the manifest's absolute path before starting
Jupyter. The example's ``raw_h3`` entry points to the bundled full P22
H3K27ac count matrix (87,043 features). Unpack it from the repository root
with ``python tools/prepare_data.py --dataset P22_H3K27ac``.

Use the saved T3 and T4 analyses obtained in ``provided`` mode. Assign
``SEAREG_T3_RUN`` and ``SEAREG_T4_RUN`` before preparing the two coordinate
tables. The following example combines native XY from those runs with the
adopted domain labels in the repository, matching on spot barcode. The
display prefixes ``A`` (ATAC) and ``H`` (H3K27ac) become ``D`` in T7, with
the domain numbers preserved.

.. code-block:: python

   import os
   from pathlib import Path

   import pandas as pd
   from tutorials.seareg_tutorial_regulatory import spot_keys

   output = Path("data/P22_regulatory")
   output.mkdir(parents=True, exist_ok=True)
   sources = (
       ("SEAREG_T3_RUN", "P22_ATAC", "spatial_atac_domain_coordinates.tsv"),
       ("SEAREG_T4_RUN", "P22_H3K27ac", "h3k27ac_domain_coordinates.tsv"),
   )
   for variable, dataset, filename in sources:
       path = Path(os.environ[variable]) / "analysis" / "spot_metadata.tsv"
       coordinates = pd.read_csv(path, sep="\t", usecols=["spot_id", "x", "y"])
       domains = pd.read_csv(
           Path("tutorials/styles") / f"{dataset}.tsv.gz",
           sep="\t", usecols=["spot_id", "domain"],
       )
       coordinates["spot_id"] = spot_keys(coordinates["spot_id"])
       domains["spot_id"] = spot_keys(domains["spot_id"])
       domains["domain"] = "D" + domains["domain"].str[1:]
       joined = coordinates.merge(domains, on="spot_id", how="outer",
                                  validate="one_to_one", indicator=True)
       if not joined["_merge"].eq("both").all():
           raise ValueError(f"The {dataset} run and adopted labels have different spots")
       joined[["spot_id", "domain", "x", "y"]].to_csv(
           output / filename, sep="\t", index=False, mode="x",
       )

T7 uses separate ATAC and H3K27ac peak-annotation tables. For each region,
annotate the assay's modeled peaks with mm10 coordinates, interval validity,
midpoint distance to the nearest GENCODE vM25 gene-level TSS, and overlap with
the mm10 ENCODE v2 blacklist. ``is_distal`` marks a distance greater than 2 kb.
Keep the original peak IDs. The notebook lists the required field names.

.. _tutorial-archr-links:

Peak-to-gene links from paired counts
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use ArchR 1.0.2 in R 4.4.3 for the manuscript examples. Prepare the mm10
ATAC project from each section's fragments and adopted peak set, retaining
the 2,187 paired E13 spots or the 9,215 paired P22 ATAC spots. Match the raw
RNA counts to the same spot IDs and import them with
``addGeneExpressionMatrix(strictMatch=TRUE)``. Use ``GeneExpressionMatrix``
``NormCounts`` scaled to 10,000, excluding chrM and chrY. The ATAC
``PeakMatrix`` uses counts with ``ceiling=4`` and ``binarize=FALSE``.

Run TileMatrix iterative LSI for 2 iterations with 25,000 variable features
and dimensions 1–30. Use clustering resolution 0.2, ``sampleCells=2000``,
``n.start=10``, and seed 1234. Run
`addPeak2GeneLinks <https://www.archrproject.com/reference/addPeak2GeneLinks.html>`_
with ``reducedDims="IterativeLSI"``, ``useMatrix="GeneExpressionMatrix"``,
``k=100``, ``maxDist=250000``, ``corCutOff=0.75`` and seed 1234. Export
the link table with
`getPeak2GeneLinks <https://www.archrproject.com/reference/getPeak2GeneLinks.html>`_
using ``corCutOff=0.2``, ``FDRCutOff=0.1``, ``varCutOffATAC=0``,
``varCutOffRNA=0`` and ``returnLoops=FALSE``. Join the peak and gene indices
to the matched peak intervals and gene annotations. Save the original
``Correlation`` and analytic ``FDR`` in the TSV schema specified in the
notebook. T7 uses the full P22 link export for candidate-target selection.

T7 sequence and gene annotations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The T7 ``pwm`` entry accepts the P22 ATAC sequence-scan matrix
``freq_motif.h5``. Use mm10 peak sequences and JASPAR 2024 CORE vertebrate
motifs. Scan both strands using motifmatchr, a background estimated from
the pooled sequences, and a match P-value cutoff of :math:`10^{-5}`. Store
the peak-by-motif scores in ``mat``, with ``row_names`` for peak IDs and
``col_names`` for native motif IDs. T7 uses positive entries from the
upstream scan for sequence support. To replay the paper network, supply the
specific scan used in the analysis alongside its motif-to-TF annotation.
The motif-to-TF table pairs each native motif ID with its candidate gene symbols.

Use the `Enrichr GO Biological Process 2023 GMT <https://maayanlab.cloud/Enrichr/geneSetLibrary?mode=text&libraryName=GO_Biological_Process_2023>`_
for ``go_gmt``. For the locus, take the Mal-202 exons and CDS from GENCODE
vM25 and the SOX6 position frequency matrix from JASPAR MA0515.1. Their
required columns and coordinate convention are listed in T7.
