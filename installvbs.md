cd /home/users/eslam.zenhom/public_html/uf_work/vbs2/analysis
source setup.sh;  
export PYTHONPATH="${PYTHONPATH}:/home/users/eslam.zenhom/public_html/uf_work/vbs2/analysis"; 
source scripts/runall_vbsvvhjets_semimerged.sh 

   #copy files from uaf to hipergator for training 
   scp -r /home/users/eslam.zenhom/public_html/uf_work/vbs2/analysis/studies/vbsvvhjets_semimerged/output_abcdnet_v6/Run2 eslam.zenhom@hpg.rc.ufl.edu:/blue/p.chang/eslam.zenhom/data/vbsvvh/vbsvvh_semimerged_data/ 
   sbatch --nodes=1 --cpus-per-task=16 --gpus=2  batch/full300.script configs/vbsvvh_semimerged_finalrun_correctsigfiles_150lamda.json
   sbatch --nodes=1 --cpus-per-task=16 --gpus=2  batch/full300.script configs/vbsvvh_semimerged_finalrun_correctsigfiles_50lamda.json
   
   #after finishing copy the files (you must be in hipergator device):
   ##for the 50 lambda
   scp -r /home/eslam.zenhom/data/vbsvvh/vbsvvh_semimerged_finalrun_correctsigfiles_50lamda eslam.zenhom@uaf-10.t2.ucsd.edu:/home/users/eslam.zenhom/public_html/uf_work/vbs2/abcdnet/
   scp /home/eslam.zenhom/uf_work/abcd/vbs/abcdnet/configs/vbsvvh_semimerged_finalrun_correctsigfiles_50lamda.json eslam.zenhom@uaf-10.t2.ucsd.edu:/home/users/eslam.zenhom/public_html/uf_work/vbs2/abcdnet/configs
   ###MODEL=/home/users/eslam.zenhom/public_html/uf_work/vbs2/abcdnet/vbsvvh_semimerged_finalrun_correctsigfiles_50lamda/models/vbsvvh_semimerged_finalrun_correctsigfiles_50lamda_modelLeakyNeuralNetwork_nhidden3_hiddensize64_lrConstantLR0.001_discolambda50_epoch300_model.pt
   ### CONFIG=configs/vbsvvh_semimerged_finalrun_correctsigfiles_50lamda.json
   
   
   ##for the 150 lamda
   scp -r /home/eslam.zenhom/data/vbsvvh/vbsvvh_semimerged_finalrun_correctsigfiles_150lamda eslam.zenhom@uaf-10.t2.ucsd.edu:/home/users/eslam.zenhom/public_html/uf_work/vbs2/abcdnet/
   scp /home/eslam.zenhom/uf_work/abcd/vbs/abcdnet/configs/vbsvvh_semimerged_finalrun_correctsigfiles_150lamda.json eslam.zenhom@uaf-10.t2.ucsd.edu:/home/users/eslam.zenhom/public_html/uf_work/vbs2/abcdnet/configs
   ###MODEL=/home/users/eslam.zenhom/public_html/uf_work/vbs2/abcdnet/vbsvvh_semimerged_finalrun_correctsigfiles_150lamda/models/vbsvvh_semimerged_finalrun_correctsigfiles_150lamda_modelLeakyNeuralNetwork_nhidden3_hiddensize64_lrConstantLR0.001_discolambda150_epoch300_model.pt
   ### CONFIG=configs/vbsvvh_semimerged_finalrun_correctsigfiles_150lamda.json


 
## running all after training
source root.sh
cd /home/users/eslam.zenhom/public_html/uf_work/vbs2/analysis
source setup.sh;  
export PYTHONPATH="${PYTHONPATH}:/home/users/eslam.zenhom/public_html/uf_work/vbs2/analysis"; 
source scripts/runall_vbsvvhjets_semimerged.sh 
## note: need to change things in runnall like TAG=abcdnet_v7

source root.sh
cd public_html/uf_work/vbs2/abcdnet
source uniqueenv/bin/activate 
source setup_ucsd.sh 
source scripts/inferall_vbsvvhjets.sh 
## note: need to change things in runnall like TAG=abcdnet_v7 and what config files and models u r using

cd /home/users/eslam.zenhom/public_html/uf_work/vbs2/analysis 
source env_analysis/bin/activate 
source setup.sh; 
export PYTHONPATH="${PYTHONPATH}:/home/users/eslam.zenhom/public_html/uf_work/vbs2/analysis"; 
source scripts/addall_vbsvvhjets.sh 
## note: need to change things in addall like TAG=abcdnet_v7




## datacard:
cd 
source .bashrc 
cd /home/users/eslam.zenhom/public_html/uf_work/vbs2/analysis 
source setup.sh;  
export PYTHONPATH="${PYTHONPATH}:/home/users/eslam.zenhom/public_html/uf_work/vbs2/analysis";  
cd /home/users/eslam.zenhom/public_html/uf_work/vbs2/analysis/scripts 
conda activate myenv 
cd .. 
python3 scripts/make_datacards_vbsvvh.py abcdnet_v11
## don't forget to change the argument at the end of the command : abcdnet_v11
 
## run grapher:
## note need to change cuts in the run_grapher_all.sh and the output version tag
source root.sh
cd /home/users/eslam.zenhom/public_html/uf_work/vbs2/analysis/scripts
source run_grapher_all.sh

## run limits:
cd 

source setup_higgs_combine.sh 
cd /home/users/eslam.zenhom/public_html/uf_work/vbs2/combine/vbsvvh  
sh runLimits.sh datacards/VBSVVH_semimerged_abcdnet_v11 

sh plotLimits.sh results/VBSVVH_semimerged_abcdnet_v11
