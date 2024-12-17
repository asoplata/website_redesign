::: {.srj-header .hnn-header .darken-header}
::: {.srj-headertext}
HNN-GUI Tutorials
:::
:::

\
\

::: {.srj .srj-spacious}
![](https://hnn.brown.edu/wp-content/uploads/2019/06/Model_PN.png){.srj-small-icon}\

# Overview {#overview .srj-center .hnn-purple}

The overall simulation process in HNN is similar to that in any
large-scale neural model study.  Working with large-scale neural models
is challenging because not all parameters can be estimated at once. In
practice, pieces of the model (e.g. individual cells and synaptic
connections) are developed and parameters are tuned and fixed based on
best-known biophysical constrained (e.g. morphology and physiology).
Once an acceptable baseline model is developed, most parameters are
fixed and parameter tuning and estimation are performed on a limited
targeted set of parameters to test hypotheses on how those parameters
contribute to specific signals of interest and changes in the signal
across experimental conditions/subjects.\
\
The underlying neocortical column model distributed with HNN has been
pre-constructed and tuned based on current-day knowledge of
generalizable principles of neocortical circuitry and the biophysical
origin of M/EEG [current sources
signals](https://hnn.brown.edu/getting-started/). The template
neocortical model was developed and applied in several [prior
publications](https://hnn.brown.edu/publications/). Simulation
experiments in HNN begin by  "activating" the neocortical column model
based on the experimental conditions and questions of interest in your
study and "tuning" a targeted set of parameters **to develop and test
hypotheses** on cell and network configurations that can provide a close
fit to data.\
\
 In the tutorials provided here, we demonstrate how we "activated" the
HNN network and tuned parameters to study the neural underpinnings of
several of the most commonly recorded EEG/MEG signals, including
event-related potentials (ERPs) and low-frequency rhythms:
:::

\

::: {.srj .srj-left .srj-spacious style="max-width:480px;"}
1.  Event Related Potentials (ERPs) (0-175ms)
2.  Alpha (7-14Hz) and Beta Rhythms (15-29Hz)
3.  Gamma Rhythms (30-80Hz)
:::

::: {.srj .srj-spacious}
 These examples are based on our published studies investigating
spontaneous and evoked neural dynamics source localized to the primary
somatosensory cortex1--5. By learning how to simulate these signals, you
will gain a general sense of the workflow to begin testing hypotheses on
the origin of your data. The HNN software provides a canonical
neocortical circuit template model, data sets, and initial parameter
sets to work with, based on our prior studies. We take you step-by-step
through the parameter settings required to replicate our experimental
data, describe how to compare experimental data to the model output, and
how to adjust parameters to test alternate hypotheses on signal
generation.\
\
**Importantly, our tutorials of use are not intended to propagate
singular theories on the generation of any particular brain rhythm or
ERP signal.** Rather, they are meant to guide you in the simulation
process in HNN and to provide a pre-tuned starting point for your
investigation based on the best-known biophysics of neocortical
circuitry and M/EEG primary current source generation. Please
see [\"Getting
Started\"](https://jonescompneurolab.github.io/hnn-tutorials/starting_with_data/getting_started) for
further background information and a discussion of expansion to other
more specialized neocortical template models.
:::

\

::: {.srj .srj-spacious}
# Step-by-Step Simulation Process {#step-by-step-simulation-process .srj-center .hnn-purple}

\
Each of our tutorials follows the following logic and step-by-step
process:\
\

::: {.srj .srj-left .srj-spacious style="max-width:750px;"}
1.  Load your M/EEG waveform data (optional)
2.  Load a cortical column network structure (starting template is
    provided - see [[HNN Template
    Model]{style="text-decoration: underline;"}](https://hnn.brown.edu/under-the-hood/))
3.  "Activate" the local network by defining layer-specific driving
    inputs (templates provided). The driving input can be in the form
    of (i) spike trains (single spikes or bursts of rhythmic input) that
    activate post-synaptic targets in the local network, (ii) current
    clamps (tonic drive), or (iii) noisy background drive. The framework
    of the spike train drive is discussed below.
4.  Run the simulation and visualize (i) net current dipole activity in
    the time and/or frequency domain, (ii) layer-specific dipole
    activity, (iii) spiking activity of individual cells
5.  Adjust parameters and repeat to explore how the model can account
    for your data
:::

::: {.srj .srj-spacious}
Before starting the tutorials, it is necessary to understand two things
**(I)** the orientation of currents up or down the pyramidal neuron
dendrites in your data, and **(II)** the framework for the adjustable
spike train drive in HNN's template network that initiates current flow
up and down the dendrites.\
\
**I.A first step in interpreting the circuit mechanisms generating
source localized data with HNN is to determine the direction of
electrical current flow in relation to the neocortical layers** , also
referred to as current in or out of the cortex. This requires the
application of inverse solution methods explicitly designed to infer the
location and directionality of the primary currents at specific points
in time. Inverse methods are not distributed with HNN but there are many
excellent software (e.g. MNE_python, Fieldtrip, etc\...). When applying
inverse solution software to your data, you must choose a method that
estimates the location of the underlying dipole currents along with the
current direction at any given point in time. By overlaying the source
orientation on structural MRI images, you can infer if the current is
going down or up the cortical pyramidal neuron dendrites (i.e., in or
out of the cortex). This knowledge constrains the HNN interpretation.
For further details and a comment on how to study non-source localized
data please see the [Getting Started With Your Data
Tutorial](https://jonescompneurolab.github.io/hnn-tutorials/starting_with_data/getting_started),
and for examples see Jones et al. 2007^1^; Kohl et al 2021^6^; Thorpe et
al 2021^7^.\
\
**II. The framework for adjusting the spike train drive in HNN's
template network** is based on known layer-specific patterns of synaptic
input to cortical circuits. Local cortical networks receive synaptic
input from spiking activity in other parts of the brain through two
primary projection pathways. One pathway, representative of the pathway
from lemniscal thalamus to the cortex, arrives in granular layers and
propagates to the proximal dendrites of neocortical pyramidal neurons
(the source of the primary dipole current -- see [[Getting
Started]{style="text-decoration: underline;"}](https://hnn.brown.edu/getting-started/)).
HNN refers to this as proximal drive, as shown in red below. The other
pathway represents input from higher-order cortical areas or
non-lemniscal input to the distal dendrites of neocortical pyramidal
neurons\' supragranular layers. HNN refers to this as distal drive, as
shown in green below.\
\

::: {.srj-center}
![](https://lh3.googleusercontent.com/SzaE8a6GNwEcigFMLJH5lyV_YRxl09BcZtmoURdC_AA--_ISfgx9KNwJU25DMmWqO_IANKEoktun-nLMaeJKFXy4YmgbH2pIT5HLG-OciRle-vnSsmpsmTk5z7cHaIfb6Dq4-TfG){width="383"
height="323"}
:::

\
HNN allows you to define and adjust trains of action potentials that
generate excitatory synaptic drive to targets in the local cortical
network through these two pathways. There are several ways to change the
pattern of action potential drive through different "buttons" built into
the HNN GUI: "evoked response", "rhythmic proximal" and "rhythmic
distal". The dialog boxes that open with these buttons allow the
creation and adjustment of patterns of evoked response drive or rhythmic
drive to the network. These design features are motivated by our prior
studies. We have simulated evoked responses through a sequence of
proximal and distal spike train drive^1-2^, this sequence is provided as
default parameters set to begin simulating evoked responses; parameters
can be adjusted, and additional inputs can be created in "evoked
response" dialog box, as detailed further in the evoked response
tutorial below. We have simulated low-frequency alpha and beta rhythms
through patterns of rhythmic drive (repeated bursts of spikes) through
proximal and distal projection pathways^1,3-4^. Default parameters set
to begin simulating low-frequency rhythms via rhythmic drive are
provided; parameters can be adjusted in the corresponding dialog boxes,
as detailed further in the alpha/beta tutorial below. 
:::

\

::: {.srj .srj-spacious}
![](https://hnn.brown.edu/wp-content/uploads/2019/06/Gears.png){.srj-small-icon}\

# Recent Model Updates {#recent-model-updates .srj-center .hnn-violet}

HNN team has been working to improve CA dynamics in the L5 PN in the
cortical template model, and an updated version of the model using the
improved calcium dynamics was included in recent
[publication](https://link.springer.com/article/10.1007/s10548-021-00838-0).
To use this updated model and reproduce figures in the paper, follow the
instructions listed [here](https://github.com/kohl-carmen/HNN-AEF).
:::

\
\

::: {.srj}
::: {.srj-small-icon}
![](https://hnn.brown.edu/wp-content/uploads/2019/06/EEG_Brain.png)
:::

\

# Tutorials {#tutorials .hnn-blue}
:::

\

::: {.srj-grid-wrapper}
::: {.srj-grid}
[](https://jonescompneurolab.github.io/hnn-tutorials/gui/tour_gui)

::: {.Tutorial-cell}
::: {.srj-cell-contents}
![](https://hnn.brown.edu/wp-content/uploads/2019/06/Gui-2.png)

::: {.srj-cell-text .hnn-blue}
Using the GUI & Saving Data
:::
:::
:::

[](https://jonescompneurolab.github.io/hnn-tutorials/erp/erp)

::: {.Tutorial-cell}
::: {.srj-cell-contents}
![](https://hnn.brown.edu/wp-content/uploads/2019/06/ERP_.png)

::: {.srj-cell-text .hnn-violet}
Evoked Potentials
:::
:::
:::

[](https://jonescompneurolab.github.io/hnn-tutorials/alpha_and_beta/alpha_and_beta)

::: {.Tutorial-cell}
::: {.srj-cell-contents}
![](https://hnn.brown.edu/wp-content/uploads/2019/06/Alpha-beta.png)

::: {.srj-cell-text .hnn-purple}
Alpha and Beta
:::
:::
:::

[](https://jonescompneurolab.github.io/hnn-tutorials/gamma/gamma)

::: {.Tutorial-cell}
::: {.srj-cell-contents}
![](https://hnn.brown.edu/wp-content/uploads/2019/06/Gamma-2.png)

::: {.srj-cell-text .hnn-blue}
Gamma
:::
:::
:::

[](https://jonescompneurolab.github.io/hnn-tutorials/starting_with_data/getting_started)

::: {.Tutorial-cell}
::: {.srj-cell-contents}
![](https://hnn.brown.edu/wp-content/uploads/2019/06/Viewingdata.png)

::: {.srj-cell-text .hnn-violet}
Starting with Your Data
:::
:::
:::

[](https://jonescompneurolab.github.io/hnn-tutorials/optimization/optimization)

::: {.Tutorial-cell}
::: {.srj-cell-contents}
![](https://hnn.brown.edu/wp-content/uploads/2019/06/Model_PN.png)

::: {.srj-cell-text .hnn-purple}
Optimization
:::
:::
:::
:::
:::

\
\
\
\

::: {.srj .srj-left}
#### References {#references .srj-center}

1.  Jones, S. R., Pritchett, D. L., Stufflebeam, S. M., Hämäläinen, M. &
    Moore, C. I. Neural correlates of tactile detection: a combined
    magnetoencephalography and biophysically based computational
    modeling study. J. Neurosci. 27, 10751--10764 (2007).
2.  Jones, S. R. et al. Quantitative analysis and biophysically
    realistic neural modeling of the MEG mu rhythm: rhythmogenesis and
    modulation of sensory-evoked responses. J. Neurophysiol. 102,
    3554--3572 (2009).
3.  Ziegler, D. A. et al. Transformations in oscillatory activity and
    evoked responses in primary somatosensory cortex in middle age: a
    combined computational neural modeling and MEG study. Neuroimage 52,
    897--912 (2010).
4.  Lee, S. & Jones, S. R. Distinguishing mechanisms of gamma frequency
    oscillations in human current source signals using a computational
    model of a laminar neocortical network. Front. Hum. Neurosci. 7, 869
    (2013).
5.  Sherman M, Lee S, Law R, Haegens S, Thorn C, Hamalainen M, Moore CI,
    Jones SR. (2016) Neural mechanisms of transient neocortical beta
    rhythms: Converging evidence from humans, computational modeling,
    monkeys, and mice. Proc. Natl. Acad. Sci.; (2016 Epub).
6.  Kohl, C., Parviainen, T. & Jones, S.R. Neural Mechanisms Underlying
    Human Auditory Evoked Responses Revealed By Human Neocortical
    Neurosolver. Brain Topogr 35, 19--35 (2022).
    https://doi.org/10.1007/s10548-021-00838-0
7.  Thorpe, R., Black, C.J., Borton, D.A., Hu, Li, Saab, C.Y., Jones,
    S.R. (2021) Distinct neocortical mechanisms underlie human SI
    responses to median nerve and laser evoked peripheral
    activation. bioRxiv 2021.10.11.463545; doi: https://doi.org/10.1101/2021.10.11.463545
:::

\
\
\
\
\
\
 
:::