# Temporal Fusion Transformers - overview
## About The Project
The repository contains code to understand and test the state-of-the-art architecture for Interpretable Multi-horizon Time Series Forecasting, proposed by Google in 2020. This mini-project was created for a recruiting assignment.

**Full article name**: Temporal Fusion Transformers for interpretable multi-horizon time series forecasting

**Authors**: Bryan Lim, Sercan Arik, Nicolas Loeff and Tomas Pfister

## Useful links
1. [Full paper available online](https://arxiv.org/pdf/1912.09363.pdf)
2. [Toward Data Science's article](https://towardsdatascience.com/temporal-fusion-transformer-googles-model-for-interpretable-time-series-forecasting-5aa17beb621)
3. [Project's GitHub](https://github.com/google-research/google-research/tree/master/tft)

## Code Organisation
- *google_repo* directory contains files directly copied from the official Google Research repository
- *data* consists of data files: both created with the help of the Google tutorial and generated manually (economicy/ directory)
- *manually_download_data* concerns manually downloaded data for the project and scripts formatting them, together with external data detailing the dates

Out of four available datasets prepared by Google for testing their solution, three were successfully downloaded to the hard drive (except for *favorita*), while only *volaitte* is available on GitHub due to its limitations (no more than 100 MB per file). Others have to be downloaded manually with the help of a tutorial available on the official repository.

## Recruitment task data

In order to examine TFT, three important factors were analyzed [together with their later abbreviated forms]:

a) **The Standard and Poor's 500** - SP500

b) **Gold Price in USD** - Gold

c) **EUR to USD Exchange Rate** - EURUSD

All indexes were downloaded from the [MarketWatch website](https://www.marketwatch.com/). Unfortunately, values got this way include NA values - mainly on weekends. Another limitation was the inability to download a larger range of data than one year - to this end, ten (for ten years) *.csv* files were manually downloaded for each of the three indexes, which were then processed and merged.

Each day and index consists of four numbers:

i) **open** - the value at the beginning of the day

ii) **high** - maximum value of the considered day

iii) **low** - minimum value of the considered day

iv) **close** - end-of-day value

## Preprocessing data

After downloading data, a few actions have been made in *manually_downloaded_data/format_manually_downloaded.ipynb* script. The code consists operations such as:
- connecting 30 manually downloaded files 
- having all the data in one Pandas DataFrame, containing all the dates since April 2nd, 2012 to April 1st, 2022
- generating summaries and visualizations, including correlation matrix and NA value information
- merging dataset containing all the available daily data from last 10 years with detailed dates' data frame

## Problems with Google repository
Original scripts cloned contains some bugs, mostly caused by changes in new versions of *TensorFlow*, for which the code has not yet been adapted. Therefore, in many places the code is modified to work as of April 2022. Among others, due to chenges in *TensorFlow*, in all the scripts importing package line:

```py
import tensorflow as tf
```

... was changed to:

```py
import tensorflow.compat.v1 as tf
```

... with addition of:

```py
tf.compat.v1.experimental.output_all_intermediates(True)
```

in a few scripts. Also, some packages had to be installed to old versions - eg. *scikit-learn* to 0.24.2
