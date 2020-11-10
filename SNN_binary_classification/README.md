# SNN binary classification

A repo for training spiking neural networks to a bright or dark stimulus. Then using those networks to classify a random stimulus as either bright or dark.

## Requirements

* Python 3.6 or >
* arpgarse
* h5py
* pyNN
* numpy
* matplotlib
* pandas

## Python scritps and files

### Imitation\_V1\_Net.py

This script creates spiking neural networks consisting of two 'source' layers and a 'target' layer. The source layers are one 'on' layer, and one 'off' layer. The firing rates of the cells in these layers are chosen from a beta distribution. The parameters of this distribution are modulated by the stimulus. The stimlulus is basically represented by these parameters alone. A 'bright' stimulus is represented by the cells in the 'on' source layer firing at a higher rate, and the cells in the 'off' layer firing at a lower rate. Similarly, a 'dark' stimulus is represented by the cells in the 'off' source layer firing at a higher rate, and the cells in the 'on' layer firing at a lower rate. See 'utilities/getOnOffSourceRates' for code details.

The 'target' layer consists of integrate-and-fire cells.

There are excitatory feed-forward connections from each source layer to the target layer, and inhibitory lateral connections from the target layer to the target layer. 

The out of the network is the spikes of the target layer.

The script trains the networks by presenting a bright or dark stimulus for a period of time. The strength of the synapses are trained by spike-timing-dependent plasticity (STDP). One 'bright' network is trained, and one 'dark' network. Each network, that is, the connections and their strenthgs, is saved down in hdf5 files.

This set up is inspired by the 'classification' part of Hopkins et al. (2018).

### load\_training\_results.py

This script loads in a bright and a dark network, presents bright or dark stimuli at random to both networks, and records the firing rates of the networks. The classification of the stimulus is exactly which network has the higher firing rate. The number of spikes fired by each network is recorded and saved down. Finally, some plots can be made. There is also the option to adjust the weight of the lateral connections, this will allow experiments with the E/I imbalance.

## WeightRecorder.py

A class to define a callback use to record the weights of a network as the network is trained. See the pyNN documentation for more information.

## PoissonWeightVariation.py

A class to define a callback that adjusts the firing rate of Poisson firing cells after each iteration of the network. See the pyNN documentation for more information.

### Other python scripts

The remaining scripts are left over from when I was learning how to use pyNN to construct spiking neural networks. These scripts construct simpler networks and save or plot their results.

### utilities.py

You will find some useful functions in here. Nothing complicated.

## Bash scripts

There are two bash scripts in the 'sh' directory. The 'run\_training\_over\_lat\_probs.sh' script trains many networks each with a different probability of creation for later connections. It does this by looping over the 'imitation\_v1\_net.py' script with different input parmeters.

The 'classify\_using\_trained\_nets.sh' runs the 'load\_training\_results.py' many times using different input files. This saves down the classification results of many networks, for comparison.

## Other directories and files

The 'csv' directory contains the csv file where classifaction results are saved.

The 'h5' directory is where the h5 files containing network information are saved. Note that h5 files are not added to the repo because of their large memory requirement.

The 'txt' directory contains a single file with a record of command lines that I used in the past. This might be useful to see how scripts should be run.
