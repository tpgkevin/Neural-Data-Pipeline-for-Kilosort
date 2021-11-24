# Neural Data Pipeline for Kilosort
This project creates an accessible pipeline for optimizing sorting of dense and large-scale neural data. It is built around a pre-existing library called Kilosort which uses machine learning techniques such as clustering and template matching for indentifying action potential that belong to a single neuron. This pipeline took the already existing library and created a pre and post processing portion to optimize the process.

## Pre-Processing
The Pre-Processing portion of this project involved 3 primary objectives. First, it needed to load in continuous data across multiple rhd (form of binary data) files and package them in a single binary .dat file for kilosort to use. It then needed to clean the data before sending them to kilosort as neural data can be contaminateds by different sources of noice such as [motion artifact noise](https://www.ahajournals.org/doi/full/10.1161/CIRCEP.111.964973). It also needed to create traces of the raw data before and after cleaning so that its work could be checked independently by researchers.  To do so, it opens the first rhd file, creates figures for the data before cleaning, cleans the data, creates figures again after the cleaning, and writes the data to a binary file. It then opens the next file and repeats the entire process and writes the data to the same binary file. It continues with this process until all of the rhd files have been processed and a single binary file remains.

### Data-Cleaning
Since a neuron will mostly appear on a single to a couple of channels, all neural signal will be filtered out from the median of a large number of channels. However, noise, which largely remains consistent across channels, will be detectable on the median. By setting a low threshold on this median, we can determine and zero timepoints that are contaminated by noise while leaving neural signal unaffected. This method proved more effective than simple median subtraction in artifact removal for spike sorting.


## Post-Processing
The primary goal of the Post-Processing section was to package all of the desired results into a single structure which could be easily interpreted and used by researchers.  Some of these results were extracted from kilosort's output, while some required efficiently opening and processing the raw data. 

Finally the post-processing script created a single interactive and digestible visual report for each neuron:  

(allowed researchers to validate results of kilosrt and also to ctalouge the results of the spike-sorting output.

![Example-Report](https://user-images.githubusercontent.com/35672096/143326569-563bc671-ed59-442a-a9df-3a60cb5c3ad9.png)

Traceview: trace of the raw data with the spikes highlighted in red  

Waveform: plot of the spike waveforms overlapped to verify the consistency of the waveform shape

[PCA](https://royalsocietypublishing.org/doi/10.1098/rsta.2015.0202) - [wiki](https://en.wikipedia.org/wiki/Principal_component_analysis): The first three PCAs of the waveform shape plotted against eachother as well as across time  

Amplitude: view of the amplitude of spikes across time  

[ISI](https://www.tau.ac.il/~tsirel/dump/Static/knowino.org/wiki/Interspike_interval.html#:~:text=The%20interspike%20interval%20is%20the,messengers%20to%20affect%20other%20neurons.): Histogram of the interspike interval used to see whether the refractory period is violated  

Channel number and quality: The last portion of the report prints out what channel the the unit was recorded from and its quality of sorting (good/mua/bad)
