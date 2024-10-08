# `lastz` versus `minimap2` versus `fastga`

## On simulated data

Which tool is most sensitive when used on divergent sequences? To answer this question we generated random sequence pairs with 1,000, 2,000, 5,000 and 10,000 nucleotides (nt) in lengths and divergences between 0% and 40% in 1% increments. Short indels (ranging from 1 to 5 nucleotides) were introduced at a rate of 1%. Random 1,000 nucleotide flanks were added to each sequence. Scripts used for generation of these sequences can be found at https://github.com/rsharris/echydna. 

### Aligners and settings

The sequences were aligned using the three aligners:

| Tool | Source | Version | Params |
|-----|-------|------|------|
| `lastz` | [![Anaconda-Server Badge](https://anaconda.org/bioconda/lastz/badges/version.svg)](https://anaconda.org/bioconda/lastz) | 1.04.22 | defaults |
| `minimap2` | [![Anaconda-Server Badge](https://anaconda.org/bioconda/minimap2/badges/version.svg)](https://anaconda.org/bioconda/minimap2) | 2.28 | `-x asm20` |
| `fastga` | https://github.com/thegenemyers/FASTGA | [170a178](https://github.com/thegenemyers/FASTGA/tree/170a178d16720b57cf33125ba3c904090bde4121) | `-i.6` |

`lastz` and `minimap2` alignments between simulated sequences were generated using Galaxy and can be found in this history: https://usegalaxy.org/u/cartman/h/simulated-runs. `fastga` alignments were computed using a local machine. Results are found in this repo:

- `./sequences/` = simulated data. Produced using `./ipynb/generate_apples_and_oranges.ipynb`
- `./ipynb/` = notebooks
- `./alignments/` = results produced by the three aligners
- `./scratch` = well ... scratch

### Results

![image](https://github.com/user-attachments/assets/45bb4fdf-94f0-4437-8e34-32d19cdc9f41)

- X-axis = divergence bins from 60% to 100% identity
- Y-axis = fraction of the simulated sequence covered by alignments produced with that aligner
- Rows correspond to sequence lengths: 1kb, 2kb, 5kb, and 10kb

## On real genomes

For this analysis we compared hg38 build of human genome against mm39. hg38 was used because it still has the highest annotation density.

### Data, aligners, and settings

Genome files were downloaded from UCSC:

- hg38 = https://hgdownload.soe.ucsc.edu/goldenPath/hg38/bigZips/hg38.fa.gz
- mm39 = https://hgdownload.soe.ucsc.edu/goldenPath/mm39/bigZips/mm39.fa.gz

We used the same version of `minimap2` as specified above. Instead of `lastz` we used `kegalign` release [v0.1.2.7](https://github.com/galaxyproject/KegAlign/releases/tag/v0.1.2.7). For `fastga` we used repo version `74d5666`. The following command line parameters were used:

| Tool | cmd |
|------|-----|
| `kegalign` | lastz defaults |
| `minimap2` | `-x asm20  --q-occ-frac 0.01 -Q --cs=long` |
| `fastga` | `-i.6` |

Resulting datasets were uloaded and analyzed in the following Galaxy history: https://usegalaxy.org/u/cartman/h/aligners--exons

### Post-processing

Postprocessing was performed using https://usegalaxy.org/u/cartman/w/cdsoverlaps workflow:

![image](https://github.com/user-attachments/assets/a58d7d67-6784-42bf-99a9-f8babc1d8c8d)

Briefly, the workflow performs the following steps:

1. Re-formats files to generate a 3-column bed file containing only coordinates of alignment blocks
2. The intervals in these files are merged using `bedtools merge` to remove redundancy. The final result is the list of non-overlapping unique intervals for each aligner.  
3. A bed file containing coordinates of all protein-coding exons for hg38 (5'- and 3'-UTRs are stripped) is fetched from the UCSC Table Browser and also merged using `bedtools merge`.
4. Coordinates of alignments from each tools are then projected to coordinates of exons using `bedtools annotate`.
5. These data were then aggregated by chromosome by counting the number of exons that overlap alignment blocks for each aligner. The total number on each chromosome was divided by the number of exons covered by alignments obtaining the final fraction for each tool. 
6. This data is processed using a [Jupyter Notebook](https://github.com/nekrut/lz-fg-mm/blob/main/ipynb/cds_exon_coverage.ipynb) to generate the image below.

### Results

![image](https://github.com/user-attachments/assets/7e2ee669-f132-410b-9d84-583ee9a7e819)

Fraction of human protein-coding exons (hg38) overlapping with alignments to mouse genome (mm39) produced by each of the three mappers. Coordinates of exons and alignment blocks were merged to avoid redundancy. 

  
