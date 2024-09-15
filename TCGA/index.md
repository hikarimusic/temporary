# Cancer Genome Research Plan

M5董宇光

## Objective

Analyze the mRNA expression in cancer cells, in hope of further undersatnding the pathogenesis of cancer at molecular level.
* Looking for driver mutation, translocation or overexpression that are not yet identified
* Looking for heterogeneity in a single cancer type or homogeneity between different cancer types

## Materials

The most useful data should be mRNA sequencing (RNA-seq) data of cancer cells. It contains all the information for further analysis. Alternatively, the whole genome sequencing (WGS) or whole exome sequencing (WXS) data can serve for the same purpose.
* **TCGA**: In TCGA, most cases have a file named "xxxx.rna_seq.genomic.gdc_realn.bam", for example, https://portal.gdc.cancer.gov/files/98b26b34-056d-439b-b1b7-2d17d258bad6. Its access is controlled, so only authorized PI can download the data. It has the file extension of ".sam" or ".bam" and has the following format:
```
@SQ	SN:NC_000009	LN:10000000
@PG	ID:vta	PN:vta	VN:0.0.1	CL:./align sequence.fna input1.fq input2.fq vta.sam
SIM0000001	99	NC_000009	6616075	255	65M1I34M	=	6616520	545	TAGGGTGAAGGCCGCGTCCGTTTTTCAATAGTAACGTTGTGACTTGGGTTCTGAGCGTGCTAATCCCCACTATAGGTTACGAGCGTAACTGGGGTCTAGA	*	aln:M65I1M8S1M25
SIM0000001	147	NC_000009	6616520	255	100M	=	6616075	-545	TTGACGGAACACAATGACGGTCGTACGCCTCAGACAGTAGACACCTTCACTAGGTGTCAAACCGCGGTCTGCAGGCCCTGGGGCACTAGTAGCCACTTGA	*	aln:M95S1M4
```
* **Lab**: If some labs are performing NGS by themselves, then they will either get the unmapped data or mapped data. The mapped data is exactly the ".sam" or ".bam" file explained above. The unmapped data has file extension of ".fastq" or ".fq" and has the following format:
```
@SIM0000001 1
TAGGGTGAAGGCCGCGTCCGTTTTTCAATAGTAACGTTGTGACTTGGGTTCTGAGCGTGCTAATCCCCACTATAGGTTACGAGCGTAACTGGGGTCTAGA
+
*
@SIM0000002 1
TCTAGTTATAATGACAAATGTCGCTAGTCCTCGGCAGCACCACTAGTCGTTGACTCCTGGGTAACAGGAATACGCTCGCACTCGGATTTCCCCCGGCAAC
+
*
```

## Methods
Using the mRNA sequencing data, we can analyze the mutation, structural variation, mRNA expression profiling, etc., of cancer cells.
* **NGS mapping**: NGS data contains ~100 millions of short sequences each with ~100 base pairs. We need to map those sequences to a reference genome to know their positions. The ".sam" or ".bam" data on TCGA is already mapped; alternatively, we can mapped these data using our own algorithm if the data is unmapped, or to gain more accuracy and allow more customization. I have developed a NGS mapping algorithm, which uses count sort, suffix array, Burrows-Wheeler transform, FM index for the indexing part, and uses seeding, clustering, DP for the alignment part: https://github.com/hikarimusic/VTA
* **Analysis**: After mapping the NGS data, we can identify which gene do these short sequences come from, and many analysis can be performed. First, we can examine whether there are single nucleotide variation (substitution, insertion, deletion) in these mRNA. Second, we can examine whether ther are structural variation (translocation, inversion, alternative splicing) in these mRNA. Lastly, we can examine whether there are overexpression or underexpression of some gene. Combing these results, chances are that we can identify new driver mutations or pathway, sub-classify a single cancer type, or unite different cancer types based on gene expression.
