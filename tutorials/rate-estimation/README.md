# L1 menu rate estimation

This tutorial will demonstrate how to set up the rate estimation tool and will present examples on how to prepare the ntuple list and the PS tables with your new seed. We will go through an example command for rate estimation with a small number of events (20k) and at the end we will see examples of how to produce a rate VS PU and a rate bar and piechart plot. 

The idea of this hands-on session is to walk you through all the necessary steps for estimating the rate of a L1 menu that includes your new seeds and produce the rate plots. The participants are encouraged to follow the live demonstration instead of trying to execute the steps by themself on real time.  
All the ingredients (ntuple list, PS tables), the L1 rate tables with the full Run3 NuGun statistics and the rate VS PU and rate bar and piechart plots will be provided by us such that everyone can follow steps offline.

## Setup instructions

Log in to Lxplus with your username.
```
ssh -X <username>@lxplus.cern.ch
```

Clone the L1MenuTools repo
```
cmsrel CMSSW_11_1_5
cd CMSSW_11_1_5/src/
git clone --depth 1 https://github.com/cms-l1-dpg/L1MenuTools.git
cd L1MenuTools/rate-estimation/
```

Get the xml menu file locally and translate it into c++ code
```
wget https://raw.githubusercontent.com/cms-l1-dpg/L1MenuRun3/master/development/L1Menu_Collisions2022_v0_1_1/L1Menu_Collisions2022_v0_1_1.xml  # alternatively: place your custom menu XML here
bash configure.sh configure.sh L1Menu_Collisions2022_v0_1_1.xml  # alternatively: provide your custom menu XML
```

Note: This brings and translates the baseline Run3 menu, without the newly developed seeds implemented. For the purpose of this exercise we will get the modified menu and translate it to C++ code in a few steps

Compile
```
cmsenv
mkdir -p objs/include
make -j 8
```

Every time the menu is modified the last 5 steps should be repeated.

In order to get the modified menu that contains the new seed the steps are the following:
```
wget https://raw.githubusercontent.com/cms-l1-dpg/L1Tutorials/master/tutorials/rate-estimation/input/L1Menu_Collisions2022_v0_1_1_modified.xml
bash configure.sh L1Menu_Collisions2022_v0_1_1_modified.xml
cmsenv
mkdir -p objs/include
make -j 8
```

## Produce all the ingredients and estimate the L1 rates

### 1. Make ntuple list

The default L1 ntuple list for the Run3 NuGun sample can be found [here](https://github.com/cms-l1-dpg/L1MenuTools/blob/master/rate-estimation/ntuple/Run3_NuGun_MC_ntuples.list) 

For producing your own ntuple list the steps are the following: 
```
cd ./ntuple
./makeFileList.py path_to_ntuples > output_list_name.list
```

This creates a list of the paths of all the L1 ntuples that will be used for the rate estimation.

For example, if we want to recreate the Run3 NuGun ntuple list the steps are:
```
cd ./ntuple
./makeFileList.py /eos/cms/store/group/dpg_trigger/comm_trigger/L1Trigger/stempl/condor/menu_Nu_11_0_X_1614189426/ > testNuGun.list
```


### 2. Make prescale table

The PS table for our modified menu can be found [here](https://github.com/cms-l1-dpg/L1Tutorials/blob/master/tutorials/rate-estimation/input/PrescaleTable-1_L1Menu_Collisions2022_v0_1_1_modified.csv)

In order to create this new PS table that contains the PS columns for the new seeds from scratch, the steps to follow are: 
```
cd L1MenuTools/pstools
bash run-ps-generate.sh https://github.com/cms-l1-dpg/L1Menu2018/raw/master/official/PrescaleTables/PrescaleTable-1_L1Menu_Collisions2018_v2_1_0.xlsx https://raw.githubusercontent.com/cms-l1-dpg/L1Tutorials/master/tutorials/rate-estimation/input/L1Menu_Collisions2022_v0_1_1_modified.xml --output PrescaleTable-1_L1Menu_Collisions2022_v0_1_1_modified
```                                                                                                                                                                             
Firstly, you have to provide an already existing PS table (in xlsx format) and then you have to provide the menu with your new seeds.

The available options are
* --newSeedPS: specifies the number of PS to use for the new seeds, by default PS of the new seeds is set to 1
* --includeBptx: PS is set to zero for trigger seeds using Bptx and NoBptx due to problems in emulation


**I. How can you set PS = 2 to all the new seeds?**
    <details>
    <summary>Answer (click to expand)</summary>
    Adding the --newSeedPS 2 in the command above
    </details>


**II. What PS should I use when I start my L1 seed rate studies?**
    <details>
    <summary>Answer (click to expand)</summary>
    For the beggining of your study we suggest that you start with PS = 1 for your new seed. This way you can check the initial rate of your seed and then study how you can control it with PS.
    </details>

The PS of the new seeds are:
* ```L1_DoubleEG_10_5_er1p2``` is [here](https://github.com/cms-l1-dpg/L1Tutorials/blob/master/tutorials/rate-estimation/input/PrescaleTable-1_L1Menu_Collisions2022_v0_1_1_modified.csv#L160)
* ```L1_DoubleMu_15upt_7upt_MassUpt_Min1_BMTF``` is [here](https://github.com/cms-l1-dpg/L1Tutorials/blob/master/tutorials/rate-estimation/input/PrescaleTable-1_L1Menu_Collisions2022_v0_1_1_modified.csv#L48)


### 3. Estimate the L1 rate

Let's see how to run the rate tool for a small number of events (20k) and estimate the rate of our new menu
```
cd L1MenuTools/rate-estimation
./testMenu2016 -m menu/PrescaleTable-1_L1Menu_Collisions2022_v0_1_1_modified.csv -l ntuple/Run3_NuGun_MC_ntuples.list -o testoutput -b 2544 --doPlotRate --doPlotEff --maxEvent 20000 --SelectCol 2E+34 --doPrintPU
```

The rate estimation tool will output the rate table in txt and csv format, a root file with the rates of the L1 seeds vs pT and eta. All these files can be found [here](https://github.com/cms-l1-dpg/L1Tutorials/tree/ratesAndPS/tutorials/rate-estimation/results/)
Additionally a [testoutput\_PU.csv](https://raw.githubusercontent.com/cms-l1-dpg/L1Tutorials/ratesAndPS/tutorials/rate-estimation/results/testoutput_PU.csv) is produced when the ```--doPrintPU``` is used. This contains the seed names, PU bins, total events, PS value and number of events fired the trigger in every PU bin. This file will be used for the rate VS PU plotting.


**III. How many events should I run for my studies?**
    <details>
    <summary>Answer (click to exand)</summary>
     As many as possible! Here we demostrate only a small number of events due to time constraints. The rate tables in the results directory have been produced with the full stats of the Run3 NuGun MC sample.
     </details>


**IV. What are the pure and proportional rates of the new seeds?** 
    <details> 
    <summary> Answer (click to expand) </summary>
     For the L1\_DoubleMu\_15upt\_7upt_MassUpt\_Min1\_BMTF is [here](https://github.com/cms-l1-dpg/L1Tutorials/blob/ratesAndPS/tutorials/rate-estimation/results/testoutput.txt#L400) and for the L1\_DoubleEG\_10\_5\_er1p2``` [here](https://github.com/cms-l1-dpg/L1Tutorials/blob/ratesAndPS/tutorials/rate-estimation/results/testoutput.txt#L512) </details>


**V. How much is each one of the new seeds adding to the total rate?**
    <details>
    <summary> Answer (click to exand) </summary>
    The ```L1_DoubleMu_15upt_7upt_MassUpt_Min1_BMTF``` has a pure rate = 0. The ```L1_DoubleEG_10_5_er1p2``` has pure rate = 230908 Hz.
</details>


**VI. How can we control the rate of the ```L1_DoubleEG_10_5_er1p2``` seed?**
    <details>  
    <summary> Answer (click to expand)</summary>
    Possible options for controlling very high rates of seeds are the optimizing the cuts of the seeds and/or the increasing the PS
    </details>


**VII. How does the rate change if the PS for ```L1_DoubleEG_10_5_er1p2``` is set to 10?**
    <details> 
    <summary> Answer (click to expand) </summary>
    We made a new PS table, set the PS =10 for the new seeds and run the rate estimation tool again for the rull Rin3 NuGun Stats. The results are [here](https://github.com/cms-l1-dpg/L1Tutorials/blob/ratesAndPS/tutorials/rate-estimation/results/testoutput_PS10.txt#L512)
    The pure rate of the ```L1_DoubleEG_10_5_er1p2``` is decreased by 1/10 (as expected)
    </details>


### 4. Rates vs PU and rate visualization plots


* For the rate vs PU plot production the --doPrintPU should be passed as argument in the previous step.
  before running the python command, open CompPUDep.py and add "L1\_DoubleEG\_10\_5\_er1p2" : "L1\_DoubleEG\_10\_5\_er1p2" in line 83
  ```
  cd /L1MenuTools/rate-estimation/plots
  python CompPUDep.py --outfolder RatesVSPU --csv ../results/testoutput_PU.csv
  ```

  The rate vs PU plots can be found [here](https://github.com/cms-l1-dpg/L1Tutorials/tree/ratesAndPS/tutorials/rate-estimation/RateVsPU_plots/Plots_RatesVSPU_NewSeeds)


* For the rate visualization plots (piechart and bar plots)
  ```
  cd src/L1MenuTools/rate-visualization
  bash run-visualize.sh --rateTable ../rate-estimation/results/testoutput.csv --output rate_visual --textOnBarPlot percentage+rates+totalrate
  ```

  The plots can be found [here](https://github.com/cms-l1-dpg/L1Tutorials/blob/ratesAndPS/tutorials/rate-estimation/Rate_Visual/)

