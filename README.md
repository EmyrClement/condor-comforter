# condor-comforter
Helper routines for using condor at Bristol, particularly aimed at CMS users.

These will probably be a little hacky, but should offer some inspiration for other users.

Please report any issues, and add any helpful scripts for other users!

Most things require [`htcondenser`](https://github.com/raggleton/htcondenser).

Robin Aggleton

##cmsRunCondor

This holds example code for running CMSSW jobs on condor. Like CRAB3, but on condor.
Currently only supports output to `/hdfs`.

Start from: [cmsRunCondor.py](cmsRun/cmsRunCondor.py) for running over one dataset with a config file.

Brief example **requires CMSSW 80X release**:

```
./cmsRunCondor.py pset_tutorial_analysis.py \
--dataset /QCD_HT500to700_TuneCUETP8M1_13TeV-madgraphMLM-pythia8/RunIISpring16MiniAODv2-PUSpring16_80X_mcRun2_asymptotic_2016_miniAODv2_v0-v1/MINIAODSIM \
--totalUnits 10 --unitsPerJob 5 --splitByFiles \
--outputDir /hdfs/user/$LOGNAME/cmsRunCondor \
--dag
```

You can then monitor job progress with `DAGstatus.py` (part of `htcondenser` package)

See all options by doing `cmsRunCondor.py --help`.

Features currently supported:

- Run using a LumiMask, and/or specified run numbers

- Run over all or part of a dataset

- Split into jobs by # files or # lumisections

- Run with a secondary dataset to do "2-file solution" (e.g. mixing RECO with RAW)

- Run with a specified list of files

- Specify additional input files needed for running (e.g. calibration files)

- Easy monitoring of jobs using `DAGstatus`

- Profile cmsRun jobs with valgrind or callgrind

##haddaway

Simple script to put `hadd` jobs onto HTCondor, splitting them up into smaller parallel groups.

##exampleDAG

This holds a simple example of a DAG (directed acyclic graph), i.e. a nice way to schedule various jobs, each of which can depend on other jobs.
It shows how to setup and 'connect' jobs with parent-child relationships, and how to pass variables to the condor job file.

Start from: [diamond.dag](exampleDAG/diamond.dag)

Also includes a neat little monitoring script for DAG jobs, [DAGstatus.py](exampleDAG/DAGstatus.py)

##simpleJob

A very simple condor job file to run a script on the worker node.

Start from: [script.job](simpleJob/script.job)
