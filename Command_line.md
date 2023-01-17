# Regions identification figure 3a and extended data table 4

## Match unique mappers on fragmented 1kb dm6

```
bowtie-build -f /home/galaxy/galaxy/database/datasets/000/354/dataset_354387.dat genome 1>/dev/null && ln -s -f '/home/galaxy/galaxy/database/datasets/000/354/dataset_354387.dat' genome.fa &&     bowtie -p ${GALAXY_SLOTS:-4}  -v 0 -m 1   -S genome -f '/home/galaxy/galaxy/database/datasets/000/331/dataset_331322.dat'| samtools view -u - | samtools sort -@ "${GALAXY_SLOTS:-4}" -T tmp -O bam -o /home/galaxy/galaxy/database/datasets/000/354/dataset_354388.dat 2>&1
```

## Count both sense and antisense reads 

```
mkdir outputdir && sambamba view -t $GALAXY_SLOTS -F "not unmapped" -f bam '/home/galaxy/galaxy/database/datasets/000/354/dataset_354390.dat' -o 'Control3 on fragmented' && samtools index 'Control3 on fragmented' && python /home/galaxy/shed_tools/toolshed.g2.bx.psu.edu/repos/artbio/bamparse/8ea06787c08a/bamparse/bamparse.py --alignments 'Control3 on fragmented' --labels 'Control3 on fragmented' --number 'multiple'
```

## DEseq2 analysis

```
cat '/home/galaxy/shed_tools/toolshed.g2.bx.psu.edu/repos/iuc/deseq2/d027d1f4984e/deseq2/get_deseq_dataset.R' > /dev/null &&  Rscript '/home/galaxy/shed_tools/toolshed.g2.bx.psu.edu/repos/iuc/deseq2/d027d1f4984e/deseq2/deseq2.R' --cores ${GALAXY_SLOTS:-1} -o '/home/galaxy/galaxy/database/datasets/000/354/dataset_354403.dat' -p '/home/galaxy/galaxy/database/datasets/000/354/dataset_354404.dat' -A 0.1                  -H  -f '[["FactorName", [{"Control": ["/home/galaxy/galaxy/database/datasets/000/354/dataset_354397.dat", "/home/galaxy/galaxy/database/datasets/000/354/dataset_354398.dat", "/home/galaxy/galaxy/database/datasets/000/354/dataset_354399.dat"]}, {"Mutant": ["/home/galaxy/galaxy/database/datasets/000/354/dataset_354400.dat", "/home/galaxy/galaxy/database/datasets/000/354/dataset_354401.dat", "/home/galaxy/galaxy/database/datasets/000/354/dataset_354402.dat"]}]]]' -l '{"dataset_354400.dat": "Counts mutant1 fragmented (table0)", "dataset_354401.dat": "Counts mutant2 fragmented", "dataset_354402.dat": "Counts mutant3 fragmented (table0)", "dataset_354397.dat": "Counts control1 fragmented (table0)", "dataset_354398.dat": "Counts control2 fragmented (table0)", "dataset_354399.dat": "Counts control3 fragmented (table0)"}' -t 1
```

## Selection of interesting 1kb tiles

```
Filtering with c6<= 0.001
```
```
Filtering with c3<=-3
```
```
Filtering with c3>=3
```

## Clusterisation

```
python '/home/galaxy/shed_tools/toolshed.g2.bx.psu.edu/repos/devteam/cluster/05696474ee89/cluster/gops_cluster.py' '/home/galaxy/galaxy/database/datasets/000/354/dataset_354410.dat' '/home/galaxy/galaxy/database/datasets/000/354/dataset_354412.dat' -1 1,2,3,0 -d 3000 -m 2 -o 1
![image](https://user-images.githubusercontent.com/122443584/212908244-b22c9300-a3de-48fe-8e0e-767fc6f11c91.png)
```

# Small RNA profile figure 3b

sambamba view -t ${GALAXY_SLOTS} -F "not unmapped and sequence_length >= 18 and sequence_length <= 29" -f bam '/home/galaxy/galaxy/database/datasets/000/354/dataset_354942.dat' -o 'Control1' && samtools index 'Control1' && sambamba view -t ${GALAXY_SLOTS} -F "not unmapped and sequence_length >= 18 and sequence_length <= 29" -f bam '/home/galaxy/galaxy/database/datasets/000/354/dataset_354943.dat' -o 'Control3' && samtools index 'Control3' && sambamba view -t ${GALAXY_SLOTS} -F "not unmapped and sequence_length >= 18 and sequence_length <= 29" -f bam '/home/galaxy/galaxy/database/datasets/000/354/dataset_354944.dat' -o 'Control3' && samtools index 'Control3' && sambamba view -t ${GALAXY_SLOTS} -F "not unmapped and sequence_length >= 18 and sequence_length <= 29" -f bam '/home/galaxy/galaxy/database/datasets/000/354/dataset_354945.dat' -o 'Mutant1' && samtools index 'Mutant1' && sambamba view -t ${GALAXY_SLOTS} -F "not unmapped and sequence_length >= 18 and sequence_length <= 29" -f bam '/home/galaxy/galaxy/database/datasets/000/354/dataset_354946.dat' -o 'Mutant3' && samtools index 'Mutant3' && sambamba view -t ${GALAXY_SLOTS} -F "not unmapped and sequence_length >= 18 and sequence_length <= 29" -f bam '/home/galaxy/galaxy/database/datasets/000/354/dataset_354947.dat' -o 'Mutant2' && samtools index 'Mutant2' && python '/home/galaxy/shed_tools/toolshed.g2.bx.psu.edu/repos/artbio/small_rna_maps/f2e7ad3058e8/small_rna_maps'/small_rna_maps.py --inputs "Control1" "Control3" "Control3" "Mutant1" "Mutant3" "Mutant2"       --sample_names "Control1" "Control3" "Control3" "Mutant1" "Mutant3" "Mutant2" --minsize 18 --maxsize 29 --plot_methods 'Counts' 'Size' --outputs '/home/galaxy/galaxy/database/datasets/000/355/dataset_355049.dat' '/home/galaxy/galaxy/database/datasets/000/355/dataset_355050.dat' &&   Rscript '/home/galaxy/shed_tools/toolshed.g2.bx.psu.edu/repos/artbio/small_rna_maps/f2e7ad3058e8/small_rna_maps'/small_rna_maps.r --first_dataframe '/home/galaxy/galaxy/database/datasets/000/355/dataset_355049.dat' --extra_dataframe '/home/galaxy/galaxy/database/datasets/000/355/dataset_355050.dat' --normalization "1 1 1 1 1 1" --ymin '' --ymax '' --first_plot_method 'Counts' --extra_plot_method 'Size' --output_pdf '/home/galaxy/galaxy/database/datasets/000/355/dataset_355051.dat'

1U enrichment 

sed -r --sandbox -e 's/T/U/g' '/home/galaxy/galaxy/database/datasets/000/354/dataset_354780.dat' > '/home/galaxy/galaxy/database/datasets/000/354/dataset_354784.dat'

grep -P -A 0 -B 0  -i -- "^U"

echo "#lines" > /home/galaxy/galaxy/database/datasets/000/355/dataset_355043.dat &&  cat '/home/galaxy/galaxy/database/datasets/000/355/dataset_355031.dat' | wc -l | awk '{ print $1 }' >> /home/galaxy/galaxy/database/datasets/000/355/dataset_355043.dat


Weblogo pairable figure 3c 

samtools index '/home/galaxy/galaxy/database/datasets/000/354/dataset_354763.dat' && python '/home/galaxy/shed_tools/toolshed.g2.bx.psu.edu/repos/artbio/small_rna_signatures/68ee7c84d498/small_rna_signatures'/overlapping_reads.py --input '/home/galaxy/galaxy/database/datasets/000/354/dataset_354763.dat' --minquery '23' --maxquery '29' --mintarget '23' --maxtarget '29' --overlap '10' --output '/home/galaxy/galaxy/database/datasets/000/354/dataset_354776.dat'

cat '/home/galaxy/galaxy/database/datasets/000/354/dataset_354776.dat' | fastx_trimmer -v -f 1 -l 23         > '/home/galaxy/galaxy/database/datasets/000/354/dataset_354780.dat'

sed -r --sandbox -e 's/T/U/g' '/home/galaxy/galaxy/database/datasets/000/354/dataset_354780.dat' > '/home/galaxy/galaxy/database/datasets/000/354/dataset_354784.dat'

weblogo -u 10 --yaxis 0.7  --size large --title Control1  --errorbars NO --ticmarks 0.1 </Users/Julie/Desktop/fasta.23-29/paired_control1.fasta > paired_control1.eps
![image](https://user-images.githubusercontent.com/122443584/212908774-65e4c2f6-94d1-4dd8-bfd7-3558abeaa21d.png)



# UCSC views figure 3i and extended data figure 4

## Coverage small-seq 23-29 uniquer mapper Ctrl

```
ln -s '/home/galaxy/galaxy/database/datasets/000/378/dataset_378805.dat' one.bam && ln -s '/home/galaxy/galaxy/database/datasets/_metadata_files/072/metadata_72363.dat' one.bam.bai &&  bamCoverage --numberOfProcessors "${GALAXY_SLOTS:-4}"  --bam one.bam --outFileName '/home/galaxy/galaxy/database/datasets/000/414/dataset_414114.dat' --outFileFormat 'bigwig'  --binSize 29   --exactScaling  --scaleFactor 0.034      --minMappingQuality '1'
![image](https://user-images.githubusercontent.com/122443584/212907365-6e354d73-1341-4f13-a140-691ed4e0c86e.png)
```