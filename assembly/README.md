# HG002 assembly pipelines and best practices
In this page we collect best practices for genome assembly based on our bakeoff study and production.

## HG002 bakeoff

Based on an extensive comparison between different assembly strategies [Jarvis, Formenti et al.] (https://www.biorxiv.org/content/10.1101/2022.03.06.483034v1.full), we developed a pipeline that combines the best practices of all approaches and used it to generate a higher-quality diploid de novo assembly (see schematics below).

The steps include:
- Removal of reads containing adaptors using [HiFiAdapterFilt] (https://github.com/sheinasim/HiFiAdapterFilt)
- Generation of HiFi maternal and paternal contigs with the graph-based haplotype phasing of [Trio Hifiasm v0.14.1] (https://github.com/chhylp123/hifiasm)
- Independent scaffolding of the maternal and paternal HiFi-based contigs with maternal and paternal Bionano optical maps
- Manual evaluation of conflicts between the HiFi contigs and Bionano optical maps 
- Further scaffolding the paternal and maternal assemblies with haplotype-filtered ([Meryl] (https://github.com/marbl/meryl)) Hi-C (OmniC) data and Salsa 2.3
- Manual curation using Hi-C contact maps
- Gap filling with a conservative version of the pipeline used to fill the gaps in the initial draft of the T2T CHM13 assembly. ONT-UL reads were base recalled with Guppy 4.2.2, haplotype binned using trio-Canu, and then assembled into haplotype specific contigs using Flye. Draft ONT-UL contigs were polished to increase consensus accuracy. Variant calls were generated using Medaka on ONT long reads, and filtered with Merfin42 using kmers from Illumina short reads and then applied to increase the quality of the consensus sequence. The polished contigs were aligned to their respective haplotypes of the curated HiFi-based scaffolds from the Hi-C step above, and used to fill gaps.
- A final round manual curation 
- Removal of bacterial, viral and mitochondrial contaminants