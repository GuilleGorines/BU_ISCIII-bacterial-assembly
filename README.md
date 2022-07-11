# ![BU-ISCIII Bacterial Assembly](docs/images/nf-core-assemblybacterias_logo.png)

**Nextflow pipeline for assembling bacterias**.

## Introduction

<!-- TODO nf-core: Write a 1-2 sentence summary of what data the pipeline is for and what it does -->
**BU-ISCIII Bacterial Assembly** is a bioinformatics best-practise analysis pipeline for

The pipeline is built using [Nextflow](https://www.nextflow.io), a workflow tool to run tasks across multiple compute infrastructures in a very portable manner. It comes with docker containers making installation trivial and results highly reproducible.

## Quick Start

1. Install [`nextflow`](https://nf-co.re/usage/installation) (`>=20.04.0`)

2. Install any of [`Docker`](https://docs.docker.com/engine/installation/), [`Singularity`](https://www.sylabs.io/guides/3.0/user-guide/), [`Podman`](https://podman.io/), [`Shifter`](https://nersc.gitlab.io/development/shifter/how-to-use/) or [`Charliecloud`](https://hpc.github.io/charliecloud/) for full pipeline reproducibility _(please only use [`Conda`](https://conda.io/miniconda.html) as a last resort; see [docs](https://nf-co.re/usage/configuration#basic-configuration-profiles))_

3. Download the pipeline and test it on a minimal dataset with a single command:

    ```bash
    nextflow run BU_ISCIII-bacterial-assembly/main.nf -profile test,<docker/singularity/podman/shifter/charliecloud/conda/institute>
    ```

    > Please check [nf-core/configs](https://github.com/nf-core/configs#documentation) to see if a custom config file to run nf-core pipelines already exists for your Institute. If so, you can simply use `-profile <institute>` in your command. This will enable either `docker` or `singularity` and set the appropriate execution settings for your local compute environment.

4. Start running your own analysis!

    ```bash
    nextflow run BU-ISCIII Bacterial Assembly -profile <docker/singularity/podman/shifter/charliecloud/conda/institute> --input '*_R{1,2}.fastq.gz'
    ```

## Pipeline Summary

By default, the pipeline currently performs the following steps:

* Sequencing quality control (`FastQC, version 0.11.9`)
* Sequence trimming (`FastP, version 0.23.2`)
* Identification of the bacterial organism (`KmerFinder, version 3.0`)
* Download of most abundant kmerfinder reference from the NCBI (`ad-hoc Python 3 scripts`)
* Assembly of reads (`UniCycler, version 0.4.8`)
* Assesment of assembly using the downloaded reference (`Quast, version 5.0.2`)
* Annotation of the assembly (`Prokka, version 1.14.6`)
* Mapping of the reads against the reference (`Minimap 2, version 2.24`)
* Generating the bam from the alignment of the reference (`Samtools, version 1.14`)

* Overall pipeline run summaries (`MultiQC, version 1.11`)

## Pipeline work, in depth

The detailed steps of the pipeline are the following:

For each sample in the sample sheet, a quality assessment using `FastQC (v 0.11.9)` with standard parameters is performed. Right after, the reads of each sample are submitted to a trimming process using `FastP (v 0.23.2)` with the following parameters by default: `--cut_front, --cut_tail, --cut_mean_quality, --qualified_quality_phred, --unqualified_percent_limit, --length_required, --trim_poly_x`. These parameters can be changed by using the available flags.

Once the reads have been trimmed, `kmerfinder (v 3.0)` identifies through a k-mer approach all the bacterias present in the reads, using its standard parameters. The genome of the most abundant bacteria strain in all the samples is downloaded using this results, along with its gff (genomic features) file. Besides, a csv (comma separated values) file containing the top 2 hits in kmerfinder is generated, in order to trace whether or not a particular sample can be contaminated.

The reads are assembled using `Unicycler (v 0.4.8)` with standard parameters. Once this process has reached its end, the resulting assembly is evaluated and compared to the downloaded genome and gff using `Quast (v 5.0.2)`. In addition, the assemblies are mapped against the downloaded reference with `Minimap 2 (v 2.24)` with the default params, and the resulting *.sam* file is converted to a sorted *.bam* file using `Samtools (v 1.14)`. The assembly is later annotated using `Prokka (v 1.14.6)`.

For the last step, all logs generated during the pipeline (FastQC, FastP, Quast) are collapsed into a `Multiqc (v 1.11)` html file to allow their easy checking.

## Credits
BU-ISCIII Bacterial Assembly was originally written by Guillermo Gorines. 


## Disclaimer
This pipeline, despite not being mantained by the nf-core community, follows its good-practise protocols, and has been created under usage of their nf-core tools.

## Citations

<!-- TODO nf-core: Add citation for pipeline after first release. Uncomment lines below and update Zenodo doi. -->
<!-- If you use  BU-ISCIII Bacterial Assembly for your analysis, please cite it using the following doi: [10.5281/zenodo.XXXXXX](https://doi.org/10.5281/zenodo.XXXXXX) -->

You can cite the `nf-core` publication as follows:

> **The nf-core framework for community-curated bioinformatics pipelines.**
>
> Philip Ewels, Alexander Peltzer, Sven Fillinger, Harshil Patel, Johannes Alneberg, Andreas Wilm, Maxime Ulysse Garcia, Paolo Di Tommaso & Sven Nahnsen.
>
> _Nat Biotechnol._ 2020 Feb 13. doi: [10.1038/s41587-020-0439-x](https://dx.doi.org/10.1038/s41587-020-0439-x).

In addition, references of tools and data used in this pipeline are as follows:

<!-- TODO nf-core: Add bibliography of tools and data used in your pipeline -->
