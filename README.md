# A global virome of methanogenic archaea highlights novel diversity and adaptations to the gut environment
## Example commands for employed programs

### Identification of viral sequences
1) Prediction of genes with prodigal v2.6.3:

```prodigal -p meta -i methanogens_database.fna -a methanogens_database.faa```

2) Prediction of MCP, terminase and portal genes with hmmer/3.3.2:

```hmmsearch -E 0.00001 --domtblout methanogens.MCP.hmmsearch.txt MCP.hmm methanogens_database.faa```

3) Check completeness of contig with CheckV/1.0.1:

``` checkv end_to_end viral_contigs.fna output_dir -t 16```

4) Prediction of MGE-specific genes with hmmscan using phrogs database (ver3):

```hmmscan --tblout methanogen_viruses.phrogs.tab -E 0.0000000001 --cpu 10 -o methanogen_viruses.phrogs.txt --noali phrogs_database.hmm viral_contigs.faa```

### Host prediction
1) Constructing CRISPRdb using CRISPRCasFinder/4.2.20:

```CRISPRCasFinder -in methanogens_database.fna -so sel392.so -maxSP 200 -cSA -q```


