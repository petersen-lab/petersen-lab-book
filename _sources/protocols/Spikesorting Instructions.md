(protocols-spikesorting)=
# Spikesorting Instructions

(protocols-spikesorting-steps)=
## Major Spikesorting Steps
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
- CorrelogramView
- WaveformView
- FeatureView
- AmplitudeView

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

Here are a few criteria for recognising putative noise clusers:
- Cluster autocorrelogram has a triangular shape and waveforms appear uniform across recording channels as in the example below.
```{image} ../assets/images/protocols/spikesorting-instructions/noise-example1.png
:name: fig-protocols-spikesorting-noise-example1
:width: 690px
:align: center
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Figure 2. Noise Cluster Example #1**

(protocols-spikesorting-units)=
### Mark Units

(protocols-spikesorting-split)=
### Split Clusters

(protocols-spikesorting-merge)=
### Merge Units

(protocols-spikesorting-muas)=
### Mark Multiunit Activity