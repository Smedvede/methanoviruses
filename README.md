# A global virome of methanogenic archaea highlights novel diversity and adaptations to the gut environment
## Example commands

### Identification of viral sequences
1) Prediction of genes with prodigal v2.6.3:

```prodigal -p meta -i methanogens_database.fna -a methanogens_database.faa```

2) Prediction of MCP, terminase and portal genes with hmmer/3.3.2:

```hmmsearch -E 1e-10 --domtblout methanogens.MCP.hmmsearch.txt MCP.hmm methanogens_database.faa```

3) Check completeness of contig with CheckV/1.0.1:

``` checkv end_to_end viral_contigs.fna output_dir -t 16```

4) Prediction of MGE-specific genes with hmmscan using phrogs database (ver3):

```hmmscan --tblout methanogen_viruses.phrogs.tab -E 1e-10 --cpu 10 -o methanogen_viruses.phrogs.txt --noali phrogs_database.hmm viral_contigs.faa```

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

