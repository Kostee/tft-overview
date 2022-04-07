# Temporal Fusion Transformers - overview
## About The Project
The repository contains code to understand and test the state-of-the-art architecture for Interpretable Multi-horizon Time Series Forecasting, proposed by Google in 2020. This mini-project was created for a recruiting assignment.

**Full article name**: Temporal Fusion Transformers for interpretable multi-horizon time series forecasting

**Authors**: Bryan Lim, Sercan Arik, Nicolas Loeff and Tomas Pfister

## Useful links
1. [Full paper available online](https://arxiv.org/pdf/1912.09363.pdf)
2. [Toward Data Science's article](https://towardsdatascience.com/temporal-fusion-transformer-googles-model-for-interpretable-time-series-forecasting-5aa17beb621)
3. [Project's GitHub](https://github.com/google-research/google-research/tree/master/tft)
4. [Forecasting tutorial with *sktime*](https://www.sktime.org/en/stable/examples/01_forecasting.html)

## Code Organisation
- *google_repo* directory contains files directly copied from the official Google Research repository
- *data* consists of data files: both created with the help of the Google tutorial and generated manually (economicy/ directory)
- *manually_download_data* concerns manually downloaded data for the project and scripts formatting them, together with external data detailing the dates
- *saved_models* includes automatically generated files after running *script_train_fixed_params* script

Out of four available datasets prepared by Google for testing their solution, three were successfully downloaded to the hard drive (except for *favorita*), while only *volaitte* is available on GitHub due to its limitations (no more than 100 MB per file). Others have to be downloaded manually with the help of a tutorial available on the official repository.

## Recruitment task data

In order to examine TFT, three important factors were analyzed [together with their later abbreviated forms]:

a) **The Standard and Poor's 500** - SP500

b) **Gold Price in USD** - Gold

c) **EUR to USD Exchange Rate** - EURUSD

All indexes were downloaded from the [MarketWatch website](https://www.marketwatch.com/). Unfortunately, values got this way include NA values - fortunately mainly on weekends, on which they are naturally not available. Another limitation was the inability to download a larger range of data than one year - to this end, ten (for ten years) *.csv* files were manually downloaded for each of the three indexes, which were then processed and merged.

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
Original scripts cloned contains some bugs, mostly caused by changes in new versions of *TensorFlow*, for which the code has not yet been adapted. Therefore, in many places the code is modified to work as of April 2022.

### Version 0.1 - Sunday, April 3rd

Among others, due to chenges in *TensorFlow*, in all the scripts importing package line:

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

in a few scripts. Also, some packages had to be installed to old versions - eg. *scikit-learn* to 0.24.2. In the end it was again updated to the latest version, but this did not affect the result.

### Version 0.2 - Thursday, April 7th

New idea came to my mind - maybe old versions of TenserFlow are not compatible with part of the code? That's why I changed my idea.

Second *gooogle_repo was* copied, once again from original source. The old one was renamed to *google_repo_old* and in this variant I decided to import two TenserFlows package:

```py
import tensorflow as tf
import tensorflow.compat.v1 as tf1
```

In most of TenserFlows references I use the first one. *tf* was changed to *tf1* only in cases of error attribute errors, which were repaired by referencing to the old version.

However - the final effect ended up the same :(

```console
Traceback (most recent call last):
  File "C:\Users\koste\AppData\Local\Programs\Python\Python310\lib\runpy.py", line 196, in _run_module_as_main
    return _run_code(code, main_globals, None,
  File "C:\Users\koste\AppData\Local\Programs\Python\Python310\lib\runpy.py", line 86, in _run_code
    exec(code, run_globals)
  File "C:\Users\koste\Studia\10\JOB\7bulls\tft-overview\google_repo\script_train_fixed_params.py", line 234, in <module>
    main(
  File "C:\Users\koste\Studia\10\JOB\7bulls\tft-overview\google_repo\script_train_fixed_params.py", line 153, in main
    val_loss = model.evaluate(valid)
  File "C:\Users\koste\Studia\10\JOB\7bulls\tft-overview\google_repo\libs\tft_model.py", line 1191, in evaluate
    raw_data = self._batch_data(data)
  File "C:\Users\koste\Studia\10\JOB\7bulls\tft-overview\google_repo\libs\tft_model.py", line 765, in _batch_data
    data_map[k] = np.concatenate(data_map[k], axis=0)
  File "<__array_function__ internals>", line 180, in concatenate
ValueError: zero-dimensional arrays cannot be concatenated
```

Related issue: https://github.com/google-research/google-research/issues/801

The above discussion suggests that the problem is in the *split_data* function from the Economy DataFormatter, whereas I don't think that is the problem.

## Other users' codes

Despite forking the Google repository by several thousand users, I only found any changes to the */tft* directory itself in a few cases. None of these, however, contained an implementation of any other than four of the proposed datasets...

However, it turned out that the code itself was implemented in a different way! The solution was at least prepared by user **KalleBylin**, as Master of Data Science ML Project. His solution which is available in one *.ipynb* file and seems to work, can be found at [this link](https://github.com/KalleBylin/temporal-fusion-transformers). I included it also in this repositorium, named *KatteBylinSolution.ipynb* in the main directory.
