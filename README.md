# A global virome of methanogenic archaea highlights novel diversity and adaptations to the gut environment

## Pipeline overview

## Example commands

### Identification of viral sequences

1) Assembly of viral sequences from metagenomes using spades/3.5.15

``` spades.py --metaviral --only-assembler -o SRR11241201_res -1 SRR11241201_1.fastq.gz -2 SRR11241201_2.fastq.gz -m 600 -t 50 ```

3) Prediction of genes with prodigal v2.6.3:

```prodigal -p meta -i methanogens_database.fna -a methanogens_database.faa```

3) Prediction of MCP, terminase and portal genes with hmmer/3.3.2:

```hmmsearch -E 1e-10 --domtblout methanogens.MCP.hmmsearch.txt MCP.hmm methanogens_database.faa```

4) Check completeness of contig with CheckV/1.0.1:

``` checkv end_to_end viral_contigs.fna output_dir -t 16```

5) Prediction of MGE-specific genes with hmmscan using phrogs database (ver3):

```hmmscan --tblout methanogen_viruses.phrogs.tab -E 1e-10 --cpu 10 -o methanogen_viruses.phrogs.txt --noali phrogs_database.hmm viral_contigs.faa```

6) Prediction of viruses with Virsorter2/2.2.3:

```virsorter config --set HMMSEARCH_THREADS=30```

```virsorter run -w result.out --keep-original-seq -i methanogens_database.fna --min-length 1000 --min-score 0.5 -j 30 all```

### Host prediction
1) Constructing CRISPRdb using CRISPRCasFinder/4.2.20:

```CRISPRCasFinder -in methanogens_database.fna -so sel392.so -maxSP 200 -cSA -q```

2) Matching CRISPR spacers to predicted viral sequences using blast+/2.13.0:

```blastn -query spacers_methanogens.fa -db viral_contigs.fna -word_size 7  -num_threads 4  -evalue 0.001 -outfmt "6 qseqid sseqid slen sstart send evalue qseq sseq qlen qstart qend bitscore score sstrand nident positive staxids sscinames stitle"  > blastn_spacer_hits```

   Filetring hits with > 90% identity:

```cat blastn_spacer_hits | awk '{if ($16/$9 > 0.8999) print}' > blastn_spacer_hits_09.txt```

3) Prediction of related viruses with vcontact2/0.9.19:

```vcontact2 -r viral_contigs.faa -p viral_contigs.tsv -e csv cytoscape -o result_vcontact -db ProkaryoticViralRefSeq85-ICTV```

### Phylogenetic tree of head-tailed viruses

