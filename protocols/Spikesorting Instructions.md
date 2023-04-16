(protocols-spikesorting)=
# Spikesorting Instructions

(protocols-spikesorting-run-kilosort)=
## Run Kilosort
Before you can do any manual spikesorting, you need to complete the automatic spikesorting part using Kilosort. Open Matlab and navigate to the folder containing raw extracellular electrophysiology data recorded using Neuropixels or any other probe. Run ```KilosortWrapper``` from that folder. You can, for example, use a similar script to the one shown below:
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

If you set ```createSubdirectory = true;```, Kilosort should produce a separate output folder inside the raw data folder. This is where you need to navigate using your terminal before launching Phy as explained in the next subsection.

(protocols-spikesorting-launch-phy)=
## Launch Phy
Open Power Shell and type in:
```
cd <kilosort-output-folder>
.<path-where-you-installed-phy-environment>\phy\Scripts\activate.ps1
phy template-gui params.py
```

A successful launch of Phy GUI indicates that your spikesorting environment is set up and ready for manual curation of the kilosort output.

(protocols-spikesorting-steps-phy)=
## Major Manual Spikesorting Steps Using Phy
- Step 1: [Configure Phy displays](protocols-spikesorting-phy-displays)
- Step 2:
  - [Mark noise clusters](protocols-spikesorting-noise)
  - [Mark 'good' clusters or units](protocols-spikesorting-units)
  - [Split clusters](protocols-spikesorting-split)
- Step 3: [Merge units](protocols-spikesorting-merge)
- Step 4: [Mark multiunit activity (MUAs)](protocols-spikesorting-muas)

Once you set up your Phy interface and are ready to start spikesorting, you would typically go through the spikesorting data twice in two major phases. During the first phase you would mark noise clusters, mark obvious 'good' clusters or units, and consider clusters for splitting. After splitting, some clusters could yield 'good' and noise subclusters which should also be marked. During the second phase you would go through your 'good' units comparing them to other 'good' units and unclassified clusters and consider whether they should be merged. Finally, you would mark the remaining unclassified clusters as MUAs.

```{danger} Do not lose your work!
Save your spikesorting progress periodically often. Phy may crash unexpectedly and all unsaved work would be lost. There is no autosave functionality in Phy. The shortcut for saving is ```Ctrl+s```.
```

(protocols-spikesorting-phy-displays)=
### Configure Phy Display
Configure your Phy display to include the below windows:
- ClusterView
- SimilarityView
- AmplitudeView
- CorrelogramView: Adjust window size to be 500 ms (250 ms when doing cluster comparisons).
- WaveformView: Make sure that at least 32 recording channels are displayed. Single waveform duration on a single channel is 83 data points or approximately 2.77 ms.
- FeatureView

One way you could organise them is shown in the figure below.
```{image} ../assets/images/protocols/spikesorting-instructions/full-phy-display.png
:name: fig-protocols-spikesorting-phy-display
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 1. Recommended Phy Display Arrangement**

(protocols-spikesorting-noise)=
### Mark Noise Clusters
You mark a cluster as noise by selecting it in the ClusterView and pressing ```Alt+n``` keyboard shortcut. Do not yet do anything about MUAs. You don't want to make judgement regarding this activity type because marking clusters as MUAs would exclude them from being considered for merging later.

As you go through the clusters one by one marking any noise clusters you encounter, you should also mark clusters that you think are 'good' units (```Alt+g``` keyboard shortcut). At this stage you should also mark any cluster that you are not sure of as a 'good' one too. The reason for doing that is explained in the [Merge Units subsection](protocols-spikesorting-merge). Low count clusters with uncontaminated refractory periods should also be marked as 'good' for now. The criteria for classifying clusters as 'good' units are discussed in [Mark Units subsection](protocols-spikesorting-merge).

Let's get back to noise cluster classification. Here are a few criteria for recognising putative noise clusers:
- Cluster autocorrelogram has a triangular shape and waveforms appear uniform across recording channels as in the example below.
```{image} ../assets/images/protocols/spikesorting-instructions/noise-example1.png
:name: fig-protocols-spikesorting-noise-example1
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 2. Noise Cluster Example #1**

- Waveforms appear on all recording channels.
```{image} ../assets/images/protocols/spikesorting-instructions/noise-example2.png
:name: fig-protocols-spikesorting-noise-example2
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 3. Noise Cluster Example #2**

- Waveform appearing on a single recording channel only.
```{image} ../assets/images/protocols/spikesorting-instructions/noise-example3.png
:name: fig-protocols-spikesorting-noise-example3
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 4. Noise Cluster Example #3**

- Waveform appearing on a single recording channel only and having a distinctly unusual non-spike-like shape.
```{image} ../assets/images/protocols/spikesorting-instructions/noise-example4.png
:name: fig-protocols-spikesorting-noise-example4
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 5. Noise Cluster Example #4**

- Unit spiking on channel 64 mixed with noise appearing on multiple channels. Splitting is not possible. Therefore, this cluster should be marked as noise.
```{image} ../assets/images/protocols/spikesorting-instructions/noise-example5.png
:name: fig-protocols-spikesorting-noise-example5
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 6. Noise Cluster Example #5**

- Activity apearing on all recording channels albeit with varying amplitude. Autocorrelogram shows strong periodicity.
```{image} ../assets/images/protocols/spikesorting-instructions/noise-example6.png
:name: fig-protocols-spikesorting-noise-example6
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 7. Noise Cluster Example #6**

- Activity waveform does not resemble a typical spike waveform.
```{image} ../assets/images/protocols/spikesorting-instructions/noise-example7.png
:name: fig-protocols-spikesorting-noise-example7
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 8. Noise Cluster Example #7**

(protocols-spikesorting-units)=
### Mark Units

(protocols-spikesorting-split)=
### Split Clusters

(protocols-spikesorting-merge)=
### Merge Units

(protocols-spikesorting-muas)=
### Mark Multiunit Activity