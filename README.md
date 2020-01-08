## Exonic Part PSI
#### Reference: Alternative Splicing Signatures in RNA-seq Data: Percent Spliced in (PSI), Curr. Protoc. Hum. Genet. 87:11.16.1-11.16.14. doi: 10.1002/0471142905.hg1116s87
#### PSI.sh derived from https://github.com/lehner-lab/Scaling_Law/blob/master/001_Exon_inclusion_levels_in_different_animals/PSI.sh, http://www.currentprotocols.com/protocol/hg1116, adding <path_to_bedtools2.23> for specify bedtools2.23

#### Dependency
- python2
- HTSeq, python2
- bedtools2.23

## Example, RNAseq sample from human
#### Make exonic part gff file
```bash
python2 path/to/dexseq_prepare_annotation.py path/to/Homo_sapiens.GRCh38.93.gtf Homo_sapiens.GRCh38.93_reduce.gtf

awk '{OFS="\t"}{if ($3 == "exonic_part") print $1,$2,$3,$4,$5,$6,$7,$8,$14":"$12}' Homo_sapiens.GRCh38.93_reduce.gtf | sed 's=[";]==g' > Homo_sapiens.GRCh38.93_Exonic_part.gff
```

#### Make SJ bed file from all samples
```bash
cat path/to/all/sample/STAR/out/*SJ.out.tab | awk '{print $1,$2,$3,$4,$5,$6,$7}' | sort -u | awk 'BEGIN{OFS="\t"}{print $1, $2-20-1, $3+20, "JUNCBJ"NR, $7, ($4 == 1)? "+":"-",$2-20-1, $3+20, "255,0,0", 2, "20,20", "0,300" }'  > junctions.bed
```

#### Calculate PSI
```bash
bash path/to/PSI.sh StartPSI path/to/Homo_sapiens.GRCh38.93_Exonic_part.gff <reads_length> example_1.bam junctions.bed example_1
```

