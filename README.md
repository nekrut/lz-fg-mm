# `lastz` versus `minimap2` vertsus `fastga`
Which tool is most sensitive towards divergent sequences? To answer this question we generated random sequence pairs with 1,000, 2,000, 5,000 and 10,000 nucleotides (nt) in lengths and divergences between 0% and 40% in 1% increments. Short indels (ranging from 1 to 5 nucleotides) were introduced at a rate of 1%. Random 1,000 nucleotide flanks were added to each sequence. Scripts used for generation of these sequences can be found at https://github.com/rsharris/echydna. The sequences were aligned using the three aligners using versions and parameters described below:

| Tool | Source | Version | Params |
|-----|-------|------|------|
| lastz | [![Anaconda-Server Badge](https://anaconda.org/bioconda/lastz/badges/version.svg)](https://anaconda.org/bioconda/lastz) | 1.04.22 | defaults |
| minimap2 | [![Anaconda-Server Badge](https://anaconda.org/bioconda/minimap2/badges/version.svg)](https://anaconda.org/bioconda/minimap2) | 2.28 | `-x asm20` |
| fastga | https://github.com/thegenemyers/FASTGA | [170a178](https://github.com/thegenemyers/FASTGA/tree/170a178d16720b57cf33125ba3c904090bde4121) | `-i.6` |


