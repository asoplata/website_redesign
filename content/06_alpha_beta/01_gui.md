<!--
# Title: 4.1 GUI Tutorial of Alpha/Beta Rhythms
# Updated: 2025-02-04
#
# Contributors:
# Nick Tolley
# Dylan Daniels
-->

<!-- this markdown file and images originally adapted from https://github.com/jonescompneurolab/hnn-tutorials/tree/core-gui/coregui_alpha_and_beta -->

# 4.1 GUI Tutorial of Alpha/Beta Rhythms

## Tutorial Table of Contents

1. [Background](#toc-1)

2. [Downloading HNN Parameter Set Files](#toc-2)

3. [Simulating Rhythmic Proximal Inputs: Alpha only](#toc-3)

4. [Simulation Rhythmic Distal Inputs: Alpha only](#toc-4)

5. [Simulating Combined Rhythmic Proximal and Distal Inputs: Alpha/Beta Complex](#toc-5)

6. [Calculating and Viewing Power Spectral Density (PSD)](#toc-6)

<!-- Currently, commenting out section 7, see note near it for details. -->
<!-- 7. [Comparing model output and recorded data](#toc-7) -->

<!-- 8. [Adjusting parameters](#toc-8) -->
7. [Adjusting parameters](#toc-8)

<!-- 9. [Have fun exploring your own data!](#toc-9) -->
8. [Have fun exploring your own data!](#toc-9)

<a id="toc-1"></a>

TODO beta "event-like"

TODO time for colab single sim

## 1. Background

In order to understand the workflow and initial parameter sets provided with this tutorial, we must first briefly describe prior studies that led to the creation of the data you will aim to simulate. This tutorial is based on results from [@jones_quantitative_2009] where, using MEG, we recorded spontaneous (pre-stimulus) alpha (7-14 Hz) and beta (15-29 Hz) rhythms that arise as part of the mu-complex from the primary somatosensory cortex (S1) [@jones_quantitative_2009]. ([Figure 1](#figure-1), See also [@ziegler_transformations_2010], [@sherman_neural_2016], [@jones_when_2016].)

<div class="stylefig">
### Figure 1
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/image03.png)
<p align="justify">
**Figure 1 Left:** Spectrogram of spontaneous activity from current dipole source in SI averaged across 100 trials from an example subject. The spectrogram shows nearly continuous prestimulus alpha and beta oscillations. At time zero, a brief tap was given to the contralateral finger tip, causing the spontaneous oscillations to briefly desynchronize.
</p>
<p align="justify">
**Figure 1 Right:** A closer look at the prestimulus waveform and spectrogram from spontaneous activity during individual example signal trials. This illustrates that the alpha and beta oscillations occur intermittently and are frequently non-overlapping.
</p>
<p align="justify">
All figures in Figure 1 are from [@jones_quantitative_2009] or related work.
</p>
</div>

Our goal was to use our neocortical model to reproduce features of the waveform and spectrogram observed on single (un-averaged) trials ([Figure 1](#figure-1), middle and right columns), where the alpha and beta components emerge briefly and intermittently in time. On any individual trial (i.e., 1 second of spontaneous, pre-stimulus data), the presence of alpha and beta activity is not time-locked and is representative of so-called "induced" activity. The alpha and beta bands of activity appear continuous when averaging the spectrograms across trials ([Figure 1](#figure-1), left column), but this is due to the fact that the spectrograms values are strictly positive and the alpha and beta events accumulate without cancellation [@jones_when_2016]. For individual trials, alpha and beta power is simultaneously high only around 50% of the time, as shown in [Figure 2](#figure-2).

<div class="stylefig">
### Figure 2
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/old-image29.png)
<p align="justify">
**Figure 2**: Key features of the spontaneous non-average SI alpha/beta complex include: intermittent transient bouts of alpha/beta activity, a waveform that oscillates around 0 nAm, power spectral densities (PSD) with peaks in the alpha and beta bands, primarily non-overlapping alpha and beta events, and a symmetric waveform oscillation. The model was able to reproduce each of these features. The subplots in the top row are from experimental MEG data exhibiting these features, while the corresponding subplots in the bottom row are from simulations of the model. See [@jones_quantitative_2009].
</p>
</div>

We found that a sequence of exogenous subthreshold excitatory synaptic drive could activate the network in a manner that reproduced important features of the SI rhythms in the model ([Figure 2](#figure-2)). This drive consisted of two nearly-synchronous 10 Hz rhythmic drives that contacted the network through proximal and distal projection pathways ([Figure 3](#figure-3), see also [Textbook Section 2.5: Evoked and Rhythmic Driving Inputs](03_model_assumptions/evoked_and_rhythmic_driving_inputs.html)). The drives were simulated as population "bursts" of action potentials that contacted the network every 100ms with the mean delay between the proximal and distal burst of 0ms. Specifically, as shown schematically in [Figure 3](#figure-3), these 10 population bursts consisted of 2-spike bursts (i.e. spike "doublets"), Gaussian distributed in time. We presumed that during such spontaneous activity, these drives may be provided by leminscial and non-lemniscal thalamic nuclei, which contact proximal and distal pyramidal neurons respectively, and they are know to burst fire at ~10 Hz frequencies in spontaneous states ([@jones_thalamic_2001], [@hughes_thalamic_2005]).

<div class="stylefig">
### Figure 3
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/image04.png)
<p align="justify">
**Figure 3**: Schematic illustration of exogenous 10 Hz burst drives through proximal and distal projection pathways. "Population bursts", consisting of a set number of "burst units" (10 instances of 2-spike bursts as shown) drive post-synaptic conductances in the local network with a set frequency (100 ms inter-burst-interval, equal to 10 Hz) and a variable mean delay between proximal and distal drives.
</p>
</div>

We assumed that the macroscale rhythms generating the observed alpha and beta activity arose from subthreshold current flow in a large population of neurons, as opposed to being generated by local spiking interaction [@zhu_relationship_2009]. As such, the effective strengths of the exogenous driving inputs were tuned so that the cells in the network remained subthreshold (all other parameters were tuned and fixed based on the morphology, physiology, and connectivity within layered neocortical circuits, see [@jones_quantitative_2009] for details). The inputs drove subthreshold currents up (proximal) and down (distal) the pyramidal neurons in order to reproduce accurate waveform and spectrogram features (see [Figure 3](#figure-3)). A scaling factor of 3000 was multiplied by the model waveform to reproduce a signal in units of nAm, comparable to the recorded data, suggesting that on the order of 200 x 3000 = 600,000 pyramidal neurons contributed to this signal.

We further found that increasing the strength and synchrony of the distal drive created stronger beta activity, but increasing the delay between the drives to ~50ms created a pure alpha oscillation (see [Section 7](#toc-8) below). The former result led to the novel prediction that brief beta events emerge when a broad proximal drive is disrupted by a simultaneous strong distal drive lasting 50ms (i.e., one beta period). Support for this prediction was found invasively with laminar recordings in mice and monkeys [@sherman_neural_2016].

In this tutorial, we will explore parameter changes that illustrate these results. We will walk you step-by-step through simulations with various combinations of rhythmic proximal and distal drives to describe how each contributes to the alpha and beta components of the SI alpha/beta complex. We will **not** be simulating evoked responses or how alpha/beta oscillations interact with evoked responses; [click here for our GUI tutorial on simulating evoked-response potentials (ERPs)](../05_erps/erps_in_gui.html).

We will begin by simulating only rhythmic proximal 10 Hz inputs ([Section 3](#toc-3)), followed by simulating only distal 10 Hz inputs ([Section 4](#toc-4)), followed by various combinations of proximal and distal drives to generate combinations of alpha and beta rhythms ([Section 5](#toc-5)). We’ll show you how HNN can plot waveforms, time-frequency spectrograms, and power spectral density plots of the simulated data.

<a id="toc-2"></a>

## 2. Downloading HNN Parameter Set Files

Throughout this tutorial, we will be using several different HNN parameter set files. These files are not included in the HNN installation, but instead must be downloaded separately. The easiest way to get them is to download this Github repository: [hnn-data](https://github.com/jonescompneurolab/hnn-data). You can download the files in that repository by clicking on the green "Code" button, then clicking "Download ZIP". Once you have extracted the folder from the ZIP file, all the files you will need for this tutorial will be in the subfolder `network-configurations`.

TODO: add Dylan's "desktop-download" link thing here

Alternatively, if you only want to download each individual HNN parameter set file individually, then right click on each of the following links and select "Save Link As" to download them:

- [OnlyRhythmicProx.json](https://raw.githubusercontent.com/jonescompneurolab/hnn-data/refs/heads/main/network-configurations/OnlyRhythmicProx.json)
- [OnlyRhythmicDist.json](https://raw.githubusercontent.com/jonescompneurolab/hnn-data/refs/heads/main/network-configurations/OnlyRhythmicDist.json)
- [AlphaAndBeta.json](https://raw.githubusercontent.com/jonescompneurolab/hnn-data/refs/heads/main/network-configurations/AlphaAndBeta.json)
- [AlphaAndBetaJitter0.json](https://raw.githubusercontent.com/jonescompneurolab/hnn-data/refs/heads/main/network-configurations/AlphaAndBetaJitter0.json)

<a id="toc-3"></a>

## 3. Simulating Rhythmic Proximal Inputs: Alpha Only

Begin by starting the HNN GUI. If you are using Google Colab, follow the instructions in the notebook. If you are using a local install of HNN, run the following command from a terminal:

```
hnn-gui
```

### 3.1 Setup General Simulation and Visualization Parameters

Note that before running any simulations, we need to change some default parameters of both the simulations and visualizations. The parameters we need to change in the `Simulation` tab, are shown in [Figure 4](#figure-4) below, and are also listed below. Please note that if you refresh the browser tab or restart HNN, you will have to re-enter all of these parameter changes.

1. Change the `Name` of the simulation to `OnlyRhythmicProx` (or you can change it to whatever you prefer).
2. Change `tstop (ms)` to `700`. This will increase the length of the simulation to 700 milliseconds, and enable us to see simulated oscillations as they evolve over longer time periods.
3. Change `Smoothing` to `0`. Without doing this, much of alpha- and beta-frequency content of our signal will be "smoothed out" and not viewable.
4. Change `Min Spectral Frequency (Hz)` to `5`.
5. Change `Max Spectral Frequency (Hz)` to `40`. These two changes will make it easier to see the alpha and beta frequency ranges.
6. (Optional, but recommended) If you are using Mac or Linux (not Windows), and you installed HNN locally using the commands provided in the INSTALL PAGE (TODO), then run the following command from a Terminal, and change the "Cores" value to that number. This will enable very significant speedup for your simulations.

```
python -c "import psutil ; print(psutil.cpu_count(logical=False)-1)"
```

<div class="stylefig">
### Figure 4
<!-- ![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/core-gui-alpha-initial-setup.png) -->
TODO: Add arrow to Cores
![](/Users/austinsoplata/rep/brn/jones-website-upstream/images/textbook/content/06_alpha_beta/images/core-gui-alpha-initial-setup.png)
</div>

### 3.2 Load/view parameters to define the network structure & to "activate" the network.

As described in the [Background section](#background), low-frequency alpha and beta rhythms can be simulated by a combination of rhythmic subthreshold proximal and distal ~10Hz inputs. Here, we begin by describing the impact of proximal inputs only. An initial parameter set that will simulate the effect of ~10 Hz subthreshold proximal drive is provided in the file
[OnlyRhythmicProx.json](https://raw.githubusercontent.com/jonescompneurolab/hnn-data/refs/heads/main/network-configurations/OnlyRhythmicProx.json).

The template cortical column networks structure for this simulation is described in the [Template Model section](../01_getting_started/template_model.html). Several of the network parameters can be adjusted via the GUI (e.g. local excitatory and inhibitory connection strengths), but we will leave them fixed for this tutorial and only adjust the inputs the "activate" the network.

To load the initial parameter set, navigate to the GUI and click the tab labeled:

```
External drives
```

This tab is highlighted in [Figure 4](#figure-4) below.

<div class="stylefig">
<table>
### Figure 4
<tr>
<td>
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/image05.png)
</td>
<td>
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/image06.png)
</td>
<td>
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/image11.png)
</td>
</tr>
</table>
</div>

Then inside of the inside of the tab, click the button

```
Load external drives (0)
```

And select the file [OnlyRhythmicProx.json](https://raw.githubusercontent.com/jonescompneurolab/hnn-data/refs/heads/main/network-configurations/OnlyRhythmicProx.json) which you downloaded previously in [Section 2](#toc-2).

To view the parameters that "activate" the network via rhythmic proximal input, click the dropdown menu labeled:

```
bursty1 (proximal)
```

You should now be able to scroll and see the values of adjustable parameters, displayed as in the dialog boxes below in [Figure 5](#figure-5). There are 4 sections, which we will describe in greater detail below. For now, we will not change any of these parameters, but instead only describe them.

<div class="stylefig">
<table>
### Figure 5
<tr>
<td>
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/image08.png)
</td>
<td>
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/image09.png)
</td>
<td>
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/image10.png)
</td>
</tr>
</table>
</div>


1. General: The first section deals with all properties of the drive not included in other sections, including the timing settings, statistical properties, number of spikes per burst, number of virtual "drive cells", and pseudo-random number generation seed. The rhythmic proximal inputs drive excitatory synapses in the neocortical network in a proximal projection pattern, as shown in descriptions of the model elsewhere in the text, such as [Model Assumptions: 2.5 Evoked and Rhythmic Driving Inputs](../03_model_assumptions/evoked_and_rhythmic_driving_inputs.html). For further details on the connectivity structure of the network, see the Under the 

TODO 
Hoodsection of the HNN website. Rhythmic proximal input occurs through stochastic, presynaptic bursts of action potentials from a population of bursting cells (set with "Number bursts"; see [Figure 3](#figure-3)) onto postsynaptic neurons of the modelled network. Stochasticity is introduced in two places: the spike train start time for each bursting cell is sampled from a normal distribution with mean "Start time mean (ms)" and standard deviation "Start time stdev (ms)" and the inter-burst intervals for each bursting cell are sampled from a normal distribution of mean $\frac{1}{Burst frequency}$ (e.g., a 100 ms inter-burst interval corresponds to a "Burst frequency" of 10 Hz) and standard deviation "Burst stdev (ms)" (see [Figure 3](#figure-3)). Also note that the number of spikes per burst unit is set with "Spikes/burst" (currently, only values of 1 and 2 with a fixed 10ms delay can be used) and the final stop time for the entire population of rhythmic proximal inputs is set with "Stop time (ms)".


2. **AMPA weights**: One regulating the AMPA post-synaptic conductances onto the Layer 2/3 and Layer 5 neurons,
3. **NMDA weights**: which is analagous to the **AMPA weights** section, but for the NMDA synapses.
4. **Synaptic delays**:One regulating the synaptic delays.


We shall describe adjustable parameters in each GUI section separately.

[Model Assumptions: 2.5 Evoked and Rhytymic Driving Inputs](../03_model_assumptions/evoked_and_rhythmic_driving_inputs.html)

: 
Layer 2/3, and Layer 5 tabs: This dialog box allows you to set the postsynaptic conductance of each of the excitatory synapses in the networks. There are AMPA and NMDA receptors on each cell type (pyramidal and basket cells). 

There is also a delay parameter to control the arrival time of each spike to the network. In this example, the delay to the layer 2/3 cells is 0.1 ms, with a slightly longer delay to the layer 5 cells of 1 ms. For further details on the connectivity structure of the network, see Under the Hood.

<a id="toc-3-3"></a>

### 3.3 Run the simulation and visualize net current dipole

To run this simulation, navigate to the main GUI window and click:
```
Start Simulation
```
This simulation runs for 700 ms of simulation time, so will take a little longer to run than the ERP simulations. Once completed, you will see output similar to that shown below.

<div class="stylefig">
### Figure 6
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/OnlyRhythmicProx.png)
</div>

As shown in the red histogram in the top panel of [Figure 6](#figure-6) above, with this parameter set, a burst of proximal input spikes is provided to the network at a rate of ~10 Hz (i.e., every 100 ms). Due to the stochastic nature of the inputs (controlled by the `start time stdev` and `Burst stdev` parameters), there is some variability in the histogram of proximal input times. Note that a decrease in the `Burst stdev` would create shorter duration bursts (i.e., more synchronous bursts); this will be explored further in [Section 5.2](#toc-5-2) below.

TODO: the variability of the histogram *between computers* is not due to the statistics, but instead due to the same seed producing different PRNG results on different computer types. Emphasize that the same seed on the same computer will always produce the same input spike pattern.

The ~10 Hz bursts of proximal drive induces current flow **up** the pyramidal neuron dendrites, increasing the signal above the 0 nAm baseline, which then relaxes back to zero approximately every 100 ms. This is observed in the blue current dipole waveform in the GUI window.

To view the time-frequency spectrogram for this waveform (example in [Figure 7](#figure-7)), click on the `Visualization` tab. Then click on the `Layout template` dropdown menu and select `Drive-Dipole-Spectrogram`. Finally click the `Make Figure` button.

<div class="stylefig">
### Figure 7
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/OnlyRhythmicProx_Spect.png)
</div>

The bottom panel of [Figure 7](#figure-7) shows the corresponding time-frequency spectrogram for this waveform, which exhibits a high-power continuous 10 Hz signal. Importantly, in this example, the strength of the proximal input spikes were titrated to be subthreshold; i.e., our simulated cortical cells do **not** spike in this instance. This is because we assume that macroscale oscillations are generated primarily by subthreshold current flow across large populations of synchronous pyramidal neurons (see [Section 1 Background](#toc-1) above for details). In [Section 5.2](#toc-5-2) below, we explore differences in the signal when the cells are driven to spike (see also [our ERP tutorial](../05_erps/erps_in_gui.html)).

TODO: reference empty rasters in 5.2 here

TODO: ASK STEPH: Should there always be high very-low-frequency power? like at 0.1 Hz?

TODO: Add mention of fig 3 for up dendrite

<!-- To better see the 10 Hz signal, we can adjust the y-axis of the spectrogram. Under the options for `ax2` which corresponds to the spectrogram on the bottom panel, change the values for `Min Spectral Frequency (Hz)` to `0.1` and `Max Spectral Frequency (Hz)` to `40.0`. -->

<!-- Finally click `Clear axis` and then `Add plot` to regenerate the spectrogram. You should see the signal corresponding to the alpha rhthym much more clearly like in the figure below: -->

This exploration with a proximal drive is only useful in understanding how subthreshold rhythmic inputs impact the current dipole produced by the circuit. However, several features of the waveform and spectrogram of the signal do not match the recorded data shown in [Figure 1](#figure-1) and [Figure 2](#figure-2). Next, we explore the impact of rhythmic *distal* inputs only ([Section 4](#toc-4)), and then a combination of the two ([Section 5](#toc-5)).

<a id="toc-4"></a>

## 4. Simulation Rhythmic Distal Inputs: Alpha only

### 4.1 Load/view parameters to define the network structure & to "activate" the network

We will use a param file that generates bursts of distal inputs provided at the alpha frequency (10 Hz; [OnlyRhythmicDist.json](https://raw.githubusercontent.com/jonescompneurolab/hnn-data/refs/heads/main/network-configurations/OnlyRhythmicDist.json)).

TODO: Mention sim name change, and external drive import

The template cortical column networks structure for this simulation is described in the Overview and What’s Under the Hood sections. Several of the network parameter can be adjusted via the HNN GUI (e.g. local excitatory and inhibitory connection strengths), but we will leave them fixed for this tutorial and only adjust the inputs the "activate" the network.

To load the initial parameter set, navigate to the HNN GUI and click the tab labeled:

```
External drives
```

Then inside of the inside of the tab, click the button

```
Load external drives (0)
```

TODO Currently doesn't HAVE the file there
Then select the file [OnlyRhythmicDist.json](https://raw.githubusercontent.com/jonescompneurolab/hnn-data/refs/heads/main/network-configurations/OnlyRhythmicDist.json) from HNN’s param subfolder or from your local machine.

To view the parameters that "activate" the network via rhythmic distal input, click the dropdown menu labeled:

```
bursty2 (distal)
```

You should see the values of adjustable parameters displayed as in the dialog boxes below. Notice that these parameters are the same as those regulating the proximal drive in [Section 3](#toc-3). However, in this case the parameters define bursts of synaptic inputs that drive the network in a distal project pattern, shown schematically at the bottom of the dialog box.

<div class="stylefig">
<table>
### Figure 8
<tr>
<td>
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/image15.png)
</td>
<td>
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/image16.png)
</td>
<td>
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/image17.png)
</td>
</tr>
</table>
</div>

To run this simulation, navigate to the main GUI window and  click:
```
Start Simulation
```
Once completed, you will see output similar to that shown below.

<div class="stylefig">
### Figure 9
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/OnlyRhythmicDist.png)
</div>

As shown in the green histogram in the top panel of the HNN GUI above, with this parameter set, a burst of distal input spikes is provided to the network ~10 Hz (i.e., every 100 ms). Due to the stochastic nature of the inputs (controlled by the start time stdev, and Burst stdev parameters), there is some variability in the histogram of proximal input times.

The ~10 Hz bursts of distal input induces current flow **down** the pyramidal neuron dendrites, decreasing the signal below the 0 nAm baseline, which then relaxes back to zero, approximately every 100 ms. This is observed in the blue current dipole waveform in the GUI window. Once again, see [Figure 3](#figure-3) above for the relationship between distal inputs and direction of signal propagation.

Once again we will create time-frequency spectrogram for this waveform by first clicking on the `Visualization` tab. Then click on the `Layout template` dropdown menu and select `Drive-Dipole-Spectrogram`.

TODO: new step, make sure you click on the `Dataset` dropdown and select our newly-created simulation data, named `OnlyRhythmicDist`.

Finally click the `Make Figure` button.

<div class="stylefig">
### Figure 10
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/OnlyRhythmicDist_Spect.png)
</div>

The bottom panel shows the corresponding time-frequency spectrogram for this waveform that exhibits a high power continuous 10 Hz signal. Importantly, in this example, the strength of the distal input was also titrated to be subthreshold (i.e., cells do not spike) under the assumption that macroscale oscillations are generated primarily by subthreshold current flow across large populations of synchronous pyramidal neurons.

While instructional, this simulation also does not produce waveform and spectral features that match the experimental data in [Figure 1](#figure-1) and [Figure 2](#figure-2). In the next section ([Section 5](#toc-5)), we describe how combining both the 10 Hz proximal and distal drives can produce an oscillation with many characteristic features of the spontaneous SI signal [@jones_quantitative_2009].

<a id="toc-5"></a>

## 5. Simulating Combined Rhythmic Proximal and Distal Inputs: Alpha/Beta Complex

<a id="toc-5-1"></a>

### 5.1 Load/view parameters to define the network structure & to "activate" the network.

In this example, we provide a parameter set ([AlphaAndBeta.json](https://raw.githubusercontent.com/jonescompneurolab/hnn-data/refs/heads/main/network-configurations/AlphaAndBeta.json)) that produces many of the waveform and spectral features observed in our SI data ([Figure 2](#figure-2)).

The template cortical column networks structure for this simulation is described in the Overview and What’s Under the Hood sections. Several of the network parameter can be adjusted via the HNN GUI (e.g. local excitatory and inhibitory connection strengths), but we will leave them fixed for this tutorial and only adjust the inputs the "activate" the network.

To load the initial parameter set, navigate to the HNN GUI and click the tab labeled:

```
External drives
```

Then inside of the inside of the tab, click the button

```
Load external drives (0)
```

Then select the file [AlphaAndBeta.json](https://raw.githubusercontent.com/jonescompneurolab/hnn-data/refs/heads/main/network-configurations/AlphaAndBeta.json) from HNN’s param subfolder or from your local machine.

To view the new parameters that "activate" the network via both rhythmic proximal and rhythmic distal input, click the dropdown menus labeled:
```
bursty1 (proximal)
bursty2 (distal)
```
You should see the values displayed in the dialogue boxes below.

<div class="stylefig">
### Figure 11
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/image20.png)
</div>

In this simulation, the Start time mean (ms) values for both proximal and distal inputs are set to 50.0 ms, and all other parameters are the same. Note that the synaptic weights are the same as used in the previous two simulations (not shown in dialog boxes above, click on Layer 2/3 and Layer 5 to see them). The equal start time implies that the proximal and distal input bursts will arrive nearly synchronously to the network on each cycle of the 10 Hz input. Due to the stochasticity in the parameters (start time stdev, and Burst stdev) sometimes the bursts will arrive together and sometimes there will be a slight delay. As will be described further below, this stochasticity creates intermittent alpha and beta events.

<a id="toc-5-2"></a>

### 5.2 Run the simulation and visualize net current dipole

To run this simulation, navigate to the main GUI window by clicking the `Simulation` tab and then click:
```
Run
```
Once completed, you will see output similar to that shown below.

<div class="stylefig">
### Figure 12
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/AlphaAndBeta.png)
</div>

Follow the steps in the previous sections to create a time-frequency spectrogram for this waveform. The output will look similar to the figure below:

<div class="stylefig">
### Figure 13
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/AlphaAndBeta_Spect.png)
</div>

As shown in the green and red histogram in the top panel of the HNN GUI above, with this parameter set, bursts of both proximal and distal input spikes are provided to the network ~10 Hz (i.e., every 100 ms). Due to the stochastic nature of the inputs, there is some variability in the timing and duration of the input bursts such that sometimes they arrive at the same time and sometimes there is a slight offset between them. As a result, intermittent transient alpha and beta events emerge in the time-frequency spectrogram.

Alpha events are produced when the inputs occur slightly out of phase and current flow is pushed alternately up and down the dendrites for ~50 ms duration each (set by the length of the bursts inputs). Beta events occur when the burst inputs arrive more synchronously and the upward current flow is disrupted by downward current flow for ~50 ms to effectively cut the oscillation period in half. As such, the relative alpha to beta expression can be controlled by the delay between the inputs and their relative burst strengths.
TODO
We will detail this further below (see [Section 7](#toc-7) below).
<!-- We will detail this further below (see [Section 8](#toc-8) below). -->

In contrast to the results from only proximal or distal input, since the current in the pyramidal neurons is pushed both upward and downward in this simulation, the current dipole signal oscillates above and below 0 nAm, which qualitatively matches the experimental data (see [Figure 1](#figure-1) and [Figure 2](#figure-2) in [Background](#background)). Additionally, this simulation reproduces the transient nature of the alpha and beta activity and several other features of the waveform and spectrogram can be quantified to show close agreement between model and experimental results (see [Figure 2](#figure-2) above, and [@jones_quantitative_2009] for further details).

We note that here we do not directly compare the spontaneous current dipole waveform to recorded data, as was done in the ERP tutorial with a root mean squared error. This is due to the fact that the spontaneous SI signal we are simulating is not time-locked to alpha or beta events on any given trial, and the stochastic nature of the driving inputs causes variability in the timing of the alpha or beta activity, making it difficult to align recorded data and simulated results. However, a direct comparison can be made between time averaged recorded and simulated signals by comparing power spectral density waveforms.

TODO ASK STEPH "As was done in [@jones_quantitative_2009]".

<!-- An example of comparison is shown in [Section 7](#toc-7) below. -->

### 5.3 Simulating and averaging multiple trials with jittered start times creates the impression of continuous oscillations

As described in the [Background](#background) section above, our simulation goal was to study the mechanisms that reproduce features of spontaneous alpha and beta rhythms observed in un-averaged data, where the alpha and beta components are transient and intermittent ([Figure 1](#figure-1), right panel). Each tutorial section up to this point was based on simulating un-averaged data. Here, we describe how to run and average multiple "trials" (700 ms epochs of spontaneous activity). We show that, due to the stochastic nature of the proximal and distal rhythmic input, controlled by the standard deviation (stdev) of the start times, and the stdev of the input bursts, when running multiple trials, the precise timing of the input bursts on each trial is jittered, and hence the alpha and beta activity in the spectrograms on each trial is jittered. This is akin to simulating induced rhythms rather than time-locked evoked rhythms. In the averaged spectrogram across trials, the alpha and beta events accumulate without cancellation (due to the fact that spectrogram value are purely positive) creating the impression of a continuous oscillation ([Figure 1](#figure-1), left panel).

Below we illustrate the effects of "jitter" in the proximal and distal rhythmic inputs across trials in two ways. First, we examine the effects of "jitter" due to the "Burst stdev", and second due to the "Start time stdev".

To first test the effects of jittering due to "Burst stdev" and averaging across trials, we will use a param file ([AlphaAndBetaJitter0.json](https://raw.githubusercontent.com/jonescompneurolab/hnn-data/refs/heads/main/network-configurations/AlphaAndBetaJitter0.json)) with rhythmic proximal and distal inputs provided at 10 Hz, with proximal and distal inputs in phase. These are the same parameters as in the [AlphaAndBeta.json](https://raw.githubusercontent.com/jonescompneurolab/hnn-data/refs/heads/main/network-configurations/AlphaAndBeta.json) file ([Section 5.2](#toc-5-2) above), but now with 10 trials instead of 1.

To load the initial parameter set, navigate to the HNN GUI and click the tab labeled:

```
External drives
```

Then inside of the inside of the tab, click the button

```
Load external drives (0)
```

TODO: huh? this isn't right, where did this come from?

Then select the file [OnlyRhythmicDist.json](https://raw.githubusercontent.com/jonescompneurolab/hnn-data/refs/heads/main/network-configurations/OnlyRhythmicDist.json) from HNN’s param subfolder or from your local machine.

To view the new parameters, click:
```
bursty1 (proximal)
bursty2 (distal)
```
You should see the values displayed in the dialog boxes below.

<div class="stylefig">
<table>
### Figure 14
<tr>
<td>
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/image21.png)
</td>
<td>
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/image22.png)
</td>
</tr>
</table>
</div>

Notice that the Start time stdev (ms) is set to 0.0 for both proximal and distal inputs, while the Burst stdev (ms) is 20. TODO: is this different than before? why is this highlighted

TODO: add incr number of trials here!! Why are so many instructions left out???

To run this simulation, navigate to the `Simulation` tab and click

```
Run
```

This simulation will take longer to run because it uses 10 trials. Once completed, you will see output similar to that shown below.

<div class="stylefig">
### Figure 15
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/AlphaAndBetaJitter0_3trials.png)
</div>

Follow the previous steps from [Section 5.2](#toc-5-2) to create a time-frequency spectrogram. The output will look like the following:

<div class="stylefig">
### Figure 16
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/AlphaAndBetaJitter0_3trials_spect.png)
</div>

Notice that the input histograms for distal (green) and proximal (red) input accumulated across the 10 trials, now have higher values than before (up to ~20 compared to 5 in [Section 3.3](#toc-3-3)) and the burst inputs are slightly broader on each cycle, since these histograms represent the accumulated activity from 10 simulations, where the standard deviation in the Burst duration across trials is 20 ms. Approximately 10 Hz rhythmicity in the timing of the distal and proximal inputs can be clearly visualized (note also the symmetric profile of the histograms). However, on any individual trial, the coincidence of inputs leading to alpha or beta events displays some variability due to the stochastic parameter value (Burst stdev=20 ms). This is observed in the dipole waveforms shown for each trial (example shown below).

TODO: can we viz single trials compared to the average?

TODO: new subsection for new sims

In the next simulation, we will jitter the start times of rhythmic inputs across trials with the Start time stdev, in addition to a non-zero Burst stdev. This will add additional variability to the timing of the transient alpha and beta events on each trial, and hence produce even more continuous bands of activity in the averaged spectrogram.

First, navigate to the `External drives` tab and open the `bursty1 (proximal)` and `bursty2 (distal)` dropdown menus. Change the start time stdev from 0 ms to 50 ms in the timing tabs. The dialog boxes should now look as follows.

<div class="stylefig">
<table>
### Figure 17
<tr>
<td>
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/image23.png)
</td>
<td>
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/image24.png)
</td>
</tr>
</table>
</div>

To run this simulation, click Start Simulation from the main GUI window. Follow the steps from [Section 5.2](#toc-5-2) again to create a time-frequency spectrogram.

Once completed, you will see output similar to that shown below.

<div class="stylefig">
### Figure 18
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/AlphaAndBetaJitter50_3trials_spect.png)
</div>

TODO there is no longer any "View spectrograms tab"

Notice that the input histograms for distal (green) and proximal (red) input accumulated across the 10 trials now show little rhythmicity due to the jitter in the rhythmic input start times across trials (Start time stdv (ms) = 50), in addition to jitter due to the Burst stdev (ms) = 20. However, if we were to visualize histograms on each individual trial (using the View spectrograms tab), they would show the ~10 Hz and 20 Hz (alpha and beta) rhythmicity. It is also difficult to visualize rhythmicity in any of the overlaid dipole waveforms. However, on each trial, alpha and beta rhythmicity is present, and even more continuous bands of alpha and beta activity are observed (compare to averaged data in [Figure 1](#figure-1) left panel; n=100 trials) when the spectrograms from individual trials are averaged. Running more trials will further increase the continuous nature of alpha and beta activity across time.

### 5.4 Viewing network spiking activity

Importantly, as stated above in this example the strength of the proximal and distal input were titrated to be subthreshold (cells do not spike) under the assumption that macroscale oscillations are generated primarily by subthreshold current flow across large populations of synchronous pyramidal neurons. We can verify the subthreshold nature of the inputs by viewing the spiking activity in the network.

Return to the single-trial parameter set as in [Section 5.1](#toc-5-1) and [Section 5.2](#toc-5-2), by loading the
[AlphaAndBeta.json](https://raw.githubusercontent.com/jonescompneurolab/hnn-data/refs/heads/main/network-configurations/AlphaAndBeta.json)
file and run the simulation. Then navigate to the `Visualization` tab, click the `Layout template` dropdown menu select `Dipole-Spikes (2x1)`. Finally click `Make figure`.

You should see the following window.

<div class="stylefig">
### Figure 19
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/AlphaAndBeta_Spikes.png)
</div>

In this window, the rhythmic distal (green/top) and proximal (red/middle) inputs bursts histograms are shown along with the spiking activity in each population of cells (bottom panel). In this case, the alpha and beta events are indeed produced through subthreshold processes and there is no spiking produced in any cell in the network (no dots present in the bottom raster plot).

### 5.5 Exercises for further exploration

Try decreasing or increasing the number of trials in the above simulations to see how these changes impact the continuity of alpha/beta power over time. View some of the individual spectrograms to see that alpha/beta are maintained on individual trials.

<a id="toc-6"></a>

## 6. Calculating and Viewing Power Spectral Density (PSD)

HNN provides a feature to calculate and view the power spectral density (PSD) of the simulated signal and imported data (Note: the PSD is calculated as the time average of the spectrogram, in the simulation examples).

To calculate and view the PSD, navigate to the `Visualization` tab, click on the `Layout template` dropdown, and select `PSD Layers (3x1)`

You should see something similar to the following window.

<div class="stylefig">
### Figure 20
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/AlphaAndBetaJitter50_3trials_PSD.png)
</div>

The PSD Viewer window shows the net current dipole (bottom panel) and contribution from each layer in the network separately (top panels). This example was run using the parameter set described in [Section 5](#toc-5). PSD from the simulation shows a strong peak in the alpha (~10 Hz) band, with a lower peak power in beta band (~20 Hz).

<a id="toc-7"></a>

<!-- Commenting this section out until the issues with carefully loading alpha data from the "S1_ongoing.txt" file are resolved, see https://github.com/jonescompneurolab/hnn-core/issues/617
-->
<!-- ## 7. Comparing model output and recorded data -->

<!-- TODO Work in progress! -->

<a id="toc-8"></a>

<!-- ## 8. Adjusting parameters -->
## 7. Adjusting parameters

Parameter adjustments will be key to developing and testing hypotheses on the circuit origin of your own low-frequency rhythmic data. HNN is designed so that many of the parameters in the model can be adjusted from the GUI (see the Tour of the GUI tutorial).

Here, we’ll walk through examples of how to adjust several "Rhythmic Proximal/Distal Input" parameters to investigate how they impact the alpha and beta rhythms described above. We end with some suggested exercises for further exploration.

<!-- ### 8.1 Changing the strength (post-synaptic conductance) and synchrony of the distal drive increases beta activity -->
### 7.1 Changing the strength (post-synaptic conductance) and synchrony of the distal drive increases beta activity

We described above ([Section 5](#toc-5)) that the timing of proximal and distal inputs can lead to either alpha events (when the bursts arrive to the local network out of phase) or beta events (when the bursts arrive in phase).

We have also found that other factors that contribute to the prevalence of beta activity are the strengthand synchrony of the distal inputs; beta activity is increased with stronger and more synchronous subthreshold drive, where the beta frequency is set by the duration of the driving bursts (~50ms) ([@jones_quantitative_2009] and [@sherman_neural_2016]). The strength is controlled by the postsynaptic conductance, and the synchrony is controlled by the Burst stdev in the "Rhythmic Distal Inputs" dialog box. We will demonstrate this here.

TODO re-add `AlphaAndBeta.json` load here, and change simulation `Name` to something like `AlphaAndBetaIncrBeta`

Open the `External drives` tab and click on the dropdown menu
 ```
bursty2 (distal)
```

TODO typo ms to Hz

Reduce the Burst stdev (Hz) value from 20 ms to 10 ms. This will create higher synchrony in the timing of the distal input bursts. Under both the Layer 2/3 and Layer 5 tabs, increase the postsynaptic condances weights of the AMPA synapses onto the Layer 2/3 and Layer 5 pyramidal neurons from 5.4e-5 $\mu S$ to 6e-5 $\mu S$. Both of these changes will cause the distal input burst to push a greater amount of current flow down the pyramidal neuron dendrites. The "Rhythmic Distal Input" dialog windows should look as shown below.

TODO 0.00006 or "6e-5" (either works)


<div class="stylefig">
<table>
### Figure 21
<tr>
<td>
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/image28.png)
</td>
<td>
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/image29.png)
</td>
<td>
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/image30.png)
</td>
</tr>
</table>
</div>

Next, we will test how these parameter changes affect the simulation. Select the `Simulation` tab and click the `Run` button. Then follow the steps from [Section 5.1](#toc-5-1) or [Section 5.2](#toc-5-2) to create a time-frequency spectrogram Once completed, you will see output in the GUI similar to that shown below.

<div class="stylefig">
### Figure 22
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/sec6pt1-oldFig20-nonspect.png)
</div>

<div class="stylefig">
### Figure 23
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/sec6pt1-oldFig20-spect.png)
</div>

First, notice that the histogram profile of the distal input bursts (green) are narrower corresponding to more synchronous input than in the prior simulation ([Section 5](#toc-5)). Second, notice that the waveform of the oscillation is different with a sharper downward deflecting signal, due to to the stronger distal input. These deflections increased ~20 Hz beta activity, as seen in the corresponding spectrogram (compare to spectrogram in [Section 5.1](#toc-5-1)). The 20 Hz frequency is set by the duration of the downward current flow, which with this parameter set is approximately 50 ms (see [@sherman_neural_2016] for further details).

<!-- ### 8.1.1 Exercise for further exploration -->
### 7.1.1 Exercise for further exploration

Try changing the frequency of the rhythmic distal drive from 10 Hz to 20 Hz. Try other frequencies for the proximal and distal rhythmic drive. How do the rhythms change? See how changes in the Burst stdev effects the rhythms expressed.

<!-- ### 8.2 Increasing the strength (post-synaptic conductance) of the distal drive further creates high frequency responses due to induced spiking activity -->
### 7.2 Increasing the strength (post-synaptic conductance) of the distal drive further creates high frequency responses due to induced spiking activity

Recall that in the above simulations, the strength of the rhythmic proximal and distal inputs were chosen so that the cells remained subthreshold (no spiking). We will now demonstrate what happens if we increase the strength of the inputs far enough to induce spikes. Instead of simulating subthreshold alpha/beta events, we will see that the dipole signals are dominated by higher-frequency events created by spiking activity. We note that the produced waveforms of activity are, to our knowledge, not typically observed in MEG or EEG data, supporting the notion that alpha/beta rhythms are created through subthreshold processes.

TODO AlphaAndBetaSpiking

TODO ms to Hz typo again

To test this, select the `External drives` tab, open the `bursty2 (distal)` dropdown menu, and change the parameters as follows. Under the timing tab, change the Burst stdev value back to 20 ms. Under both the Layer 2/3 and Layer 5 tabs, increase the postsynaptic conductance weights of the AMPA synapses onto the Layer 2/3 and Layer 5 pyramidal neurons from 6e-5 $\mu S$ to 40e-5 $\mu S$ (a big change that will provide enough current to cause the cells to spike). The "Rhythmic Distal Input" dialog windows should look as shown below.

TODO 0.00040 or "40e-5"

<div class="stylefig">
### Figure 24
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/image25.png)
</div>

Next, run the simulation. Click on Start Simulation from the main GUI window. Once completed, you will see output similar to that shown below.

<div class="stylefig">
### Figure 25
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/sec6pt2-oldFig23-nonspect.png)
</div>

TODO 
Notice that the histogram profile of the distal input bursts (green) are once again wider corresponding to less synchronous input and comparable to those shown in the example in [Section 5](#toc-5). However, in this case the postsynaptic conductance of these driving spike is significantly larger (40e-5 $\mu S$). This strong input induces spiking activity in the pyramidal neuron on several cycles of the drive (2.5 shown here) resulting in a sharp and rapidly oscillating dipole waveform. The corresponding dipole spectrogram shows broadband spiking from ~60-120 Hz. This type of activity is not typically seen in EEG or MEG data, and hence unlikely to underlie macroscale recordings.

We can verify that the neurons are spiking by looking at the spiking raster plots. To do so, navigate to `Visualization` tab, click the `Layout template` dropdown menu and select `PSD Layers (3x1)`. Then click `Make figure`.

<div class="stylefig">
### Figure 26
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/sec6pt2-oldFig25.png)
</div>

Notice that highly synchronous neuronal spiking in each population coincides with the high-frequency events seen in the waveform and spectrogram. The waveform response is induced by the pyramidal neuron spiking which creates rapid back-propagating action potentials and repolarization of the dendrites.

Hypothesis testing:This simulation demonstrates that HNN can be used to test the limits of physiological variables and to see how, as parameters are varied, simulations results can be similar or dissimilar to experimental data.

<!-- ### 8.2.1 Exercise for further exploration -->
### 7.2.1 Exercise for further exploration

View the contribution of Layer 2/3 and Layer 5 to the net current dipole waveform and compare with the spiking activity in each population. How do each contribute? Try also to change the proximal input parameters instead of the distal input parameters.

Adjust one of the parameter regulating the local network connections. What happens?

<!-- ### 8.3 Increasing the delay between the proximal and distal inputs to anti-phase (50 ms delay) creates continuous alpha oscillations without beta activity -->
### 7.3 Increasing the delay between the proximal and distal inputs to anti-phase (50 ms delay) creates continuous alpha oscillations without beta activity

We mentioned above that, in addition to parameters controlling the strength and synchrony of the distal (or proximal) drive, the relative timing of proximal and distal inputs is an important factor in determining relative alpha and beta expression in the model. Here we will demonstrate that out-of-phase, 10 Hz burst inputs can produce continuous alpha activity without any beta events. 

# AlphaOnly

For this simulation, load the [AlphaAndBeta.json](https://raw.githubusercontent.com/jonescompneurolab/hnn-data/refs/heads/main/network-configurations/AlphaAndBeta.json) parameter file as described in [Section 5](#toc-5) by clicking Set Parameters From File and selecting the file from HNN’s param subfolder. To view the new parameters, click on the `External drives` tab. Next, in the `bursty1 (proximal)` dropdown menu, change the start time mean from 50 to 100 ms. The timing tabs in the Rhythmic Proximal and Distal Input dialog boxes should look as follows:

# TODO should be distal change, NOT proximal

<div class="stylefig">
<table>
### Figure 27
<tr>
<td>
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/image26.png)
</td>
<td>
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/image27.png)
</td>
</tr>
</table>
</div>

Note that both the proximal and distal input frequency are set to 10 Hz (bursts of activity every ~100 ms). Since the proximal input Start time mean is 50.0 ms and the the distal input Start time mean is 100.0 ms, the input will, on average, arrive to the network a 1/2 cycle out of phase (i.e., in antiphase, every 50 ms).

Next, we will run the simulation to investigate the impact of this parameter change. Click on Start Simulation from the main GUI window. Once completed, you will see output similar to that shown below.

<div class="stylefig">
### Figure 28
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/sec6pt3-oldFig27-everything-except-spect.png)
</div>

<div class="stylefig">
### Figure 29
![](https://raw.githubusercontent.com/jonescompneurolab/jones-website/master/images/textbook/content/06_alpha_beta/images/sec6pt3-oldFig27-spect.png)
</div>

Notice that the histogram profile of the proximal (red) and distal (green) input bursts are generally ½ cycle out-of-phase (antiphase). This rhythmic alteration of proximal followed by distal drive induces alternating subthreshold current flow up and down the pyramidal neuron dendrites to create a continuous alpha oscillation in the current dipole waveform that oscillates around 0 nAm. The period of the oscillation is set by the duration of each burst (~50 ms, controlled in part by Burst stdev) and the 50 ms delay between the inputs on each cycle (due to different start times). The corresponding spectrogram shows continuous nearly pure alpha activity. This type of strong alpha activity is similar to what might be observed over occipital cortex during eyes closed conditions.

<!-- ### 8.3.1 Exercise for further exploration -->
### 7.3.1 Exercise for further exploration

Try changing the delay between the proximal and distal drive by varying amounts. What happens to the rhythm expressed?

Can you create a simulation where other frequencies are expressed? How is it created? Are the cells spiking or subthreshold?

<a id="toc-9"></a>

<!-- ## 9. Have fun exploring your own data! -->
## 8. Have fun exploring your own data!

<!-- Follow sections 1-8 above using your data and parameter adjustments based on your own hypotheses. -->
Follow sections 1-7 above using your data and parameter adjustments based on your own hypotheses.

## References
