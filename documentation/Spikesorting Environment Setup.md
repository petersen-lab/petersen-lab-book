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
Kilosort3 installation is more involved and is outlined on the Kilosort's Github repository [page](https://github.com/MouseLand/Kilosort). Don't forget to add the main Kilosort folder with its subfolders to the Matlab path.

If you are using Windows, you will also need to install Visual Studio Community 2019 and make it the Matlab's default C++ compiler. The quickest way to install Visual Studio Community 2019 is via the Microsoft Store. Open Microsoft Store, search for Visual Studio Community 2019, and click Install. Once you've done that, click on Modify and click the option Desktop development with C++ inside the Workloads pane, and then click Modify again. This should install the extra needed components.

Open Matlab and in the console type:
```matlab
mex -setup cpp
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

After editing the file as described, copy the file ```<path-to-where-you-downloaded-phy>\phy\plugins\n_spikes_views.py``` to the folder ```C:\Users\<your-user>\.phy\plugins```. Open the newly copied file and edit a few lines. Replace line 9 with
```
controller.n_spikes_waveforms = 500
```
and replace line 17 with this line:
```
controller.model.n_closest_channels = 32
```

For the next step, create a file called ```SplitShortISI.py``` inside the folder ```C:\Users\<your-user>\.phy\plugins```. Then open it and paste in the following content:
```python
"""Show how to write a custom split action."""

from phy import IPlugin, connect
import numpy as np
import logging

logger = logging.getLogger('phy')



class SplitShortISI(IPlugin):
    def attach_to_controller(self, controller):
        @connect
        def on_gui_ready(sender, gui):
            #@gui.edit_actions.add(shortcut='alt+i')
            @controller.supervisor.actions.add(shortcut='alt+i')
            def VisualizeShortISI():
                """Split all spikes with an interspike interval of less than 1.5 ms into a separate
                cluster. THIS IS FOR VISUALIZATION ONLY, it will show you where potential noise
                spikes may be located. Re-merge the clusters again afterwards and cut the cluster with
                another method!"""
                
                logger.info('Detecting spikes with ISI less than 1.5 ms')

                # Selected clusters across the cluster view and similarity view.
                cluster_ids = controller.supervisor.selected

                # Get the amplitudes, using the same controller method as what the amplitude view
                # is using.
                # Note that we need load_all=True to load all spikes from the selected clusters,
                # instead of just the selection of them chosen for display.
                bunchs = controller._amplitude_getter(cluster_ids, name='template', load_all=True)

                # We get the spike ids and the corresponding spike template amplitudes.
                # NOTE: in this example, we only consider the first selected cluster.
                spike_ids = bunchs[0].spike_ids
                spike_times = controller.model.spike_times[spike_ids]
                dspike_times = np.diff(spike_times)

                labels = np.ones(len(dspike_times),'int64')
                labels[dspike_times<.0015]=2
                labels = np.append(labels,1) #include last spike to match with len spike_ids

                # We perform the clustering algorithm, which returns an integer for each
                # subcluster.
                #labels = k_means(y.reshape((-1, 1)))
                assert spike_ids.shape == labels.shape

                # We split according to the labels.
                controller.supervisor.actions.split(spike_ids, labels)
                logger.info('Splitted short ISI spikes from main cluster')
```
Save and close the file.

Also, create a file called ```CustomActionPlugin.py``` inside the folder ```C:\Users\<your-user>\.phy\plugins```. Then open it and paste in the following content:
```python
# import from plugins/action_status_bar.py
"""Show how to create new actions in the GUI.

The first action just displays a message in the status bar.

The second action selects the first N clusters, where N is a parameter that is entered by
the user in a prompt dialog.

"""

from phy import IPlugin, connect
import numpy as np
import logging

logger = logging.getLogger('phy')

class CustomActionPlugin(IPlugin):
    def attach_to_controller(self, controller):
        @connect
        def on_gui_ready(sender, gui):

            @controller.supervisor.actions.add(shortcut='ctrl+c')
            def select_first_unsorted():

                # All cluster view methods are called with a callback function because of the
                # asynchronous nature of Python-Javascript interactions in Qt5.
                @controller.supervisor.cluster_view.get_ids
                def find_unsorted(cluster_ids):
                    """This function is called when the ordered list of cluster ids is returned
                    by the Javascript view."""
                    groups = controller.supervisor.cluster_meta.get('group',list(range(max(cluster_ids))))
                    for ii in cluster_ids:
                        if groups[ii] == None or groups[ii] == 'unsorted':
                            s = controller.supervisor.clustering.spikes_in_clusters([ii])
                            if len(s)>0:
                                firstclu = ii
                                break
                    
                    if 'firstclu' in locals():
                        controller.supervisor.select(firstclu)

                    return


            @controller.supervisor.actions.add(shortcut='ctrl+v')
            def move_selected_to_end():

                logger.warn("Moving selected cluster to end")
                selected = controller.supervisor.selected
                s = controller.supervisor.clustering.spikes_in_clusters(selected)
                outliers2 = np.ones(len(s),dtype=int)
                controller.supervisor.actions.split(s,outliers2)

            
            @controller.supervisor.actions.add(shortcut='ctrl+b')
            def move_similar_to_end():

                logger.warn("Moving selected similar cluster to end")
                sim = controller.supervisor.selected_similar
                s = controller.supervisor.clustering.spikes_in_clusters(sim)
                outliers2 = np.ones(len(s),dtype=int)
                controller.supervisor.actions.split(s,outliers2)
```
Save and close the file.

Finaly, open the file ```C:\Users\<your-user>\.phy\phy_config.py``` and replace the last line in the file with the following line:
```
c.TemplateGUI.plugins = ['n_spikes_views','SplitShortISI']  # list of plugin names to load in the TemplateGUI
```

Now that Phy has been installed, navigate to the folder with the Kilosort output and, while phy environment is still open (activated with command ```.<path-to-where-you-will-install-phy-environment>\phy\Scripts\activate.ps1```), in your terminal type:
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
basepath = cd;
[~, basename] = fileparts(basepath);
GPU_id = 1;
procPath = '';
createSubdirectory = true;
performAutoCluster = true;
config = '';
phyPath = 'C:\Users\<your-user-name>\Python_environments\phy';
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