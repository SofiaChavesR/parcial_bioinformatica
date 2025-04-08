# PARCIAL BIOINFORMÁTICA
## Sofia Carolina Chaves Rodriguez


### Punto 1
1. Primero descargué las secuencias al computador y para moverlas a mi carpeta en ubunto utilicé el comando ```mv home/Descargas/AB201258.fasta AB56.fasta AJ52.fasta AJ54.fasta AP0064.fasta AP70.fasta AP72.fasta AP75.fasta X04.fasta /\\wsl.localhost\Ubuntu\root\sofia_bioinfo/parcial```,
luego para concatenarlas usé ``` cat AB201258.fasta AB56.fasta AJ52.fasta AJ54.fasta AP0064.fasta AP70.fasta AP72.fasta AP75.fasta X04.fasta > concat.fasta``` ya que con ```cat``` puedo concatenar varios archivos y con ```>``` se le asigna el nombre de salida al archivo resultante ```concatenado.fasta```
luego lo paso a la carpeta "parcial1_SofiaChavesR/blastn" en el cluster ```scp -r -i llave/bio.pt.pem -P 37022 /root/sofia_bioinfo/parcial/ bio.pt@login01-hpc.urosario.edu.co:/home/bio.pt/data/Parcial1/parcial1_SofiaChavesR/blastn```
2. ```sed -E -i '13s/^([A-Za-z]+) ([a-z]+) \([A-Za-z]+, [0-9]{4}\)$/\1.\2_>AB201258.1_Balaenoptera_edeni/' concatenado.fasta```

3. hacer  base de datos tipo Blastn ```makeblastdb -in concatenado.fasta -dbtype nucl -parse_seqids -out trans_ballenas -title "ballenas_transcriptome" ```

4. Blast para saber la identidad de la especie y gen ```blastn -query query.fasta -task megablast -db trans_ballenas -outfmt 7 -word_size 7 -out blast_concatquery -num_threads 1```

5. El indidviduo AJ554052.1  tiene el porcentaje de identidad más alto (64), siendo la que tiene la mejor calidad, aunque el mayor bitscore (1062) lo tiene AB201258.1 ya la que mas se parece al query es AB201258.1, ya que aunque no tiene un gran porcentaje de identidad (3), es el  que tiene de los mayores subject accuracy (584) y el mayor bitscore (1062) 


### Punto 2
6. concatenadas en un solo fasta ```con.fasta```

7.
```
#!/bin/bash
#SBATCH -p normal
#SBATCH -N 1
#SBATCH -n 10
#SBATCH -t 0-20:00
#SBATCH -o salida.out
#SBATCH -e error.err
#SBATCH --mail-user=sofia.chaves@urosario.edu.co
#SBATCH --mail-type=ALL
muscle -in con.fasta -out alinea_muscle.fasta
```

8.  ```iqtree -s FcC_supermatrix.phy -m TEST -bb 1000 -pre alinea_muscle.fasta```



![arbol de maxima verosimilitud] (https://github.com/SofiaChavesR/parcial_bioinformatica/blob/main/alinea_muscle.fasta.treefile.png)
los indidviduos AJ554052 Y AP008475 son los que no están asociados a ningún grupo mientras que los otros estan cercanamente relacionados entre si, siendo seq1 y AB201258 los que están más cercanamente relacionados.

### Punto 3

12. las secuencias de proteinas no tienen intrones debido a que los intrones no tienen intrones porque ya han pasado por el proceso de transcripción y procesamiento del ARN, y representan la versión final traducida del gen.






