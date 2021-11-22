# Neural Data Pipeline for Kilosort
This project was commissioned to create an accessible pipeline for optimizing sorting of dense and large-scale neural data. It is built around a pre-existing library called Kilosort which uses machine learning techniques such as clustering and template matching to sort neurons across time even if they are located extremely close to one another. This pipeline took the already existing library and created a pre and post processing portion to minimize manual work to nearly zero. 

## Pre-Processing
The Pre-Processing portion of this project involved 3 primary objectives. First, it needed to load in continuous data across multiple rhd (form of binary data) files and package them in a single binary .dat file for kilosort to use. It then needed to clean the data before sending them to kilosort as neural data can be prone to [movement artifact noise](https://radiopaedia.org/articles/motion-artifact-2?lang=us). It also needed to create traces of the raw data before and after cleaning so that its work could be checked independently by researchers.  To do so, it opens the first rhd file, creates figures for the data before cleaning, cleans the data, creates figures again after the cleaning, and writes the data to a binary file. It then opens the next file and repeats the entire process and writes the data to the same binary file. It  continues with this process until all of the rhd files have been processed and a single binary file remains.

### Data-Cleaning
Since a neuron will mostly appear on a single to a couple of channels, the cleaning of the data is done through taking the median of the data across multiple channels and zeroing out values that exceed some predetermined threshold. Given that the median is of enough channels, no neural data will be caught in the crossfire and all movement artifacts will be removed providing a much more clear picture of the neural signal.

## Post-Processing
The primary goal of the Post-Processing section was to package all of the desired results into a single structure which could be easily interpreted and used by anyone. Kilosort saves most of its output across multiple files that require short commands or scripts to extract so streamlining this process was an absolute must for the researchers. Most of the results such as spike times were easy to process. However, some required opening the raw data again and extracting portions of the trace using the spike times. This was done to extract the waveforms of the neural signal.

Finally the post-processing script created a single interactive (the example is a screenshot of the report. The graphs are completely interactable) and digestible visual report for each neuron:

![Example-Report](https://user-images.githubusercontent.com/35672096/142940293-c46fed76-c473-425d-8bb0-a6a7c5a48e7f.png)

Traceview: trace of the raw data with the spikes highlited in red
Waveform: Graph of spikes across time overlapped to show waveform of signal
[PCA](https://royalsocietypublishing.org/doi/10.1098/rsta.2015.0202) - [wiki](https://en.wikipedia.org/wiki/Principal_component_analysis): Plot of the combinations of PCAs as well as plot of PCAs across time
Amplitude: view of the amplitude of spikes across time
[ISI](https://www.tau.ac.il/~tsirel/dump/Static/knowino.org/wiki/Interspike_interval.html#:~:text=The%20interspike%20interval%20is%20the,messengers%20to%20affect%20other%20neurons.): Histogram for the refractory period of the signal. used to see whether multiple signals were sorted as one
Channel number and state: The last portion of the report prints out what channel the signal was recoded on and what state it was sorted in by kilosort (good/mua/bad)
