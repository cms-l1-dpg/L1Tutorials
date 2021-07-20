# L1 menu rate estimation

This tutorial will show how to setup and cofigure the L1 menu tool framework, how to produce a PS tablesm how to run the tool for a small number of events and examples on how to produce the rate vs PU and rate visualization plots.

We provide you with the [PS tables](https://github.com/cms-l1-dpg/L1Tutorials/blob/ratesAndPS/tutorials/rate-estimation/input/PrescaleTable-1_L1Menu_Collisions2022_v0_1_1_modified.csv), the [rate estimation output](https://github.com/cms-l1-dpg/L1Tutorials/tree/ratesAndPS/tutorials/rate-estimation/results) for the full statistics of the Run3 NuGun sample, the [rate vs PUp plots](https://github.com/cms-l1-dpg/L1Tutorials/tree/ratesAndPS/tutorials/rate-estimation/RateVsPU_plots/Plots_RatesVSPU_NewSeeds) and the [rate bar plot](https://github.com/cms-l1-dpg/L1Tutorials/blob/ratesAndPS/tutorials/rate-estimation/Rate_Visual/rate_visual_percentage%2Brates%2Btotalrate_barPlot.pdf).



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

The default L1 ntuple [list](https://github.com/cms-l1-dpg/L1MenuTools/blob/master/rate-estimation/ntuple/Run3_NuGun_MC_ntuples.list) for the Run3 NuGun sampe is located under ``` /L1MenuTools/rate-estimation/ntuple ``` 

If one wants to produce their own ntuple list the steps are the following: 

```

cd ntuple
./makeFileList.py path_to_ntuples > output_list_name.list

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

The L1\_DoubleEG\_10\_5\_er1p2  was set to inde 204. Its line in the prescale table is [here](https://github.com/cms-l1-dpg/L1Tutorials/blob/master/tutorials/rate-estimation/input/PrescaleTable-1_L1Menu_Collisions2022_v0_1_1_modified.csv#L160)

The   L1\_DoubleMu\_15upt\_7upt\_MassUpt\_Min1\_BMTF  was set to index 52. Its line in the prescale table is [here](https://github.com/cms-l1-dpg/L1Tutorials/blob/master/tutorials/rate-estimation/input/PrescaleTable-1_L1Menu_Collisions2022_v0_1_1_modified.csv#L48)


## Estimate the L1 rate 

We will demonstrate how to run the tool for a small amount of data (20k).

```

L1MenuTools/rate-estimation
./testMenu2016 -m menu/PrescaleTable-1_L1Menu_Collisions2022_v0_1_1_modified.csv -l ntuple/Run3_NuGun_MC_ntuples.list -o testoutput -b 2544 --doPlotRate --doPlotEff --maxEvent 20000 --SelectCol 2E+34 --doPrintPU

```


## Results

The result tables for the run on the full statistics can be found [here](https://github.com/cms-l1-dpg/L1Tutorials/tree/ratesAndPS/tutorials/rate-estimation/results)

testoutput.csv(txt): rate table as printed out on the terminal screen, to be used for the rate visualization plot
testoutput.root    : trigger rates vs pT and eta
testoutput\_PU.csv : seed names, accepted events and PS, to be used in the rate vs PU plot

Lets check the rate of the new seeds [here](https://github.com/cms-l1-dpg/L1Tutorials/blob/ratesAndPS/tutorials/rate-estimation/results/testoutput.csv)

## Rates vs PU plots

For the rate vs PU plot production the --doPrintPU should be passed asargyment in the previous step.


```

cd /L1MenuTools/rate-estimation/plots
python CompPUDep.py --outfolder RatesVSPU --csv ../results/testoutput_PU.csv

```

before running the python command, open CompPUDep.py and add "L1\_DoubleEG\_10\_5\_er1p2" : "L1\_DoubleEG\_10\_5\_er1p2" in line 83
The result of the above command can be found [here](https://github.com/cms-l1-dpg/L1Tutorials/tree/ratesAndPS/tutorials/rate-estimation/RateVsPU_plots/Plots_RatesVSPU_NewSeeds)

## RAte visualization

In order to produce a rate bar plot for the different seed categories of the menu

```

cd src/L1MenuTools/rate-visualization
bash run-visualize.sh --rateTable ../rate-estimation/results/testoutput.csv --output rate_visual --textOnBarPlot percentage+rates+totalrate

```

The result bar plot can be found [here](https://github.com/cms-l1-dpg/L1Tutorials/blob/ratesAndPS/tutorials/rate-estimation/Rate_Visual/rate_visual_percentage%2Brates%2Btotalrate_barPlot.pdf)







