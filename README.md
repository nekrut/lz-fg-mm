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
  
