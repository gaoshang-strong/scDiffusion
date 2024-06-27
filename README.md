# scDiffusion: single cell RNA-seq data generative model 

![alt text](https://github.com/gaoshang-strong/scDiffusion/blob/main/scDiffusion.jpg)

Score-based Generative Models (SBGM) https://arxiv.org/abs/2011.13456 are a type of advanced diffusion model https://arxiv.org/abs/2006.11239. Essentially, they use vast amounts of training data to learn the score function of the data distribution, which is the gradient of the log probability density. SBGM excels at generating high-quality samples, particularly in image generation tasks. For example, the stable diffusion model https://arxiv.org/abs/2112.10752, trained on hundreds of millions of images, can generate extremely high-quality images with rich details and high realism. Besides generating new samples from scratch, SBGM is widely used for controlled sample transformations such as image restoration, style transfer, and image enhancement. In the biomedical field, SBGM has been applied to drug molecule generation and optimization, high-throughput compound screening, etc., providing a powerful new tool for drug development. However, current applications of generative models focus on the level of proteins and compounds https://www.nature.com/articles/s41467-024-45051-2 https://www.nature.com/articles/s41586-023-06415-8, with less mature products and deep exploration in upstream areas such as transcriptomics and epigenomics, which regulate protein expression. These expression regulation-related data are crucial for exploring pathological causes and guiding clinical practice. Therefore, we plan to apply score-based generative models to transcriptomics and epigenomics to generate samples and simulate changes in transcriptomics and epigenomics after interventions.

Single-cell RNA sequencing (scRNA-seq) is a high-throughput sequencing technology used to analyze gene expression profiles at the single-cell level. Compared to traditional transcriptomics methods based on bulk cells, scRNA-seq can reveal the heterogeneity of different cell subtypes, states, and functions, offering significant advantages in covering a large variety of cell types. The data from single-cell RNA sequencing often provide high-resolution gene expression analysis of thousands to millions of individual cells from complex tissues or organs, thus offering ample sample support for score-based generative models.

In this project, we plan to first construct a single-cell gene expression generative model (scDiffusion). scDiffusion consists of two independent sub-modules: the first module is the dimension reduction module, based on a variational auto-encoder (VAE). For each sample in the single-cell transcriptome data, the encoder in this module compresses it, reducing the original large vector containing expression levels of over 20,000 genes to a new vector of length 256. This step extracts key information, reduces noise, and significantly optimizes subsequent computational costs. In the dimension reduction module, we will also train a decoder that can restore the compressed 256-length vector back to the original data, ensuring no useful information is lost during compression. The second module of scGM is the generative module, which uses a score-based generative model to learn the distribution's score function from the dimension-reduced single-cell transcriptome data, thereby generating new single-cell transcriptome data. The generative module's output is a dimension-reduced 256-length vector, which can be further restored to a realistic single-cell transcriptome data using the dimension reduction module's decoder.

To establish the single-cell gene expression generative model, we will use various open-source single-cell transcriptome databases, including UCSC Cell Browser, Human Cell Atlas, and Tumor Immune Single-cell Hub 2, as training sets. These databases contain single-cell transcriptome data from various cell subtypes in healthy human organs and tissues, as well as related data from diseased tissues, such as those affected by SARS-COVID-2 and various cancers. We have uniformly preprocessed these data, including filtering, normalization, and log transformation, to form a large training set (scGDB) containing over 30 million cells. These cells will be used for scGM training. The goal of scGM is to generate new single-cell transcriptome data from scratch, with the ability to select cell types as needed. This will be applied to optimizing and enhancing single-cell transcriptome data. scGM will also need to accept new data and perform fine-tuning.

Here, we use Human Cell Atlas as training set to build scDiffusion as a validation before using full dataset. Human Cell Atlas has 599926 cells from different healthy human organs and tissue. The notebook scAE.ipynb shows the training of scAE which is a dimension reduction model. The notebook scGeneration.ipynb shows the score-based generative model trained with Human Cell Atlas. The notebook decode.ipynb shows the decoder results from samples generated by score-based generative model. The figure below shows the generation results: 

![alt text](https://github.com/gaoshang-strong/scDiffusion/blob/main/real_vs_simulated_data.png)

![alt text](https://github.com/gaoshang-strong/scDiffusion/blob/main/real_vs_simulated_data_separated.png)

The plot on the the left (orange) is the umap of real single cell data randome selected from Human Cell Atlas. The plot on the right (blue) is the umap of generated single cell data from scDiffusion.

Secondly, we will use scDiffusion to construct a single-cell gene expression editor (scGEditor). Inspired by the SDEdit model https://arxiv.org/abs/2108.01073, this model modifies existing single-cell transcriptome data to simulate a specific type of experimental intervention. The main idea is to adjust the sampling process of scGM, intervening in the output at each sampling step, thereby generating specific types of single-cell transcriptome data.
