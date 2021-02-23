# U-GPD Transfer Learning model for detecting seismic phase arrivals at Nabro volcano
[![DOI](https://zenodo.org/badge/314559222.svg)](https://zenodo.org/badge/latestdoi/314559222)

This repository contains two notebooks to accompany the paper **_'A Little Data Goes A Long Way: Automating Seismic Phase Arrival Picking at Nabro Volcano with Transfer Learning'_**, currently under consideration for publication by [Journal of Geophysical Research: Solid Earth](https://agupubs.onlinelibrary.wiley.com/journal/21699356). Preprint is available on [ESSOAR](https://www.essoar.org/) at: (_link coming soon_).

The first notebook ([1_UGPD_training.ipynb](https://github.com/sachalapins/U-GPD/blob/main/1_UGPD_training.ipynb)) demonstrates how to isolate the feature extraction system from an existing, extensively-trained deep learning model for seismic phase arrival detection (GPD model; Ross et. al, 2018, BSSA, https://doi.org/10.1785/0120180080) and use it as a building block in a new, all-convolutional model commonly referred to as a U-Net (hence 'U-GPD': GPD feature extraction weights within a U-Net architecture). By replacing the original model's fully-connected layers with convolutional layers, we greatly reduce the number of trainable model parameters (ideal for smaller training sets), more precisely label ground truth phase arrivals (which subsequently leads to more precise phase arrival time predictions) and improve computational efficiency when applied as a sliding window over continuous sections of data.

The second notebook ([2_UGPD_run.ipynb](https://github.com/sachalapins/U-GPD/blob/main/2_UGPD_run.ipynb)) demonstrates how to run our best, trained transfer learning model from the first notebook over 24 hours of seismic data from Nabro volcano and locate events using our model's phase arrival picks and [NonLinLoc](http://alomax.free.fr/nlloc/).

Both notebooks are designed to be run in Google Colab and only require that you have a Google account. Sometimes notebooks fail to render on GitHub, so you won't be able to view or run them if that happens. This is a well-known problem among GitHub users that appears to happen sporadically. If you can't see the notebooks for whatever reason, try again at a later time and they should hopefully work okay.

If any of the code or results presented are useful then please consider citing the paper above (once a DOI is available) or the archived version of this repository from Zenodo.

The training, validation and test sets can be downloaded from Zenodo at https://doi.org/10.5281/zenodo.4498549.

For any issues, questions or suggestions, please contact me on [sacha.lapins@bristol.ac.uk](mailto:sacha.lapins@bristol.ac.uk).


### Work summary:

Nabro, a large caldera on the Eritrea-Ethiopia international border, erupted unexpectedly for the first time in recorded history on 12th June, 2011 (the volcano was previously thought to be inactive). It was a significant eruption, disrupting aviation across north Africa and causing a significant humanitarian crisis, with around 12,000 people displaced. At least seven people were tragically killed. The eruption also injected a vast amount of SO<sub>2</sub> into the atmosphere, one of the largest eruptive SO<sub>2</sub> masses globally since the eruption of Mount Pinatubo in 1991.

Eight broadband seismometers were deployed around the volcano following the eruption and remained operational for 14 months. The challenge with processing this seismic dataset is that it is extremely time-consuming to manually identify seismic events (i.e., volcanic earthquakes) and phase arrivals (P- and S-waves), and most automated approaches are either crude or computationally expensive. Supervised deep learning models have proven to be an effective and efficient alternative for automating earthquake and seismic phase arrival detection, with several published works using California or global earthquake catalogues as test cases. However, these models, which often have millions of tunable parameters, generally require large amounts of labelled data for training. Such large, annotated datasets are obviously not available for previously unmonitored volcanoes.

Transfer learning allows us to use the knowledge acquired by existing, extensively trained deep learning models and apply it to similar tasks for which we have much smaller training sets. In this work, we leverage the generalised knowledge of seismic signal features learned by a model trained on millions of seismic waveforms from southern California and fine-tune it to detect volcanic earthquakes following the eruption at Nabro volcano using a training set that is three orders of magnitude smaller (~ 5,000 waveforms as opposed to 4.5 million). Transfer learning yields less overfitting than training the same model from scratch with this small dataset. Furthermore, by changing model task from classification to segmentation, our transfer learning model yields better classification accuracy and smaller phase arrival time residuals than the original base GPD model when applied to data from Nabro. The full 14 month dataset can be processed in a few hours using just a single GPU, making this approach very well-suited to real-time volcano-seismic monitoring and re-processing large historical datasets.
