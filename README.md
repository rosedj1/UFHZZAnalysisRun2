# HZZ Analyzer for CMS Run2

## How to install the Analyzer

```bash
# Prepare your area.
export SCRAM_ARCH=slc7_amd64_gcc700
cmsrel CMSSW_10_6_12
cd CMSSW_10_6_12/src
cmsenv

# Install the repo.
git cms-init
git clone -b 10_6_12 https://ferrico@github.com/ferrico/UFHZZAnalysisRun2.git
git config merge.renameLimit 999999

cp UFHZZAnalysisRun2/install*.sh .
chmod u+x install_2.sh
./install_2.sh
```

## How to use the Analyzer

Now you can run over the template files found in `UFHZZAnalysisRun2/UFHZZ4LAna/python/`:

```bash
cmsRun UFHZZAnalysisRun2/UFHZZ4LAna/python/Sync_102X_2016_Legacy_cfg.py
cmsRun UFHZZAnalysisRun2/UFHZZ4LAna/python/Sync_102X_2017_Legacy_cfg.py
cmsRun UFHZZAnalysisRun2/UFHZZ4LAna/python/Sync_102X_2018_Legacy_cfg.py
```

Only run over **a maximum of 10K events**.
If you need to run over more, then you should submit a CRAB job (remember to test your code locally before submitting to CRAB):

```bash
cp UFHZZAnalysisRun2/Utilities/crab/* .
# Initialize your proxy. If you get errors, check your certificate.
voms-proxy-init --valid=168:00
# May need to instead do: `voms-proxy-init -voms cms -rfc`
source /cvmfs/cms.cern.ch/crab3/crab.sh

# Submit your job:
python SubmitCrabJobs.py -t "myTask_Data" -d datasets_2016ReReco.txt -c UFHZZAnalysisRun2/UFHZZ4LAna/python/templateData_80X_M1703Feb_2l_cfg.py

# or similary for MC:
python SubmitCrabJobs.py -t "myTask_MC" -d datasets_Summer16_25ns_MiniAOD.txt -c UFHZZAnalysisRun2/UFHZZ4LAna/python/templateMC_80X_M17_4l_cfg.py
```

### Check the status of your job

You can use `manageCrabTask.py` to check the status, resubmit, or kill your task. E.g. after submitting:

```bash
nohup python -u manageCrabTask.py -t resultsAna_Data_M17_Feb19 -r -l >& managedata.log &
```

This will start an infinite loop of running crab resubmit on all of your tasks, then sleep for 30 min.
You should kill the process once all of your tasks are done.
Once all of your tasks are done, you should run the following command to purge your crab cache so that it doesn't fill up:

```bash
python manageCrabTask.py -t resultsAna_Data_M17_Feb19 -p
```

## Useful templates

```bash
UFHZZ4LAna/python/templateMC_102X_Legacy16_4l_cfg.py
UFHZZ4LAna/python/templateMC_102X_Legacy17_4l_cfg.py
UFHZZ4LAna/python/templateMC_102X_Legacy18_4l_cfg.py
UFHZZ4LAna/python/templateData_102X_Legacy16_3l_cfg.py
UFHZZ4LAna/python/templateData_102X_Legacy17_3l_cfg.py
UFHZZ4LAna/python/templateData_102X_Legacy18_3l_cfg.py
```

