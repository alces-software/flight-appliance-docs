.. _gridware:

Alces Gridware Software Applications
####################################

This page documents the software which is currently available via the Alces Gridware project. Software applications are listed in the Alces Gridware repository with the structure ``repository/type/name/version``, which corresponds to:

 - **repository** - packages are listed in the **main** repository if available for auto-scaling clusters, and the **volatile** repository otherwise. 
 - **type** - packages are listed as **apps** (applications), **libs** (shared libraries), **compilers** or **mpi** (message-passing interface API software for parallel applications)
 - **name** - the name of the software package
 - **version** - the published version of the software package

For example, a package listed as ``main/apps/bowtie2/2.2.6`` is version 2.2.6 of the Bowtie2 application, from the stable repository. 

The following list is updated periodically as new software is added to the repositories. For clarity, multiple versions of the same application are not shown, unless there are significant functional differences between versions. Software categories are provided as a guide only - many packages are multi-discipline and have a range of appropriate uses. 


Tools and Utilities
-------------------
.. code:: bash

  gnuplot 	  A portable command-line driven graphing utility
  grace 	  Grace is a WYSIWYG 2D plotting tool for the X Window System and M*tif
  idr 	  	  Framework to measure the reproducibility of findings
  mawk 	  	  mawk is an interpreter for the AWK Programming Language.
  parallel 	  A shell tool for executing jobs in parallel using one or more computers


Benchmarks
----------

.. code:: bash

  HPL 	  	  A Portable Implementation of the High-Performance Linpack Benchmark for Distributed-Memory Computers
  memtester	  A userspace utility for testing the memory subsystem for faults.
  hpcc	   	  The HPC Challenge benchmark
  imb	  	  IMB is a networking benchmark tool.
  iozone	  IOzone is a filesystem benchmark tool.
  gpuburn	  GPU-Burn is a stress test for multi-GPGPU-setups.
  Folding@home 	  A distributed computing project studying protein folding and misfolding


Biochemistry
------------

.. code:: bash

  amber	  	  Set of programs for biomolecular simulation and analysis
  beast	  	  Cross-platform program for Bayesian MCMC analysis of molecular sequences
  placement	  An algorithm for prediction of explicit solvent atom distribution


BioInformatics
--------------

,, code:: bash

  MS Prot Tools   Some hopefully useful tools for mass spectrometry applied to proteomics
  454 Sequencing  454 Sequencing System Off-Instrument Software Applications suite
  ABRA 	  	  Assembly Based ReAligner
  'AbySS 	  A de novo, parallel, paired-end sequence assembler that is designed for short reads
  ANGES 	  Reconstucts ancestral genome maps from homologous markers in extant related genomes
  ANNOVAR 	  Functional annotation of genetic variants from high-throughput sequencing data
  ARAGORN 	  Detect tRNA genes and tmRNA genes in nucleotide sequences
  ArtificialFqG	  Ouputs artificial FASTQ files derived from a reference genome
  Atlas-SNP2 	  Next-generation sequencing suite of variant analysis tools
  AUGUSTUS 	  Predicts genes in eukaryotic genomic sequences
  autoadapt 	  Automatically detect and remove adaptors and primers present in a FASTQ file
  bam-readcount   Generate metrics at single nucleotide positions
  BAMStats 	  Tool to calculate and display various metrics derived from SAM/BAM files
  BamTools 	  Provides a programmer's API and an end-user's toolkit for handling BAM files
  bamUtil 	  Several programs that perform operations on SAM/BAM files
  BamView 	  Interactive display of read alignments in BAM data files
  BLAST		  Compares nucleotide or protein sequences to sequence databases and calculates the statistical significance of matches
  Batalign 	  An incremental method for accurate gapped alignment
  BCFtools 	  Utilities for variant calling and manipulating VCF and BCF files
  Bcl2FastQ 	  A tool to handle bcl conversion and demultiplexing
  Bcl2FastQ 	  A tool to handle bcl conversion and demultiplexing
  BCL Converter   Convert *.bcl files into *_qseq.txt files
  BEAGLE 	  A general purpose library for evaluating the likelihood of sequence evolution on trees
  BEAGLE 	  Analysis of large-scale genetic data sets with hundreds of thousands of markers genotyped on thousands of samples
  BEAGLE 	  A general purpose library for evaluating the likelihood of sequence evolution on trees
  BEDTools 	  A flexible suite of utilities for comparing genomic features
  biom-format 	  The Biological Observation Matrix (BIOM) format
  BIONJ 	  An improved version of the NJ algorithm based on a simple model of sequence data
  BioPerl 	  A community effort to produce Perl code which is useful in biology
  Biopython 	  Set of freely available tools for biological computation written in Python
  bio-rainbow 	  Package for RAD-seq related clustering and de novo assembly.
  Bismark 	  A bisulfite read mapper and methylation caller
  BLAST (Legacy)  Compares nucleotide or protein sequences to sequence databases and calculates the statistical significance of matches
  Bowtie 2 	  Fast and sensitive read alignment
  Bowtie 	  Ultrafast memory-efficient short read aligner
  BreakPointer 	  Pinpoint rearrangement breakpoints using paired end reads
  CAP3 	  	  Sequence Assembly Program
  car 	  	  Reconstructing contiguous regions of an ancestral genome
  cdbfasta 	  CDB (Constant DataBase) indexing and retrieval tools for FASTA files
  CD-HIT 	  A program for clustering DNA/protein sequence database at high identity with tolerance.
  cdhit 	  A program for clustering and comparing protein or nucleotide sequences.
  CEGMA 	  Building sets of gene annotations in eukaryotic genomes
  CGAT 	  	  The Computational Genomics Analysis Toolkit
  CHANCE 	  Assess the quality of ChiP-seq experiments
  CHIAMO 	  Call genotypes from the Affymetrix 500K Mapping chip
  Chimerascan 	  Detection of chimeric transcripts in high-throughput sequencing data
  ClonalFrame 	  Inference of bacterial microevolution using multilocus sequence data
  CLUMPP 	  Deals with label switching and multimodality problems in population-genetic cluster analyses
  Clustal Omega	  Multiple alignment of nucleic acid and protein sequences
  ClustalW 	  Multiple alignment of nucleic acid and protein sequences
  cnD 	  	  Copy number variant caller for inbred strains
  CNVnator 	  CNV discovery and genotyping from depth of read mapping
  CoNIFER 	  Copy Number Inference From Exome Reads
  CASAVA 	  Processes sequencing reads provided by RTA or OLB
  CONTIGuator 	  A bacterial genomes finishing tool for structural insights on draft genomes
  Control-FREEC   Detect copy-number changes and allelic imbalances using deep-sequencing data
  CNATR		  Tool for copy number variation (CNV) detection for targeted resequencing data
  Corset 	  Software for clustering de novo assembled transcripts and counting overlapping reads
  Cortex 	  "Software for genome assembly and variation analysis
  CRAMTools 	  Set of Java tools and APIs for efficient compression of sequence read data
  CREST 	  Algorithm for detecting genomic structural variations at base-pair resolution
  CRISP 	  Multi-sample variant caller for high-throughput pooled sequence data
  Curtain 	  Assembler of next generation sequence, developed by Matthias Haimel in the Ensembl Genomes team at the EBI
  cutadapt 	  A tool that removes adapter sequences from DNA sequencing reads
  DARWIN 	  Data Analysis and Retrieval With Indexed Nucleotide/peptide sequences
  DDiMAP 	  Analyses mapped NGS read data to discover rare variants
  dDocent 	  An interactive bash wrapper to QC, assemble, map, and call SNPs from double digest RAD data
  Delly 	  Structural variant discovery by integrated paired-end and split-read analysis
  distruct 	  Graphically display results produced by the genetic clustering program structure
  DREEP 	  Detecting low-level mutations by utilizing the RE-sequencing Error Profile of the data
  EAD 		  Error aware demultiplexer is a probabilistic demultiplexer for Illumina BCL files.
  EIGENSOFT 	  Combines functionality from population genetics methods and EIGENSTRAT stratification method
  EIGENSOFT 	  Combines functionality from population genetics methods and EIGENSTRAT stratification method
  Ensembl API  	  Abstraction layer for accessing Ensembl genomic databases
  Ensembl Variant Effect Predictor 	  Predict the functional consequences of known and unknown variants
  e-PCR 	  Identifies sequence tagged sites (STSs) within DNA sequences
  Exonerate 	  Generic tool for pairwise sequence comparison
  eXpress 	  Streaming tool for quantifying the abundances of a set of target sequences from sampled subsequences
  FamSeq 	  A computational tool for calculating probability of variants in family-based sequencing data
  FASTA 	  Search protein or DNA sequence databases comparing a protein sequence to a DNA sequence database
  FastQC 	  A quality control tool for high throughput sequence data
  fastq-tools 	  Small utilities for working with fastq sequence files
  FastTree 	  Inference of approximately-maximum-likelihood phylogenetic trees from alignments of nucleotide or protein sequences.
  FastUniq 	  an ultrafast de novo duplicates removal tool for paired short DNA sequences
  Flexbar 	  Flexible barcode and adapter removal for sequencing platforms
  FreeBayes 	  Bayesian genetic variant detector designed to find small polymorphisms
  FREGENE 	  Simulates sequence-like data over large genomic regions in large diploid populations
  FusionFinder 	  Find fusion transcript candidates in RNA-Seq data
  FusionMap 	  Align reads spanning fusion junctions directly to the genome
  Galaxy 	  Open, web-based platform for data intensive biomedical research
  GBrowse 	  The Generic Genome Browser
  geneid 	  Predicts genes in anonymous genomic sequences designed with a hierarchical structure
  GeneTorrent 	  Transfer genomic data reliably across a network
  GATK 	  	  Software package developed at the Broad Institute to analyse next-generation resequencing data
  GAT + queue 	  Broad Institute package for analysing next-generation resequencing data; including command-line scripting framework for defining multi-stage genomic analysis pipelines
  GenomeMapper 	  Short read mapping tool designed for accurate read alignments
  GENSCAN 	  Analyze genomic DNA sequences from a variety of organisms
  GERP++ 	  Identifies constrained elements in multiple alignments by quantifying substitution deficits
  GIMSAN 	  GIbbsMarkov with Significance ANalysis
  Glimmer 	  System for finding genes in microbial DNA, especially the genomes of bacteria, archaea, and viruses
  GMAP/GSNAP 	  Genomic mapping and alignment and short-read nucleotide alignment programs
  GREAT 	  Genomic Regions Enrichment of Annotations Tool
  Grinder 	  A versatile omics shotgun and amplicon sequencing read simulator
  GTOOL 	  Transforms sets of genotype data for use with the programs SNPTEST and IMPUTE
  HAL 	  	  Hierarchical Alignment Format API and analysis and conversion tools
  hapflk 	  Haplotype-based test for differentiation in multiple populations
  HLA*IMP BE 	  Impute HLA type information based on SNP genotypes back-end
  HLA*IMP FE 	  Impute HLA type information based on SNP genotypes front-end
  HMMcopy 	  Make copy number estimations for whole genome data
  HPCall 	  Improved base-calling for homopolymer-sensitive next-gen data
  HTSeq 	  Process data from high-throughput sequencing assays
  HTSlib 	  C library for high-throughput sequencing data formats
  abacas	  Rapidly contiguate (align, order, orientate), visualize and design primers
  Bio-bwa	  Aligns relatively short nucleotide sequences against a long reference sequence such as the human genome
  bowtie2	  Fast and sensitive read alignment
  bowtie	  Ultrafast memory-efficient short read aligner
  bedtools	  A flexible suite of utilities for comparing genomic features
  phast	  	  Software package for comparative and evolutionary genomics
  cufflinks	  Assembles transcripts, estimates their abundances, and tests for differential expression and regulation in RNA-Seq samples
  emboss	  Software analysis suite developed for the molecular biology community
  genetics	  Reports position-specific measures of conservation
  genome	  An alignment tool like BLAST
  varscan	  Mutation caller for targeted, exome, and whole-genome resequencing data
  breakdancer	  Provides genome-wide detection of structural variants from next generation paired-end sequencing reads
  Genome-music	  A comprehensive analysis suite for mutations in cancer genomes
  radmarkers	  Guppy RAD tools
  fastx	  	  A collection of command line tools for Short-Reads FASTA/FASTQ files preprocessing
  fastx2	  Assaf Gordon text utilities
  hmmer	  	  Biosequence analysis using profile hidden Markov models
  htsfilter	  Standard Filter for identification of polyclonal and independant errors for SOLiD short read sequences
  macs	  	  Novel algorithm for identifying transcript factor binding sites
  sambamba	  Tools for working with SAM/BAM data
  mag	  	  Builds qassembly by mapping short reads to reference sequences
  picard	  Java-based command-line utilities and API for manipulating SAM files
  ribopicker	  Identify and remove rRNA sequences from metagenomic and metatranscriptomic datasets
  samtools	  Provides various utilities for manipulating alignments in the SAM format, including sorting, merging, indexing and generating alignments in a per-position format
  plinkseq	  Library for working with human genetic variation data
  bamview	  Variant detector and alignment viewer for next-generation sequencing data in the SAM/BAM format
  recon	  	  Package for finding repeat families from biological sequences
  Picard-broad	  Command line tools for manipulating high-throughput sequencing (HTS) data and formats
  mabkit	  Tools for common BAM file manipulations
  speedseq	  A flexible framework for rapid genome analysis and interpretation
  fgwas	  	  Functional genomics and genome-wide association studies
  lighter	  Fast and memory-efficient sequencing error corrector
  bamtools3	  Provides a programmer's API and an end-user's toolkit for handling BAM files
  diffreps	  Differential analysis for ChIP-seq with biological replicates
  Macs-taoliu	  Novel algorithm for identifying transcript factor binding sites
  sift	  	  Predicts whether an amino acid substitution affects protein function
  impute	  A genotype imputation and phasing program based on ideas from Howie et al. (2009)
  snpomatic	  Fast, stringent short-read mapping software
  soap	    	  A short read de novo assembly tool
  amos	  	  A collection of tools and class interfaces for the assembly of DNA reads
  bfast	  	  Facilitates the fast and accurate mapping of short reads to reference sequences
  kggseq	  A biological knowledge-based mining platform for genomic and genetic studies using sequence data
  passion	  A pattern growth algorithm based pipeline for splice site detection in paired-end RNA-Seq data
  Cnv-seq	  A method for detecting DNA copy number variation (CNV) using highthroughput sequencing
  tophat	  A spliced read mapper for RNA-Seq
  Tophat-fusion	  Enhanced version of TopHat with the ability to align reads across fusion points
  trinitymaseq	  A novel method for the efficient and robust de novo reconstruction of transcriptomes from RNA-seq data
  vcftools	  Package designed for working with VCF files, such as those generated by the 1000 Genomes Project
  fastq_screen	  A screening application for high throughput sequence data
  gatk	  	  Software package developed at the Broad Institute to analyse next-generation resequencing data
  igv	  	  A high-performance visualization tool for interactive exploration of large, integrated genomic datasets
  scripture	  Java-based command-line tool for transcriptome reconstruction
  bertone	  Subdivision of ChIP-seq/ChIP-chip regions into discrete signal peaks
  oases	  	  De novo transcriptome assembler for very short reads
  velvet	  Sequence assembler for very short reads
  hgsc	   	  SNP discovery tool developed for next generation sequencing platforms
  htslib	  Provides various utilities for manipulating alignments in the SAM format, including sorting, merging, indexing and generating alignments in a per-position format
  bam2fastq	  Extract raw sequences (with qualities)
  mothur	  Provides microbial ecologists with the functionality of dotur, sons, treeclimber, s-libshuff, unifrac and more.
  mutationtaster  Next-generation sequencing pipeline from Mutation Taster (http
  ngsqctoolkit	  A toolkit for the quality control (QC) of next generation sequencing (NGS) data
  w.cgi	  	  Software for performing Bayesian inference Using Gibbs Sampling
  repeatmasker	  Screens DNA sequences for interspersed repeats and low complexity DNA sequences
  artemis	  Genome browser and annotation tool
  dindel	  Calls small indels from next-generation sequence data by realigning reads to candidate haplotypes
  reapr	  	  Evaluates the accuracy of a genome assembly using mapped paired end reads
  smalt	  	  Efficiently aligns DNA sequencing reads with genomic reference sequences
  shapeit	  Segmented HAPlotype Estimation and Imputation Tool - Fast and accurate haplotype inference
  stampy	  Maps short reads from Illumina sequencing machines on to a reference genome
  iassembler	  Assemble ESTs generated using Sanger and/or Roche-454 pyrosequencing technologies into contigs
  Infernal 	  Search DNA sequence databases for RNA structure and sequence similarities
  InterProScan 	  Allows sequences to be scanned against InterPro's signatures
  JAGS 	  	  Analysis of Bayesian hierarchical models using Markov Chain Monte Carlo simulation
  Kent src utils  Jim Kent and the UCSC Genome Bioinformatics Group program suite
  khmer 	  k-mer counting, filtering and graph traversal
  LASTZ 	  Program for aligning DNA sequences, a pairwise aligner
  LifeScope 	  LifeScope Genomic Analysis Solutions Tools
  LOCAS 	  Low-coverage short-read assembler
  LoFreq 	  Fast and sensitive variant-caller for inferring SNVs from high-throughput sequencing data
  LUMPY 	  A probabilistic framework for structural variant discovery
  MAFFT 	  Multiple alignment program for amino acid or nucleotide sequences
  mafJoin 	  Tool for combining pairs of maf files that share a common sequence
  MAKER 	  Portable and easily configurable genome annotation pipeline
  M.A.Q Viewer 	  Graphical read alignement viewer
  MaSuRCA 	  Whole genome assembly
  Mauve 	  Mauve Genome Alignment Software
  MeDUSA 	  Methylated DNA Utility for Sequence Analysis - Computational pipeline to perform a full analysis of MeDIP-seq data
  MEGA 		  Molecular Evolutionary Genetics Analysis - Software suite for analyzing DNA and protein sequence data from species and populations
  MEME Suite 	  Motif-based sequence analysis tools
  MERLIN 	  Uses sparse trees to represent gene flow in pedigrees
  Microbiome 	  Microbiome Utilities 	  
  MIRA 	  	  Whole genome shotgun and EST sequence assembler
  MISO 	  	  Probabilistic analysis and design of RNA-Seq experiments for identifying isoform regulation
  MitoSeek 	  Extraction of mitochondrial genome information from exome sequencing data
  MODELLER 	  Program for Comparative Protein Structure Modelling by Satisfaction of Spatial Results
  mpiBLAST 	  Open-Source Parallel BLAST
  MrBayes 	  Bayesian Inference of Phylogeny
  Multiz/TBA 	  Threaded-Blockset Aligner, a local multiple sequence alignment tool; MULTIZ, aligns highly rearranged or incompletely sequenced genomes
  MUMmer 	  System for rapidly aligning entire genomes
  MUSCLE 	  Multiple sequence alignment
  MuTect 	  Reliable and accurate identification of somatic point mutations
  NGS-SNP 	  Collection of command-line scripts for providing rich annotations for SNPs
  Novoalign  	  Aligner for short nucleotide space reads
  NucleoATAC  	  Package for calling nucleosomes using ATAC-Seq data
  Oases 	  De novo transcriptome assembler for very short reads
  454-OISA  	  454 Sequencing System Off-Instrument Software Applications suite
  OL Basecaller   Performs base calling and bcl to qseq conversion for the HiSeq, HiScan-SQ, or Genome Analyzer
  ONCOCNV 	  Detection of copy number changes in Deep Sequencing data
  OncoSNP-SEQ 	  Characterise copy number alterations and loss-of-heterozygosity events
  Oncotator 	  Annotate human genomic point mutations and indels with data relevant to cancer researchers
  OpenMS 	  LC/MS data management and analyses
  PAGIT 	  Post Assembly Genome Improvement Toolkit
  PAML 	  	  Phylogenetic Analysis by Maximum Likelihood
  Panseq 	  Determine the core and accessory regions among a collection of genomic sequences
  PeakRanger 	  Multi-purporse software suite for analyzing next-generation sequencing (NGS) data
  PEAR 	  	  PEAR is an ultrafast, memory-efficient and highly accurate pair-end read merger
  PeSV-Fisher 	  Pipeline for the detection of five general types of structural variants
  phantompeakqual Compute quick, highly informative enrichment and quality measures for ChIP-seq/DNase-seq/FAIRE-seq/MNase-seq data
  phrap 	  phrap is a program for assembling shotgun DNA sequence data
  PHYLIP 	  A free package of programs for inferring phylogenies
  Pindel 	  Detection of breakpoints of structural variants at single-based resolution from next-gen sequence data
  plink 	  Whole genome association analysis toolkit
  Polymutt 	  Calls single nucleotide variants and detects de novo point mutation events in families for next-generation sequencing data
  PolyPhen-2 	  Predicts possible impact of amino acid substitutions on the structure and function of human proteins
  popoolation2 	  Allows comparision of allele frequencies between two ore more populations
  popoolation 	  Estimate natural variation and positive selection
  Preseq 	  Predict and estimate the complexity of a genomic sequencing library
  Primer3 	  PCR primer design tool
  PRINSEQ Lite 	  Filter, reformat, or trim genomic and metagenomic sequence data
  PROCHECK 	  Stereochemical protein structure quality analysis
  ProgCactus 	  A whole-genome alignment package
  PSIPRED 	  Accurate protein secondary structure prediction
  PyCogent 	  A toolkit for making sense from sequence
  PyNAST 	  Python Nearest Alignment Space Termination tool
  pyprophet 	  Analyse MRM data
  PyroBayes 	  A novel base caller for pyrosequences from the 454 Life Sciences sequencing machines
  Q 	  	  Whole genome association analysis toolkit
  Qiime 	  Quantitative Insights Into Microbial Ecology
  RAxML 	  Randomized Axelerated Maximum Likelihood 	  Sequential and parallel inference of large phylogenies with maximum likelihood
  Rcount 	  Simple and flexible RNA-Seq read counting
  RDP Classifier  Naive Bayesian classifier that can rapidly and accurately provides taxonomic assignments from domain to genus.
  RepeatNet 	  An ab initio centromeric sequence detection algorithm
  rMATS 	  Detect differential alternative splicing events from RNA-Seq data
  RMBlast 	  NCBI Blast modified for use with RepeatMasker/RepeatModeler
  RSEM 	  	  Estimate gene and isoform expression levels from RNA-Seq data
  RSeQC 	  An RNA-seq Quality Control Package
  RStudio Desktop A free and open source integrated development environment for R
  samblaster 	  Mark duplicates and extract discordant and split reads from sam files
  Satsuma 	  High-sensitivity alignments through cross-correlation
  screed 	  Short read sequence utils in Python.
  Scythe 	  A very simple adapter trimmer
  Scythe 	  A very simple adapter trimmer
  SeqAn 	  Open source C++ library of efficient algorithms and data structures for the analysis of sequences
  SeqClean 	  The Gene Indices Sequence Cleaning and Validation script (SeqClean)
  SeqEM 	  Adaptive genotype-calling approach for next-generation sequencing studies
  SeqGene 	  Software for mining next-gen sequencing datasets
  Seqtk 	  Toolkit for processing sequences in FASTA/Q formats
  SRAT	 	  Programmatically access data housed within SRA and convert it from the SRA format
  SSAHA		  Sequence Search and Alignment by Hashing Algorithm - A pairwise sequence alignment program for efficient mapping of sequencing reads
  SVA		  Sequence Variant Analyzer - Annotate, visualize and analyze the genetic variants indentifed through next-generation sequencing studies
  SOAP		  Short Oligonucleotide Analysis Package - An updated version of SOAP software for short oligonucleotide alignment
  SHRiMP 	  Software package for aligning genomic reads against a target genome
  SICER 	  Identify enriched domains from histone modification ChIP-Seq data
  Sickle 	  A windowed adaptive trimming tool for FASTQ files using quality
  Sickle 	  A windowed adaptive trimming tool for FASTQ files using quality
  Sierra Perl 	  Perl client to access Sierra, the Stanford HIV Web Service
  SiPhy 	  Rigorous statistical tests to detect bases under selection from a multiple alignment data
  SNAP 	  	  General purpose gene finding for both eukaryotic and prokaryotic genomes
  snpEff 	  Fast variant effect predictor (SNP, MNP and InDels) for genomic data
  SNPTEST v2 	  Analysis of single SNP association in genome-wide studies
  SNVMix 	  Detect single nucleotide variants from next generation sequencing data
  SOAPdenovoTrans A de novo transcriptome assembler designed specifically for RNA-Seq
  SOAPfusion 	  Fusion discovery with paired-end RNA-Seq reads
  SOAP-ICLU 	  Identify genome-wide large variants, such as CNVs and LOH etc.
  'SOAPIndel' 	  Call indels from next-generation paired-end sequencing data
  SOAPsnp 	  Calls consensus genotype by carefully considering data quality, alignment and recurring experimental errors
  'SOAPsplice' 	  Genome-wide ab initio detection of splice junction sites from RNA-Seq
  STIR		  Software for Tomographic Image Reconstruction - Multi-platform object-oriented framework for data manipulations in tomographic imaging
  SortMeRNA 	  SortMeRNA is a software designed to rapidly filter ribosomal RNA fragments from metatranscriptomic data produced by next-generation sequencers.
  Stacks 	  Software pipeline for building loci from short-read sequences
  Staden Package  A fully developed set of DNA sequence assembly (Gap4 and Gap5), editing and analysis tools (Spin)
  STAR 	  	  Aligns RNA-seq reads to a reference genome using uncompressed suffix arrays
  Strelka 	  Somatic variant calling workflow for matched tumor-normal samples
  StructHarvester Extracting data from STRUCTURE results files
  Structure 	  Use multi-locus genotype data to investigate population structure
  SuperHirn 	  Tool to quantitatively analyze multi-dimensional LC-MS data
  SVMerge 	  Enhanced structural variant and breakpoint detection
  Tabix++ 	  C++ wrapper to Tabix indexer for TAB-delimited genome position files
  Tabix 	  Generic indexer for TAB-delimited genome position files
  Tablet 	  Lightweight, high-performance graphical viewer for next generation sequence assemblies and alignements
  Tandem Repeats  Locate and display tandem repeats in DNA sequences
  tax2tree 	  Assists in decorating an existing taxonomy onto a phylogenetic tree with overlapping tip names
  T-Coffee 	  Align sequences or combine the output of other alignment methods into one unique alignment
  TMAP 	  	  Torrent mapping alignment program
  TopHat 	  A spliced read mapper for RNA-Seq
  Trans-ABySS 	  Analyze ABySS multi-k-assembled shotgun transcriptome data
  treeviewx 	  Phylogeny tree viewer
  Trim Galore! 	  Wrapper tool around Cutadapt and FastQC to consistently apply quality and adapter trimming to FastQ files
  Trimmomatic 	  A flexible read trimming tool for Illumina NGS data
  Trinity 	  A novel method for the efficient and robust de novo reconstruction of transcriptomes from RNA-seq data
  trRNAscan-SE 	  Improved detection of transfer RNA genes in genomic sequence
  UNPHASED 	  Software for genetic association analysis
  USeq 	  	  Collection of software tools for analysis of sequencing data from the Solexa, SOLiD, and 454 platforms
  VariationHunter A tool for discovery of structural variation in one or more individuals simultaneously using high throughput technologies
  vcflib utils 	  Command-line utilities for executing complex manipulations on VCF files
  Velvet 	  Sequence assembler for very short reads
  VerifyBamID 	  Verify whether reads match previously known genotypes for an individual
  Vienna RNA 	  RNA Secondary Structure Prediction and Comparison
  Vmatch 	  Software tool for efficiently solving large scale sequence matching tasks
  Wise2 	  Program for aligning proteins or protein HMMs to DNA
  WU BLAST 	  Washington University-produced alternative to NCBI BLAST
  wwwblast 	  A suite of standalone BLAST programs produced by NCBI for use on the web
  Bioconductor 	  Tools for the analysis and comprehension of high-throughput genomic data


Bio-physics
-----------

.. code:: bash

  vmd	  	  Molecular visualization program for displaying, animating, and analyzing large biomolecular systems
  NAMD 	  	  A parallel molecular dynamics code designed for high-performance simulation of large biomolecular systems


Chemistry
---------

.. code:: bash

  ASE		  Atomistic Simulation Environment - Python modules for manipulating atoms, analyzing simulations and visualization
  Desmond 	  High-speed molecular dynamics simulations of biological systems
  DL_POLY 	  General purpose classical molecular dynamics (MD) simulation
  ESPResSo 	  Extensible Simulation Package for Research on Soft matter
  GAMESS	  General Atomic and Molecular Electronic Structure System (GAMESS)  -Ab initio molecular quantum chemistry
  babel	  	  A chemical toolbox designed to speak the many languages of chemical data
  gpaw	  	  A density-functional theory (DFT) Python code
  gromacs	  Perform molecular dynamics; simulate the Newtonian equations of motion for systems with hundreds to millions of particles
  nwchem	  Methods for computing the properties of molecular and periodic systems
  OpenMD 	  Open source molecular dynamics engine
  Maestro 	  Schrödinger Maestro - a powerful, all-purpose molecular modelling envirotnment


Compilers
---------

.. code:: bash

  GNU GCC	  GNU Compiler Collection including front ends for C, C++, Objective-C and Fortran
  Cluster Studio  Intel Cluster Studio - High performance cluster tools to increase performance and scalability
  Open64	  An open source, optimizing compiler for the Itanium and x86-64 microprocessor architectures
  Oracle Java(TM) Java Programing Language

  
Databases
---------

.. code:: bash

  hdf5	  	  Data model, library, and file format for storing and managing data


Electronics
-----------

.. code:: bash

  Octopus 	  A scientific program aimed at the ab initio virtual experimentation


Engineering
-----------

.. code:: bash

  ANSYS Workbench Suite of advanced engineering simulation tools
  Code_Saturne 	  Solve the Navier-Stokes equations for 2D, 2D-axisymmetric and 3D flows


Geography
---------

.. code:: bash

  GRASS GIS 	  Free and open source Geographic Information System (GIS) software suite
  PROJ.4 	  Convert geographic longitude and latitude coordinates into cartesian coordinates


Graphics and Imaging
--------------------

.. code:: bash

  DIL		  Developer's Image Library - Cross-platform image library utilizing a simple syntax to load, save, convert, manipulate, filter and display a variety of images with ease
  POV-Ray 	  The Persistence of Vision Raytracer
  vtk 	  	  Package for 3D graphics, modeling and image processing
  Bsoft 	  Bernard's Software Package
  CTFFIND3	  CTF estimation
  CTFFIND4 	  CTF estimation
  Dynamo 	  Software environment for subtomogram averaging of cryo-EM data
  EMAN2 	  Broadly based greyscale scientific image processing suite
  FFmpeg 	  A complete, cross-platform solution to record, convert and stream audio and video
  IHRSR++ 	  Extension of Iterative Helical Real Space Reconstruction (IHRSR) software
  IMOD 	  	  Image processing, modeling and display programs for tomographic reconstruction
  RELION 	  REgularised LIkelihood OptimisatioN
  ResMap 	  Local Resolution Map Algorithm
  SPIDER 	  System for Processing Image Data from Electron microscopy and Related fields


Languages
---------

.. code:: bash

  Anaconda Py2.7  Completely free Python distribution including popular Python packages (python 2.7)
  Anaconda Py3 	  Completely free Python distribution including popular Python packages (python 3)
  Cython 	  C-Extensions for Python
  Glasgow 	  Haskell Compiler and interactive environment for the functional language Haskell
  julia	  	  High-level, high-performance dynamic programming language for technical computing
  perl	  	  A highly capable, feature-rich programming language with over 24 years of development
  php	  	  A widely-used general-purpose scripting language that is especially suited for Web development
  python	  A remarkably powerful dynamic programming language
  R	  	  Language and environment for statistical computing and graphics
  ruby	  	  A dynamic, open source programming language with a focus on simplicity and productivity
  IPython 	  Rich architecture for interactive computing
  Mono 	  	  A software platform designed to allow developers to easily create cross platform applications
  Oracle Java(TM) Java Programing Language
  R 	  	  Language and environment for statistical computing and graphics
  Scala 	  Multi-paradigm programming language built on top of the Java virtual machine.
  Virtualenv 	  Virtual Python Environment Builder
 

Libraries
---------

.. code:: bash

  GCC		  GNU C/C++ Compiler
  ANTLR		  ANother Tool for Language Recognition - Language tool that provides a framework for constructing interpreters, compilers, and translators
  BLACS		  Basic Linear Algebra Communication Subprograms - A linear algebra oriented message passing interface
  Caffe 	  A fast open framework for deep learning
  CUDA Toolkit 	  Development environment for C and C++ developers building GPU-accelerated applications
  CythonGSL 	  Cython interface for the GNU Scientific Library (GSL)
  GEOS 	  	  Geometry Engine, Open Source
  GDAL		  Geospatial Data Abstraction Library 	  Translator library for raster geospatial data formats
  GMP 	  	  Library for arbitrary precision arithmetic
  GSL		  GNU Scientific Library - A numerical library for C and C++ programmers
  Rnetcdf	  RNetCDF
  libdc1394	  C++ API to the PostgreSQL database management system.
  Math-atlas	  Automatically Tuned Linear Algebra Software - portably optimal linear algebra software
  mpi4py	  Python bindings for the Message Passing Interface (MPI)
  libgit2	  Portable, pure C implementation of the Git core methods
  boost	  	  Free peer-reviewed portable C++ source libraries
  fftw	   	  C subroutine library for computing the discrete Fourier transform (DFT) in one or more dimensions
  fltk	  	  A cross-platform C++ GUI toolkit providing modern GUI functionality without the bloat
  freeds	  A set of libraries for Unix and Linux that allow programs to natively talk to Microsoft SQL Server and Sybase databases
  freetype	  Freetype fonts package	
  graphicsmagick  Swiss army knife of image processing
  imagemagick	  An open source software suite for displaying, converting, and editing raster image files
  Blas-forum	  Reference implementation for the C interface to the Legacy BLAS
  blas	  	  Routines that provide standard building blocks for performing basic vector and matrix operations
  clapack	  LAPACK translated from Fortran to C
  lapack	  Linear Algebra PACKage - routines for equation solving systems
  scalapack	  A library of high-performance linear algebra routines for parallel distributed memory machines
  pil	  	  Adds image processing capabilities to your Python interpreter
  netcdf	  NetCDF
  zeroc	  	  A modern distributed computing platform.
  Img 	  	  Support for many image formats for Tk
  JasPer 	  Reference implementation of the JPEG-2000 Part-1 standard
  Lasagne 	  Lightweight library to build and train neural networks in Theano
  libctl 	  Flexible control files for scientific simulations
  libgdiplus 	  C-based implementation of the GDI+ API
  Libxc 	  Libxc is a library of exchange-correlation functionals for density-functional theory.
  LLVM Core 	  A modern source- and target-independent optimizer with code generation support
  matplotlib 	  2D plotting library for Python which produces publication quality figures
  MPFR 	  	  C library for multiple-precision floating-point computations with correct rounding
  NetCDF Fortran  Set of interfaces and libraries for array-oriented data access
  NetCDF 	  Set of interfaces and libraries for array-oriented data access
  numexpr 	  Fast numerical array expression evaluator for Python and NumPy
  OpenBLAS 	  An optimized BLAS library
  OpenCV 	  Open source computer vision and machine learning software library
  OpenLibm 	  High quality, portable, standalone C mathematical library
  pandas 	  Powerful data structures for data analysis, time series,and statistics
  PCRE2 	  Perl Compatible Regular Expressions
  Protocol Buff	  Protocol Buffers are a way of encoding structured data in an efficient yet extensible format. Google uses Protocol Buffers for almost all of its internal RPC protocols and file formats.
  pybedtools 	  Wrapper around BEDTools for bioinformatics work
  PyGTK Libraries GTK+ for Python
  PyQt4 	  Python v2 and v3 bindings for Digia's Qt application framework
  PyQwt 	  PyQwt plots data with Numerical Python and PyQt
  Pysam 	  Python module for reading and manipulating Samfiles
  PyTables 	  Hierarchical datasets
  Qt 	  	  A cross-platform application and UI framework
  QuTIP 	  Quantum Toolbox in Python
  Rmpi 	   	  Provides an interface (wrapper) to MPI APIs
  ROOT 	  	  Set of OO frameworks to handle and analyze large amounts of data efficiently
  RPy 	  	  A simple and efficient access to R from Python
  Seaborn 	  Seaborn is a Python visualization library based on matplotlib.
  SLICOT 	  Subroutine Library in Systems and Control Theory
  Snappy Java 	  Snappy compressor/decompressor for Java
  SparseHash 	  An extremely memory-efficient hash_map implementation
  SPIOL		  Staden Package I/O Libraries - I/O libraries developed as part of the Staden Project
  Theano 	  Define, optimize, and evaluate mathematical expressions involving multi-dimensional arrays efficiently
  TBB		  Threading Building Blocks - C++ template library that simplifies the development of software applications running in parallel
  Trilinos 	  Algorithms for the solution of multi-physics engineering and scientific problems
  UDUNITS 	  Programatic handling of units of physical quantities
  Ceres Solver 	  C++ library for modeling and solving large complicated nonlinear least squares problems
  GetPot 	  Tool to parse the command line and configuration files.
  gflags 	  Library that implements commandline flags processing
  glog 	  	  Application-level logging library
  nanoflann 	  C++ header-only fork of FLANN, a library for KD-trees
  pyBigWig 	  A python extension, written in C, for quick access to and creation of bigWig files.


Mathematics
-----------

.. code:: bash

  eigen	  	  C++ template library for linear algebra
  Arpack-ng	  Collection of Fortran77 subroutines designed to solve large scale eigenvalue problems
  numpy	  	  Fundamental package for scientific computing in Python
  suitesparse	  A suite of sparse matrix packages
  octave	  High-level interpreted language, primarily intended for numerical computations
  qhull	  	  General dimension code for computing convex hulls
  METIS 	  Serial Graph Partitioning and Fill-reducing Matrix Ordering
  MGRIDGEN 	  Obtain a sequence of successive coarse grids that are well-suited for geometric multigrid methods
  qrupdate 	  Fortran library for fast updates of QR and Cholesky decompositions
  SciPy 	  Scientific tools for Python
  SCOTCH 	  Graph and mesh/hypergraph partitioning, graph clustering, and sparse matrix ordering
  SMLT		  The Shogun Machine Learning Toolbox - A large scale machine learning toolbox


Medicine
--------

.. code:: bash

  FreeSurfer 	  A comprehensive library of analysis tools for FMRI, MRI and DTI brain imaging data
  FreeSurfer 	  An open source software suite for processing and analyzing (human) brain MRI images


MPIs
----

.. code:: bash

  mvapich2	  MPI-2 over OpenFabrics-IB, OpenFabrics-iWARP, PSM, uDAPL and TCP/IP
  mvapich 	  MPI-1 over OpenFabrics/Gen2, OprnFabrics/Gen2-UD, uDAPL, InfiniPath, VAPI and TCP/IP
  mpich2	  A high-performance and widely portable implementation of the MPI standard (both MPI-1 and MPI-2)
  MPICH 	  A high-performance and widely portable implementation of the MPI standard (MPI-1, MPI-2 and MPI-3)
  Open MPI 	  A High Performance Message Passing Library


Physics
-------

.. code:: bash

  CASTEP 	  A leading code for calculating the properties of materials from first principles
  fREEDA 	  Multi-physics simulator
  Harminv 	  Extract mode frequencies from time-series data
  openfoam	  A C++ toolbox for the development of customized numerical solvers, and pre-/post-processing utilities
  LAMMPS 	  Molecular Dynamics Simulator - LAMMPS ('Large-scale Atomic/Molecular Massively Parallel Simulator') is a molecular dynamics program from Sandia National Laboratories
  Meep 	  	  Finite-difference time-domain (FDTD) simulation software
  MPB		  MIT Photonic Bands - Electromagnetic eigenmode solver


Statistics
----------

.. code:: bash

  biogeme 	  Estimation of discrete choice models


Tools
-----

.. code:: bash

  Bazel 	  Correct, reproducible, fast builds for everyone
  bcrypt 	  Cross-platform file encryption utility
  CMake 	  An extensible, open-source system that manages the build process in an operating system and compiler-independent manner
  EC2 AMI Tools   Command-line tools to create and manage Amazon Machine Images
  EC2 API Tools   Command-line tools for managing EC2 instances
  Git 	  	  Git - the stupid content tracker
  GNU Parallel 	  Shell tool for executing jobs in parallel using one or more computers
  flex	  	  The fast lexical analyzer
  cpanminus	  Get, unpack, build and install modules from CPAN
  patchelf	  Utility to modify the dynamic linker and RPATH of ELF executables
  cmake	  	  An extensible, open-source system that manages the build process in an operating system and compiler-independent manner
  tau	  	  Portable profiling and tracing toolkit for performance analysis of parallel programs written in Fortran, C, C++, Java, Python
  OpenStackClient Command-line client for OpenStack
  pip 	  	  The PyPA recommended tool for installing and managing Python packages.
  setuptools 	  Download, build, install, upgrade and uninstall Python packages -- easily!
  SIP 	  	  Automatically generate Python bindings for C and C++ libraries
  h5utils 	  Utilities for visualization and conversion of scientific data in HDF5 format
  mbuffer	  Tool for buffering data streams


Visualization
-------------

.. code:: bash

  Circos 	  A software package for visualizing data and information.
  ants	      	  Advanced Normalization ToolS for brain and image mapping
  OctoMap 	  A probabilistic, flexible, and compact 3D mapping library for robotic systems
  ParaView 	  Data analysis and visualization

