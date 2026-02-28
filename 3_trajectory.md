#### Trajectory

##### velocity
```bash
#!/bin/bash
#PBS -l nodes=1:ppn=12
#PBS -l mem=50g
#PBS -l walltime=10:00:00
#PBS -m abe
#PBS -N HDAC1_2_KD_day20
#PBS -q medium   
#PBS -o /home/lfliuhku/job.out/HDAC1_2_KD_day20.out
#PBS -e /home/lfliuhku/job.out/HDAC1_2_KD_day20.erro
#PBS -r n
ml Miniconda3/25.7.0-2 
source activate /home/lfliuhku/conda/pkgs/python_envs

# cd /home/lfliuhku/rawdata/HDAC1_twopart/hNP_day20
# velocyto run10x -m /home/lfliuhku/reference/rmsk/hg38_rmsk.gtf /home/lfliuhku/rawdata/HDAC1_twopart/hNP_day20 /home/lfliuhku/reference/refdata-cellranger-arc-GRCh38-2020-A-2.0.0/genes/genes.gtf

cd /home/lfliuhku/rawdata/HDAC1_twopart/HDAC1_2_KD_day20
velocyto run10x -m /home/lfliuhku/reference/rmsk/hg38_rmsk.gtf /home/lfliuhku/rawdata/HDAC1_twopart/HDAC1_2_KD_day20 /home/lfliuhku/reference/refdata-cellranger-arc-GRCh38-2020-A-2.0.0/genes/genes.gtf

```