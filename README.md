# Neural Data Pipeline for Kilosort
This project was commissioned to create an accessible pipeline for optimizing sorting of dense and large-scale neural data. It is built around a pre-existing library called Kilosort which uses machine learning techniques such as clustering and template matching to sort neurons across time even if they are located extremely close to one another. This pipeline took the already existing library and created a pre and post processing portion to minimize manual work to nearly zero. 

## Pre-Processing
The Pre-Processing portion of this project involved 3 primary objectives. First, it needed to load in continuous data across multiple rhd (form of binary data) files and package them in a single binary .dat file for kilosort to use. It then needed to clean the data before sending them to kilosort as neural data can be prone to [movement artifact noise](https://radiopaedia.org/articles/motion-artifact-2?lang=us). It also needed to create traces of the raw data before and after cleaning so that its work could be checked independently by researchers.  To do so, it opens the first rhd file, creates figures for the data before cleaning, cleans the data, creates figures again after the cleaning, and writes the data to a binary file. It then opens the next file and repeats the entire process and writes the data to the same binary file. It  continues with this process until all of the rhd files have been processed and a single binary file remains.

### Data-Cleaning
Since a neuron will mostly appear on a single to a couple of channels, the cleaning of the data is done through taking the median of the data across multiple channels and zeroing out values that exceed some predetermined threshold. Given that the median is of enough channels, no neural data will be caught in the crossfire and all movement artifacts will be removed providing a much more clear picture of the neural signal.

## Post-Processing
