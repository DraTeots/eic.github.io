---
title: Software News June
author: Markus Diefenthaler
layout: default
symbol: glyphicon-calendar
until: 2020-12-31
---
<p/>

<img src="{{ '/assets/images/site/EICUG-SWG-News-Banner.png' | relative_url }}" width="800"/>

The EICUG Software Working Group is working on physics and detector simulations that enable for the Yellow Report Initiative a quantitative assessment of the measurement capabilities of the EIC detector(s) and their physics impact. The common simulation tools and workflow environment being setup by the working group allow the EICUG to pursue Yellow Report studies in a manner that is accessible, consistent, and reproducible to the EICUG as a whole.



#### Software Update
In our first Software News, updates on eic_smear and ESCalate are included. Further updates on these and other packages will be in future newsletters. 


##### eic-smear

###### Stand-alone detector repository with versioning

We have separated the detector smearing scripts from the main code. If you have eic-smear installed, use [this repository](https://github.com/eic/eicsmeardetectors) to quickly select and experiment with different versions simply via
```
$ git clone https://github.com/eic/eicsmeardetectors.git
$ root -l
root[] gSystem->Load("libeicsmear")
root[] .L eicsmeardetectors/SmearHandBook_1_2.cxx
root[] SmearTree(BuildHandBook_1_2(), "in.root", "out.root")
```
The README.md, which nicely displays on the github page, has a table with all available versions and more instructions.
Improvements to eic-smear for comfortable compiling a local detector script and/or using a library of all available ones is in the works.


###### Advanced usage example

In order to demonstrate some of the more subtle issues of eic-smear output, we created an [example project](github.com:eic/eicsmear-jetexample.git).

The repo just has two cxx files, meant to be self-documenting, and a detailed README. The first example demonstrates how to use just smeared output, the second how to compare to the truth level and/or how to extract some information from the truth level that should be in the smeared level but as of now isn't yet.

It is intentionally meant to be stand-alone, with a very simple Makefile and no cmake complications.
Please note that while the sample analysis is jet-specific, this is purely because that is a natural application to show 4-vector calculations from partial information.


##### Fun4All
Fun4All is a well established simulation/reconstruction framework initially developed to reconstruct and analyze data from the PHENIX experiment. In recent years it has provided the simulations to shepherd the sPHENIX experiment from the first drawing on a napkin all the way through CD1. For the EIC it was used to develop a concept for an [EIC detector based on the Babar magnet](https://arxiv.org/pdf/1402.1209.pdf) and [an EIC detector Built around the sPHENIX Solenoid](https://indico.bnl.gov/event/5283/attachments/20546/27556/eic-sphenix-dds-final-2018-10-30.pdf). It can be used interactively (run a few events, have a look, run a few more, look again) as well as running massive productions on large farms (and as of recent also on CORI and on the OSG). For more details (and program flow charts) have a look at the [presentation](https://indico.in2p3.fr/event/18281/contributions/71607/) at the 2019 EIC User Group Meeting.

###### New developments
All fieldmaps (Babar, Beast and JLeic) are now available. They can be chosen on the ROOT macro level for details have a look at this [tutorial](https://github.com/eic/fun4all_eic_tutorials/tree/master/MagneticField).

<img src="{{ '/assets/images/diagrams/fun4all/fun4all_fieldmaps.png' | relative_url }}" width="800"/>

The magnetic field was verified using charged geantinos (shown here are 0.5, 1, 2, 3 GeV/c) compared to constant solenoidal fields as shown on the right for the Beast magnet.

Importing detectors via gdml files has its quirks but is now well understood and fully supported. It is not possible to do this generically since one has to address the volumes by name but this only needs minor modifications to existing code and is largely cut and paste.

<img src="{{ '/assets/images/diagrams/fun4all/fun4all_gdml.png' | relative_url }}" width="800"/>

Here are the latest additions to our selection of implemented detectors (all of which can be combined in a ROOT macro). We have a detailed model of the [Beast magnet](https://github.com/eic/fun4all_eicdetectors/tree/master/source) (already used in the fieldmap plots) as well as the [*All Silicon Tracker*](https://github.com/eic/g4lblvtx) and the EIC beam pipe.

The ability to combine these (and other detectors) at will is shown here:

<img src="{{ '/assets/images/diagrams/fun4all/fun4all_allsilicon.png' | relative_url }}" width="800"/>

where the *All Silicon Tracker* is put inside the Beast magnet and is being used as central tracker inside the EIC detector based on the Babar magnet.


###### Verification
Fun4All is also being used to reconstruct test beam data which allows a direct comparison of simulations with data. sPHENIX has performed extensive test beam campaigns for its detectors at energies which are relevant for EIC detectors. The calorimeter test beam results have been [published](https://ieeexplore.ieee.org/document/8519782) recently, the analysis of TPC and Maps detector data is ongoing. A good agreement with previous EICRoot based simulations has been found for the All Silicon Tracker ([slide 8](https://indico.bnl.gov/event/7894/contributions/37609/attachments/28098/43125/200514_AllSi_in_Fun4All_2.pdf))


## ESCalate

<img src="{{ '/assets/images/diagrams/escalate/escalate-logo.png' | relative_url }}" width="150" style="float:left; padding: 10px 20px 10px 20px;"/>

**ESCalate** is a modern framework, which brings together Monte Carlo event generators, 
[Eic Smear](https://gitlab.com/eic/eic-smear.git) for fast simulations, 
[G4E](https://gitlab.com/eic/escalate/g4e.git) for full detector simulations, 
as well as 
[JANA2](https://github.com/JeffersonLab/JANA2) and 
[eJana](https://gitlab.com/eic/escalate/ejana.git) for analysis and reconstruction.
The framework is modular on package and library levels, provides command line interface, 
python and jupyter APIs and ensures data consistency between its loose coupled parts.

<div style="clear: left;"></div>

#### Fast simulations made simple

<img src="https://gitlab.com/eic/escalate/smear/-/raw/master/logo.png" width="150" style="float:left; padding: 10px 20px 10px 20px;"/>

Now it is easy to smear any EIC MCEG supported file as simple as running a console command:

```bash
smear my_mc_file.txt
```
(And it is also easy to select a detector, its version, events to process, etc.)

<div style="clear: left;"></div>

Smear tool can use different *smearing engines* under the hood. Eic-Smear C++ API is the main one, 
but one can also select detectors from Simple Smear written by Yulia Furletova. 
We are now considering integration of Delphes as the third engine.

The ```smear``` command requires installation of ROOT and other packages, so we have instructions for different
scenarios:

1. [Run docker on your local machine](https://gitlab.com/eic/escalate/smear/-/blob/master/simple_instruction_docker.md),
2. [Using singularity (at labs or locally)](https://gitlab.com/eic/escalate/smear/-/blob/master/simple_instruction_singularity.md)
3. [Run directly on IFarm or JLab Jupyterhub](https://gitlab.com/eic/escalate/smear/-/blob/master/simple_instruction_ifarm.md)
4. [Full documentation](https://gitlab.com/eic/escalate/smear/)
 

#### Standalone plugins

In many cases one can analyse the output of the ESCalate framework with ROOT macros, pyROOT or uproot. 
But for extended cases, reconstruction or extension of eJana users can write their own plugins. 
Assuming that the most convenient workflow for users is to keep their analysis in their own separate repositories, 
we added examples of standalone plugins, which work completely independently of eJana location, 
so no modification of the central repository is needed. The [gallery of standalone plugins with examples is here](https://gitlab.com/eic/escalate/plugins) and will be expanding.
 
It is also possible to use pyjano to generate new plugins on fly. Newly generated plugins instanly work with 
command line, python or in Jupyter. 


#### HepMC3, file converter and Delphes

<img src="https://avatars3.githubusercontent.com/u/8037711?s=200&v=4" width="150" style="float:left; padding: 10px 20px 10px 20px;"/>

As [HepMC3](https://gitlab.cern.ch/hepmc/HepMC3) was named "stable" with the 3.2.1 release, 
we switched eJana to work with HepMC3 (previously it worked with HepMC2), which can read files of both versions.

We also now provide ```hepmc_writer``` plugin. As eJana can read a number of Lund file formats (Beagle, HepEvents, PythiaEIC, etc.) it can be easily used as a converter
of Beagle and other files to HepMC3 which later can be processed with Delphes, in [python](https://pypi.org/project/HepMC3/), etc.
As there are users adapting [Delphes for EIC](https://github.com/miguelignacio/delphes_EIC), the work is being done towards verification and validation 
of using the feature in Delphes workflow.

<div style="clear: left;"></div>

Example command to convert Beagle file to HepMC3:

```bash
ejana -Pplugins=beagle_reader,hepmc_writer beagle_file.txt
```

Beagle and other generators provide extended info such as true x, Q2 and other DIS values, one of the advantages of HepMC3
is that now it allows to have custom attributes for events and particles, i.e. allows to save those values. Now only 
basic number of values is supported but work is ongoing to ship them all at least for main MCEG such as Beagle. 


#### eJana, G4E, Containers

- Everything versioned. One can now get the [versions table](https://gitlab.com/eic/containers) of ESCalate images.
- In Cloud. One now can open Jupyter examples library on Binder [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gl/eic%2Fescalate%2Fworkspace/master)
(It might take some time to load as Binder is free, has limited resources and our image provide ROOT, Geant4 and other giant packages)
So no installation is needed. While Binder provides limiter resources, we work on adding an ability to submit a job to OSG right from the Jupyter.     
- Validation and Analysis. We added some validation and analysis plugins for ejana, that allows to check fast simulation
results as well as to get DIS plots (x, Q2, t, etc.) with a simple CLI command. 
- G4E has many changes mainly related to forward and far forward region such as new ZDC, virtual detectors for B0 tracking, etc.
(Results are presented in Pavia's meeting and on [Meson and Kaon structure workshop](https://indico.bnl.gov/event/8315/overview))
<img src="{{ '/assets/images/diagrams/escalate/G4E_with_zdc.png' | relative_url }}" width="250" style="float:left; padding: 10px 20px 10px 20px;"/>











###### Help
Help is available via [our support channel in Mattermost](https://chat.sdcc.bnl.gov/eic/channels/fun4all-software-support), non Scientific Data Center Accounts need an invite - contact us