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
doc KilosortWrapper
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

A successful launch of Phy GUI indicates that your spikesorting environment is set up and ready for manual curation of the Kilosort output.

(protocols-spikesorting-steps-phy)=
## Major Manual Spikesorting Steps Using Phy
- Step 1: [Configure Phy displays](protocols-spikesorting-phy-displays)
- Step 2:
  - [Mark noise clusters](protocols-spikesorting-noise)
  - [Mark 'good' clusters or units](protocols-spikesorting-units)
  - [Mark 'unsure' clusters as 'good'](protocols-spikesorting-unsure)
- Step 3:
  - [Split clusters](protocols-spikesorting-split)
  - [Merge clusters](protocols-spikesorting-merge)
- Step 4: [Mark multiunit activity (MUAs)](protocols-spikesorting-muas)

Once you set up your Phy interface and are ready to start spikesorting, you would typically go through the spikesorting data twice in two major phases. During the first phase you would mark noise clusters, mark obvious 'good' clusters or units, and mark any clusters that you are not sure of as 'good' also. In the second sift though the data you should examine clusters marked as 'good' for splitting. You should also compare these clusters to other 'good' and 'unsorted' clusters in the SimilarityView window for potential mergers. Finally, you would mark the remaining unclassified clusters as MUAs.

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

As you go through the clusters one by one marking any noise clusters you encounter, you should also mark clusters that you think are 'good' units (```Alt+g``` keyboard shortcut). At this stage you should also mark any cluster that you are not sure of as a 'good' one too. The reason for doing that is explained in the [Merge Units](protocols-spikesorting-merge) subsection. Low count clusters with uncontaminated refractory periods should also be marked as 'good' for now. The criteria for classifying clusters as 'good' units are discussed in [Mark Units](protocols-spikesorting-merge) subsection.

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
When deciding between single unit ('good' unit; ```Alt+g```) and multiunit activity (```Alt+m```), look for a few features:
- The refractory period in the autocorrelogram should not be contaminated with spikes. This is the most important criterion. It is reasonable to expect some minor contamination, but the refractory period should be reasonably clean. It may be possible to clean up the refractory period by splitting the cluster. This is the reason why units with contaminated refractory periods should be marked as 'good' in Step 2. They should be re-examined in Step 3 and can either be cleaned up by splitting or, if could not be cleaned up, marked as unsorted (```Alt+u```).
- The refractory period should be demarcated by a smooth transition in the histogram bins from zero (or very low) count to high count. The appearance of a sharp cliff at the edge of the refractory period is indicative of multiunit activity. The cliff is indicative of the spike detection algorithm missing overlapping spikes that belong to the same cluster. These missing spikes create the appearance of a sharp refractory period as they suddenly go missing once two spikes appear too close to each other.
- The spike waveform should have a distinct shape characteristic of action potentials. It should be clean meaning that local field potential deflections should be aligned and clearly visible on the peak channel, multiple nearby waveforms should not be present on the peak channel, waveforms should not be contaminated by noise. This is the third key criterion.
- 'Good' units should have a relatively high firing rate. At minimum it should have a firing rate of 200 spikes per hour. The firing rate criterion over-rides all other criteria. If a unit has a low firing rate, its autocorrelogram is meaningless.
- Unit activity should appear clusterred in an eliptical manner in the principal component space shown in the FeatureView window. Violations of this requirement may indicate the presence of merged subclusters that should be split, if possible.
- Unit amplitudes should ideally remain stable throughout the recording session. This is not a strict requirement as bursting units are likely to have multiple amplitudes. Amplitude may also change over the period of the recording due to the probe drift. However, periods where amplitude strongly deviates from the median value should be inspected for noise.

(protocols-spikesorting-unsure)=
### Mark 'Unsure' Clusters
'Unsure' clusters are the ones that you want to come back to later to inspect them for splitting or merging with other clusters. They include:
- Low spike count clusters with uncontaminated autocorrelogram refractory periods (considered for merging).
- Clusters with distinct refractory periods with minor contamination (considered for splitting).
- Clusters with distinct refractory periods but having a large count zero lag histogram bin (considered for splitting).
- Any cluster you are not sure of and one to come back to at a later spikesorting stage.

(protocols-spikesorting-split)=
### Split Clusters
You should ideally be splitting clusters as part of the spikesorting Step 3. At this stage you will only be focusing on clusters that were manually marked as 'good' appearing green in the CluserView window (do not confuse with KSLabel 'good'). To display only 'good' clusters type ```group=="good"``` in the filter box located at the top of the CluserView window. This action should filter out any unsorted and 'noise' clusters from the CluserView window.

Go through each cluster that was marked as 'good' one by one and, if you think they are contaminated by noise or contain multiple units, try splitting them. Clusters that might be 'worthwhile' spliting and could potentially be salvaged typically come from the 'unsure' group. They may have a contaminated refractory period or a large bin count at zero lag while otherwise possess a clear refractory period. These are the splitting steps you should attempt on these clusters:
- Visualise spikes in the refractory period: go to ```Edit >> VisualizeShortISI``` in the Phy menu (or ```Alt+i``` keyboard shortcut). This action will split spikes with the short inter-spike interval (ISI) from the rest of the cluster. If the short ISI spikes contain only noise, they should be discarded as noise. If they contain units, they cannot be split apart and should be re-merged with the rest of the cluster they came from.
- Inspect the FeatureView window. If you see that the cluster does not have a spherical cloud shape, this may indicate that multiple clusters were merged together. Try to separate such a cluster by drawing lines around part of the cluster (```Ctrl+left-click```) and pressing ```k``` to split. If you think that the two resultant clusters do not belong together, keep them separated. Otherwise, re-merge them. On criteria for deciding when clusters should be merged or kept seprate, see the [Merge Clusters](protocols-spikesorting-merge) subsection.
- Inspect the AmplitudeView window for unusual groupings of spikes. Try to split those apart by drawing lines around these groupings (```Ctrl+left-click```) and pressing ```k``` to split. These clusters may contain noise or other clusters of spikes that should be kept separate. If the resultant clusters are similar, they should be re-merged back. On criteria for deciding when clusters should be merged or kept seprate, see the [Merge Clusters](protocols-spikesorting-merge) subsection.
- Try splitting off outlying large amplitude spikes clearly deviating away from the median amplitude in the AmplitudeView window. These large amplitude events may be noise.
- You can recluster a particular cluster of interest using ```Edit >> Recluster PCAs``` action (```Alt+K```). The resulting new clusters may be significantly different from each warranting being split into seprarate clusters. They may be different units or one of the culsters may contain noise. If the new clusers do not look significantly different, merge them back together.

Other times when you would perform cluster split are when you try to clean up 'good' clusters that pass as single unit activity. In this case:
- You can draw a close line around the cloud cluster in the FeatureView window and split away any outliers. These outliers may be noise or spikes from other clusters that were merged by mistake. Discard them as noise or multiunit activity accordingly. However, you must re-merge them if these split-off spikes are similar to the spikes in the main cluster.
- You can further clean up by isolating outlying large amplitude events in the AmplitudeView window like you do when you try to split 'unsure' clusters.
- Reclustering is useful when you have a unit that has contaminated refractory period and you want to clean it up. Use the action ```Edit >> Recluster PCAs``` (```Alt+K```). The resulting clusters may be different units or one of the culsters may contain noise. If the new clusers do not look significantly different, merge them back together.

(protocols-spikesorting-merge)=
### Merge Clusters
You should ideally be merging clusters as part of the spikesorting Step 3. At this stage you will only be focusing on clusters that were manually marked as 'good' appearing green in the CluserView window (do not confuse with KSLabel 'good'). To display only 'good' clusters type ```group=="good"``` in the filter box located at the top of the ClusterView window. This action should filter out any unsorted and 'noise' clusters from the CluserView window.

Go through each cluster that was marked as 'good' one by one. Press ```spacebar``` to select the next most similar cluster in the ```SimilarityView```. Only 'good' or 'unsorted' clusters can be selected. This is the reason why you should not mark clusters as multiunit activity until the very end of the spikesorting process. Below are the criteria to consider when merging clusters:
- The spike waveforms of the clusters being compared should be very similar and appear on the same recording channels.
- The cross-correlograms should have clean refractory periods. However, if the spike waveforms are identical but the cross-correlogram refractory periods are still contaminated these clusters might be merged. Then they should be attempted to be split based on the criteria outlined in [Split Clusters](protocols-spikesorting-split) subsection or should be kept merged and the resultant cluster re-classified as 'unsorted'.
- The two clusters should overlap or appear adjacent to each other in the principal component space of the FeatureView window. The merged cluster should then form a single cloud cluster in the principal component space.
- Amplitudes of the two clusters should overlap or form continuous grouping over time in the AmplitudeView window. This is not a strict requirement as amplitudes of the two clusters may differ due to a drift or burst firing.

You merge clusters by pressing ```g```. If the newly merged cluster can no longer be merged with other clusters and still has a low spike count, it should be marked as 'unsorted'.

(protocols-spikesorting-muas)=
### Mark Multiunit Activity
You should do this step at the very end once you went though your data twice. Essentially, everything that is not noise or a 'good' unit is a multiunit activity. This would include:
- Clusters with contaminated or no refractory periods in the autocorrelograms. If you failed to clean up a 'dirty' unit, it should belong here.
- Clusters with refractory periods with non-smooth transitions between zero (or low count) and high count histogram bins.
- Clusters with waveforms that have multiple spikes in them.
- Units with low firing rates.
- If you have a hard time making a decision about a particular cluster, it most likely belongs here too.