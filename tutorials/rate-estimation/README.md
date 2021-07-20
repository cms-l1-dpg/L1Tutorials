# L1 menu rate estimation

This tutorial will show how to setup the L1 menu tool framework and compute the rates of the `L1_DoubleEG_10_5_er1p2` and `L1_DoubleMu_15upt_7upt_MassUpt_Min1_BMTF` triggers [see TME hands-on session](../tme-seed-development/)


**The rates and prescales tutorial steps are going to be the following:.**


## Setup instructions
Log in to Lxplus with your username.

```
ssh -X <username>@lxplus.cern.ch

# Setting up the environment and folder structure
cmsrel CMSSW_11_1_5
cd CMSSW_11_1_5/src/
git clone --depth 1 https://github.com/cms-l1-dpg/L1MenuTools.git
cd L1MenuTools/rate-estimation/

# Translating the menu XML file into C++ code
wget https://raw.githubusercontent.com/cms-l1-dpg/L1Tutorials/master/tutorials/rate-estimation/input/L1Menu_Collisions2022_v0_1_1_modified.xml  # alternatively: place your custom menu XML here
bash configure.sh L1Menu_Collisions2022_v0_1_1_modified.xml  # alternatively: provide your custom menu XML

# Compile
cmsenv
mkdir -p objs/include
make -j 8
 
```

Every time the menu is modified the last 4 steps should be re-run.

## Make ntuple list


In order to  make our own ntuple list from the latest Run3 Nutrino Gun L1 ntuples

```

cd ntuple
./makeFileList.py /eos/cms/store/group/dpg_trigger/comm_trigger/L1Trigger/bundocka/condor/reHcalTP_Nu_11_2_105p20p1_1623921599 > Run3_NuGun_MC_ntuples_L1MenuTutorial.list

```

This will crate a list of the paths of all the L1 ntuples that will be used for the rate estimation

## Make prescale table.

The prescale table for our modified menu can be found [here](https://github.com/cms-l1-dpg/L1Tutorials/blob/master/tutorials/rate-estimation/input/PrescaleTable-1_L1Menu_Collisions2022_v0_1_1_modified.csv)

let's get it locally

```

wget https://raw.githubusercontent.com/cms-l1-dpg/L1MenuTools/master/rate-estimation/menu/Prescale_2022_v0_1_1.csv

```

For generating a new prescale table on needs to do the following:

```

cd L1MenuTools/pstools
bash run-ps-generate.sh https://github.com/cms-l1-dpg/L1Menu2018/raw/master/official/PrescaleTables/PrescaleTable-1_L1Menu_Collisions2018_v2_1_0.xlsx https://raw.githubusercontent.com/cms-l1-dpg/L1Tutorials/master/tutorials/rate-estimation/input/L1Menu_Collisions2022_v0_1_1_modified.xml --output /PrescaleTable-1_L1Menu_Collisions2022_v0_1_1_modified

```

The newlly developed seed can be traced using their index:

The L1_DoubleEG\_10\_5\_er1p2  was set to inde 204. Its line in the prescale table is [here](https://github.com/cms-l1-dpg/L1Tutorials/blob/master/tutorials/rate-estimation/input/PrescaleTable-1_L1Menu_Collisions2022_v0_1_1_modified.csv#L160)

The   L1\_DoubleMu\_15upt\_7upt\_MassUpt\_Min1\_BMTF  was set to index 52. Its line in the prescale table is [here](https://github.com/cms-l1-dpg/L1Tutorials/blob/master/tutorials/rate-estimation/input/PrescaleTable-1_L1Menu_Collisions2022_v0_1_1_modified.csv#L48)

<<<<<<< HEAD

```
# Log in to Lxplus with your username.
ssh -X <username>@lxplus.cern.ch

# Setting up the environment and folder structure
cmsrel CMSSW_11_1_5
cd CMSSW_11_1_5/src/
git clone --depth 1 https://github.com/cms-l1-dpg/L1MenuTools.git
cd L1MenuTools/rate-estimation/

# Translating the menu XML file into C++ code
wget https://raw.githubusercontent.com/cms-l1-dpg/L1Tutorials/master/tutorials/rate-estimation/input/L1Menu_Collisions2022_v0_1_1_modified.xml  # alternatively: place your custom menu XML here
bash configure.sh L1Menu_Collisions2022_v0_1_1_modified.xml  # alternatively: provide your custom menu XML

# Compile
cmsenv
mkdir -p objs/include
make -j 8
 
```

Every time the menu is modified the last 4 steps should be re-run.

## Make ntuple list


In order to  make our own ntuple list from the latest Run3 Nutrino Gun L1 ntuples

```

cd ntuple
./makeFileList.py /eos/cms/store/group/dpg_trigger/comm_trigger/L1Trigger/bundocka/condor/reHcalTP_Nu_11_2_105p20p1_1623921599 > Run3_NuGun_MC_ntuples_L1MenuTutorial.list
cd ..

```

This will crate a list of the paths of all the L1 ntuples that will be used for the rate estimation

## Make prescale table.

The prescale table for our modified menu can be found [here](https://github.com/cms-l1-dpg/L1Tutorials/blob/master/tutorials/rate-estimation/input/PrescaleTable-1_L1Menu_Collisions2022_v0_1_1_modified.csv)

let's get it locally

```
cd ./menu
wget https://raw.githubusercontent.com/cms-l1-dpg/L1Tutorials/ratesAndPS/tutorials/rate-estimation/input/PrescaleTable-1_L1Menu_Collisions2022_v0_1_1_modified.csv

```

For generating a new prescale table one needs to do the following:

```

cd L1MenuTools/pstools
bash run-ps-generate.sh https://github.com/cms-l1-dpg/L1Menu2018/raw/master/official/PrescaleTables/PrescaleTable-1_L1Menu_Collisions2018_v2_1_0.xlsx https://raw.githubusercontent.com/cms-l1-dpg/L1Tutorials/master/tutorials/rate-estimation/input/L1Menu_Collisions2022_v0_1_1_modified.xml --output PrescaleTable-1_L1Menu_Collisions2022_v0_1_1_modified

```

The first argument of the command should be an existing prescale table (xlsx format), in our case this can be the default Run 3 collisions menu (PrescaleTable-1\_L1Menu\_Collisions2018\_v2\_1\_0.xlsx) and the second argument should be the newlly deveoped menu in xml format.

The newlly developed seed can be traced using their index:

The L1_DoubleEG\_10\_5\_er1p2  was set to inde 204. Its line in the prescale table is [here](https://github.com/cms-l1-dpg/L1Tutorials/blob/master/tutorials/rate-estimation/input/PrescaleTable-1_L1Menu_Collisions2022_v0_1_1_modified.csv#L160)

The   L1\_DoubleMu\_15upt\_7upt\_MassUpt\_Min1\_BMTF  was set to index 52. Its line in the prescale table is [here](https://github.com/cms-l1-dpg/L1Tutorials/blob/master/tutorials/rate-estimation/input/PrescaleTable-1_L1Menu_Collisions2022_v0_1_1_modified.csv#L48)

Note: In the previous step (geerating PS table for a new menu) by default all the prescale columns of a new seed will be set to 1. 
=======
>>>>>>> 90489ebfb2aae914f4b03399beb5f9900322ce06

## Rate estimation

For the rate estimation we will run ./testMenu2016 with the necessary arguments. This will print the rate table in the terminl screen and enerate three outoup files that contain the rate table (.txt, .csv and .root) under the result directory. 

The full list of available arguments can be displayed 

```
./testMenu2016 --help

```
The results have been produced with the command

```
./testMenu2016 -m menu/PrescaleTable-1_L1Menu_Collisions2022_v0_1_1_modified.csv -l ntuple/Run3_NuGun_MC_ntuples.list -o testoutput -b 2544 --doPlotRate --doPlotEff --maxEvent -1 --SelectCol 2E+34

```

## Results

The result tables can be found [here](https://github.com/cms-l1-dpg/L1Tutorials/tree/ratesAndPS/tutorials/rate-estimation/results)

testoutput.csv(txt): rate table as printed out on the terminal screen
testoutput.root    : trigger rates vs pT and eta (are the seeds grouped somehow?)

Lets check the rate of the new seeds in the .txt file

## Rates vs PU plots

```

cd plots
python CompPUDep.py --outfolder _RatesVSPU --csv ../results/testoutput_PU.csv

```

before running the python command, open CompPUDep.py and add "L1\_DoubleEG\_10\_5\_er1p2" : "L1_DoubleEG\_10\_5\_er1p2" in line 83
=======
The result tables cn be found [here](add link to the result repo with the txt, csv and root files)


Check the rate of the new seed




