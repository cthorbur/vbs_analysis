# vbs_analysis
The repository contains the principal files used in the analysis.

<b> 4l channel, ZX data driven MC </b>

For this part the following CMSSW version was used: CMSSW_8_0_26_patch1

The folder data_driven_MC must be included in the src directory.
The following instructions are necessary:

The discriminant is defined in: src/run_makeZX.cpp

cd ext/

sh compile_ext.sh

cd ..

source set_library.sh

make run_makeZX

./run_makeZX

The necessary variables (ZZMass dbkg_kin weight) will be stored in ZX.root

<b> 4l channel </b>

For this part the following CMSSW version was used: CMSSW_7_4_7

plotter.c -> The file reads 4l MC + data (from relevant repositories), togetherwith the data driven zx component, contained in the zx.root file. It performs relevant selection, mela cuts, kin_variable generation. The output is the 1D histogram portarying all contributions.

new_plotter.c -> Same functions as the previous file, with the exclusion of 1D histogram generation. In addition the file enables the user to generate the mass+kin_variable 2D histograms and to save them on a dedicated root file.

bkg_Workspace.c -> Generation of templates for signal and background (devided by component: vbs, qqZZ etc..). The templates are created at workspace level and saved accordingly.

The code is run with:

source runbkg.sh 

zx.c -> Reads the ZX.root file, which is the final step of the data-driven ZX monte carlo generation procedure. The output file is zx.root (taken in input by plotter.c and new_plotter.c as explained above).

<b> 2l2q channel </b>

For this part the following CMSSW version was used: CMSSW_8_0_26_patch1

plotMC.c -> This file is based on Roberto's original 1D template generator for the 2l2q channel. It performs the 2D mass +kin_variable histogram generation for the different components. This version is not automated, so for each contribution (for example, ttbar merged and so on) it is necessary to change the output file names). 

bkgWorkspace_2l2q.c -> Generation of templates for signal and background. The templates are created at workspace level and saved accordingly. The file creates the templates for both the merged and the resolved components. However, since COMBINE requires the two contributions to be saved on two different files, it is necessarry to comment the lines (at the end of the file) where the unwanted templates are imported on the workspace. As can be seen, in this file version this is done for the merged component.

The code is run with:

source runbkg_2l2q.sh 

alpha.C -> This file has the same structure of the plotMC.c file. It embeds the alpha method used for the the DY component treatment, by operations with 1D histograms. The default output is the whole set of possible 1D plots (all possible contributions and variables). 

<b> Combine </b>

All cards can be found in the COMBINATION_FOLDER, with sub-directories for single channels and combination of channels. I suggest a double check on the systematics and whether all channels and processes are correctly included/positioned in the right column etc. The following are the Combine commands which I used:


To combine multiple cards: 

combineCards.py Name1=old_card1.txt Name2=old_card2.txt .... > new_card.txt

To run the likelihood analysis (expected significance, without systematics): 

combine -M ProfileLikelihood --significance card_name.txt -t -1 --expectSignal=1 -S 0 --toysFreq

To run the likelihood analysis (expected significance, with systematics): 

combine -M ProfileLikelihood --significance card_name.txt -t -1 --expectSignal=1 -S 1 --toysFreq

To run the likelihood analysis (observed significance): 

combine -M ProfileLikelihood --significance card_name.txt 


<b> Note </b>

These are the single files, and the most complete version I was able to find. However I've included .tar compressed file of the complete directories here: https://www.dropbox.com/sh/mahvupqcauchx53/AAAhKMKCSfl-Bhsw1uwJo5HQa?dl=0 (quite messy, but containing a long list of different file versions, data cards, plots and so on). It may be useful in case there is some file not included in the above list, but which is used somewhere in the code.

The Meng.txt contains the instructions that Meng gave me at our CERN meeting last Spring. 
