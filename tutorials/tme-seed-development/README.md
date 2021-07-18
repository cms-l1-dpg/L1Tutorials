# L1 seed development with the Trigger Menu Editor (TME)

This tutorial will show how to modify/extend an existing L1 menu with your own
custom L1 seeds using the Trigger Menu Editor (TME).

First, the TME setup procedure is explained.

Then, two exercises exemplify the addition of custom L1 seeds to an existing L1 menu.
The first exercise involves a rather simple modification of an existing seed.
The second one requires the treatment of a seed that is a bit more complicated.

**The following tutorial steps are to be followed in the given order.**


## Setup instructions

The following describes the TME setup steps as of July 2021. Please refer to the
[TME website](https://globaltrigger.web.cern.ch/globaltrigger/upgrade/tme) for the latest setup instructions.

> The setup instructions assume a bare CC7 Lxplus environment. The TME can be
> installed locally on some systems (e.g., Ubuntu), which is more convenient
> in many cases.

Log in to Lxplus with your username and enable X11 forwarding (with the option `-X`).
```
ssh -X <username>@lxplus.cern.ch
```

Clone this repository and navigate to the relevant subfolder:
```
git clone https://github.com/cms-l1-dpg/L1Tutorials.git
cd L1Tutorials/tutorials/tme-seed-development/
```

Install the package `tm-editor` in a virtual environment:
```
python3 -m venv tme
. tme/bin/activate
pip install --upgrade pip
pip install git+https://github.com/cms-l1-globaltrigger/tm-editor.git@0.12.1
```

> If the last command returns an error, it might be necessary to downgrade PyQt5 to <5.15. Run `pip install "PyQt5<5.15"` and repeat the last "pip install" command.

That's it. You can now start the TME with
```
tm-editor
```

When starting a new terminal sessions, you do not need to re-install `tm-editor` from scratch. Simply activate the `tme` virtual environment again using `. tme/bin/activate` at the corresponding path.


## Seed development

In this exercise, you will develop two custom L1 seeds.

The first seed is a modification of an existing "DoubleEG" seed and should be a rather straightforward exercise.
The second seed is an alteration of a "DoubleMuon" seed and involves a couple of simultaneous seed modifications.

**It is recommended to follow the two exercises in order. But don't worry if you get stuck at one step. The full solutions will be provided at the end, so you will be able to proceed with the tutorial regardless.**


### Exercise 1: `L1_DoubleEG_10_5_er1p2`

[Click here for instructions.](exercise-1.md)


### Exercise 2: `L1_DoubleMu_15upt_7upt_MassUpt_Min1_BMTF`

[Click here for instructions.](exercise-2.md)


## Results

TBD
