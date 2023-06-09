(doc-spikesorting-env)=
# Setup Spikesorting Environment
Instructions for setting up spikesorting environment on your Windows computer.

(doc-spikesorting-env-prerequisites)=
## Prerequisites
To spikesort, you have to have [Matlab](https://se.mathworks.com/products/matlab.html), [Git](https://git-scm.com/), [CellExplorer](https://cellexplorer.org/), [npy-matlab](https://github.com/kwikteam/npy-matlab), [Kilosort3](https://github.com/MouseLand/Kilosort), [KilosortWrapper](https://github.com/dervinism/KilosortWrapper), and [Phy](https://github.com/cortex-lab/phy) installed on your system. If you are using Windows, it is advisable to install the [Windows Terminal](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701) and the [Windows Subsystem for Linux](https://ubuntu.com/wsl) (WSL).

(doc-spikesorting-env-install-matlab)=
### Matlab Installation
Matlab is a proprietary software and the University of Copenhagen has acquired the license for the software. Matlab can be installed by following instructions provided [here](https://se.mathworks.com/academia/tah-portal/university-of-copenhagen-30348482.html). Make sure that you also install Matlab with all of its toolboxes.

(doc-spikesorting-env-install-git)=
### Git Installation
Git installation instructions are provided [here](https://git-scm.com/downloads).

(doc-spikesorting-env-install-cellexplorer)=
### CellExplorer Installation
Download CellExplorer Github [repository](https://github.com/petersenpeter/CellExplorer) and add it to your Matlab path (make sure you add it with subfolders). If you are using Linux, an extra step is to go to ```CellExplorer/calc_CellMetrics/mex/``` folder while in Matlab and type:
```
mex -O CCGHeart.c
mex -O FindInInterval.c
```

(doc-spikesorting-env-install-npy-matlab)=
### npy-matlab Installation
The purpose of the npy-matlab library and its installation instructions can be found on its Github repository [page](https://github.com/kwikteam/npy-matlab). You simply need to download this repository and add it to your Matlab path.

(doc-spikesorting-env-install-kilosort)=
### Kilosort Installation
#### Kilosort 3
Kilosort 3 installation is more involved and is outlined on the Kilosort's Github repository [page](https://github.com/MouseLand/Kilosort). Don't forget to add the main Kilosort folder with its subfolders to the Matlab path.

If you are using Windows, you will also need to install Visual Studio Community 2019 and make it the Matlab's default C++ compiler. The quickest way to install Visual Studio Community 2019 is via the Microsoft Store. Open Microsoft Store, search for Visual Studio Community 2019, and click Install. Once you've done that, click on Modify and click the option Desktop development with C++ inside the Workloads pane, and then click Modify again. This should install the extra needed components.

Open Matlab and in the console type:
```matlab
mex -setup cpp
```

Finally, open ```./preProcess/datashift2.m``` file inside the Kilosort source code folder and replace line 54:
```matlab
spkTh = 10; % same as the usual "template amplitude", but for the generic templates
```
with the following line:
```matlab
spkTh = ops.spkTh; % same as the usual "template amplitude", but for the generic templates
```

#### Kilosort 2.5
Kilosort 2.5 installation is analogous to Kilosort 3 installation. Download compressed source code for Kilosort 2.5 from [here](https://github.com/MouseLand/Kilosort/releases/tag/v2.5), extract it, and place the source code folder into your Matlab libraries folder. Add the folder with its subfolder to the Matlab path. Finally, open CUDA folder in Matlab and execute the following line of code in the Matlab console:
```matlab
mexGPUall
```

As the last step, open ```./preProcess/datashift2.m``` file inside the Kilosort source code folder and replace line 27:
```matlab
spkTh = 10; % same as the usual "template amplitude", but for the generic templates
```
with the following line:
```matlab
spkTh = ops.spkTh; % same as the usual "template amplitude", but for the generic templates
```

(doc-spikesorting-env-install-kswrapper)=
### KilosortWrapper Installation
To install the KilosortWrapper, follow the instructions outlined in its Github repository [page](https://github.com/dervinism/KilosortWrapper). Download the repository and add it to your Matlab path. Also add subdirectories ```chanMaps``` and ```ConfigurationFiles``` to your Matlab path. One extra step to follow, inside Matlab navigate to the ```private``` folder of the wrapper and type:
```
mex -O CCGHeart.c
```

Once installed, you need to edit ```NchanNear``` parameter inside the Kilosort source code in order to increase the number of recording channels available in the WaveformView window of the Phy GUI. To do so, open files ```\clustering\final_clustering.m```, ```\clustering\template_learning_part2.m```, and ```\mainLoop\trackAndSort.m```. In all these files replace the line
```
NchanNear   = min(ops.Nchan, 16);
```
with the line
```
NchanNear   = min(ops.Nchan, 32);
```
Save and close the files.

(doc-spikesorting-env-install-phy)=
### Phy Installation
At this stage you will have to use the terminal of your operating system. Below is the description of some notation that can be used here and that you may not be familiar with:
- []: Square brackets mean an optional argument (inclusive of brackets)
- <>: Angle brackets mean that the text within should be replaced with an appropriate argument (inclusive of brackets).
- Both types of brackets can be combined.
- The use of dashes in command argument names indicates that the argument name does not allow for blank spaces.
- Path arguments most often do not contain spaces. If your path does contain spaces, enclose your path argument within single or double quotations. For example, use either ```some-command path/to/my/repository``` or ```some-command "path/to/my repository"```.
- $WINDOWS-STATE-VARIABLE: State variables in Windows are denoted using the dollar sign followed by the name of the variable in capital letters.

Phy is a Python library and we are going to install it in its dedicated environment. Assuming you are using Windows, execute all subsequent commands in the PowerShell. First, install Python by typing in the terminal:
```
python
```
Then proceed with setting up Phy environment
```
python -m pip install --user virtualenv
pip install virtualenv
cd <path-to-where-you-will-install-phy-environment>
python -m venv phy
```
The next step is to install Phy. You will need to download phy and phylib Github repositories. Make sure that you keep your phy environment and phy code folders separate. In your terminal type:
```
cd <path-to-your-folder-where-you-want-to-download-phy>
git clone https://github.com/cortex-lab/phy
cd phy
```
Now edit the ```requirements.txt``` file as it is not up-to-date with the latest Phy code. In your terminal type:
```
notepad requirements.txt
```
Replace the line that says ```numpy``` to ```numpy==1.23.5```. Once the ```requirements.txt``` file is updated, continue with the rest of the installation and type:
```
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
.<path-to-where-you-will-install-phy-environment>\phy\Scripts\activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
pip install -r requirements-dev.txt
pip install -e .
cd ..
git clone https://github.com/cortex-lab/phylib
cd phylib
pip install -e . --upgrade
```
Now, navigate to the folder ```<path-to-where-you-downloaded-phy>\phylib\phylib\io``` and open the file ```model.py```. Replace the line 305 where it says
```
n_closest_channels = 12
```
with the line
```
n_closest_channels = 32
```
Save and close the file.

After editing the file as described, add [Phy plugins](https://github.com/dervinism/phy-plugins). The plugin files and installation instructions are provided in the [Phy plugins](https://github.com/dervinism/phy-plugins) repository README file.

Now that Phy has been installed, navigate to the folder with the Kilosort output (if such folder exists; otherwise, skip to [the next section](doc-spikesorting-env-run-kilosort)) and, while phy environment is still open (activated with command ```.<path-to-where-you-will-install-phy-environment>\phy\Scripts\activate.ps1```), in your terminal type:
```
phy template-gui params.py
```
When finished working with Phy, to close Phy environment, type:
```
deactivate
```

(doc-spikesorting-env-run-kilosort)=
## Run Kilosort
Open Matlab and navigate to the folder containing raw extracellular electrophysiology data recorded using Neuropixels or any other probe. Run ```KilosortWrapper``` from that folder. You can, for example, use a similar script to the one shown below:
```matlab
algorithm='ks3';
driftCorrection = true;
basepath = cd;
[~, basename] = fileparts(basepath);
GPU_id = 1;
procPath = '';
createSubdirectory = true;
performAutoCluster = true;
config = '';
phyPath = 'C:\Users\Martynas\Python_environments\phy';
acqBoard = 'OpenEphys';
probe = 'Neuropixels1_checkerboard';
savepath = KiloSortWrapper(basepath=basepath, basename=basename, ...
  GPU_id=GPU_id, procPath=procPath, createSubdirectory=createSubdirectory, ...
  performAutoCluster=performAutoCluster, config=config, phyPath=phyPath, ...
  acqBoard=acqBoard, probe=probe);
```
For more details on how to run ```KilosortWrapper``` type in Matlab console one of the following:
```matlab
help KilosortWrapper
dic KilosortWrapper
```

If Matlab completes the execution of ```KilosortWrapper``` without errors, it means that Kilosort is properly set up on your system.

(doc-spikesorting-env-launch-phy)=
## Launch Phy
Open Power Shell and type in:
```
cd <kilosort-output-folder>
.<path-where-you-installed-phy-environment>\phy\Scripts\activate.ps1
phy template-gui params.py
```

A successful launch of Phy GUI indicates that your spikesorting environment is set up and ready for manual curation of the kilosort output.