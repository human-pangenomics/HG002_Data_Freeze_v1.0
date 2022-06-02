# HG002 assembly pipelines and best practices
In this page we collect best practices for genome assembly based on our bakeoff study and production.

## HG002 bakeoff

Based on an extensive comparison between different assembly strategies [Jarvis, Formenti et al.](https://www.biorxiv.org/content/10.1101/2022.03.06.483034v1.full), we developed a pipeline that combines the best practices of all approaches and used it to generate a higher-quality diploid de novo assembly (see schematics below).

The steps include:
- Removal of reads containing adaptors using [HiFiAdapterFilt](https://github.com/sheinasim/HiFiAdapterFilt);
- Generation of HiFi maternal and paternal contigs with the graph-based haplotype phasing of [Trio Hifiasm](https://github.com/chhylp123/hifiasm);
- Independent scaffolding of the maternal and paternal HiFi-based contigs with maternal and paternal Bionano optical maps;
- Manual evaluation of conflicts between the HiFi contigs and Bionano optical maps;
- Further scaffolding the paternal and maternal assemblies with haplotype-filtered ([Meryl](https://github.com/marbl/meryl)) Hi-C (OmniC) data and [Salsa](https://github.com/marbl/SALSA);
- [Manual curation](https://doi.org/10.1093/gigascience/giaa153) using Hi-C contact maps;
- Gap filling with [a custom pipeline](https://github.com/gf777/misc/tree/master/HPRC%20HG002/for_filling). Polishing of draft ONT-UL contigs assembled with [Flye](https://github.com/fenderglass/Flye) to increase consensus accuracy. Variant calls were generated using Medaka on ONT long reads, and filtered with [Merfin](https://github.com/arangrhie/merfin) using kmers from Illumina short reads
- A final round [Manual curation](https://doi.org/10.1093/gigascience/giaa153)
- Removal of bacterial, viral and mitochondrial contaminants