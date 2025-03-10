# Code Pipeline
Here you will find all Python scripts created and used for this project. In this readme, you can read about their meaning, usage and restrictions. 


## Set-Up and Instructions

### Tested Pre-Requisites
- Memory: 8 GB
- `Windows 10` and `Arch Linux`
- [Python 3.7](https://www.python.org/downloads/release/python-370/)

### Installation
- Clone Repo
- Install Dependencies either through [pip](https://pip.pypa.io/en/stable/) or [anaconda](https://docs.anaconda.com/)
  - [fasttext](https://fasttext.cc/)
  - [numpy](https://numpy.org/doc/)
  - [sklearn](https://scikit-learn.org/)
  - [nltk](https://www.nltk.org/)

### Usage

In general the scripts run by taking a input and output argument as follows:

    $ python <SCRIPT_NAME> <INPUT_DATASET.txt> <OUTPUT.json>

> Detailed example commands for each script can be found below

# Scripts
>Click on the headers to inspect the linked scripts / folders.

## [Classifiers](./classifiers)
This folder is home to two fasttext classifiers. Both are tested with a fasttext data set text file as input and generate a json containing the results of a 10-fold validation. The meaned results are in "results" and in "details" you can see the folds themselfes.

### [Stock Classifier](./classifiers/classifier.py)

`classifier.py` is a stock classifier, the calculated Percision/Recall/F1 is generated by the `model.test()` function, which differs from the evaluation used in [ticket-tagger](https://github.com/rafaelkallis/ticket-tagger/tree/master/src). To run this script if could be needed to create a folder called "tmp" (with `mkdir ./tmp/`)

    $ python classifier.py <INPUT_DATASET.txt> <OUTPUT.json>

### [Multi-Label-Binary Classifier](./classifiers/ml_bin_classifier.py)

`ml_bin_classifier.py` on the other hand, uses a hand crafted multi-label-binary approach. It takes a **normal** fasttext data set as input and uses the [create_binary_datasets.py](./data_acquisition/create_binary_datasets.py) script to split it into 3 binary ones. During this process, temporary files are created and deleted. The structure of the output file is similar to the other classifier.
 >Both classifiers use a fasttext text file data set as input and produce a json file with the results of the 10-fold validation.

    $ python ml_bin_classifier.py <INPUT_DATASET.txt> <OUTPUT.json>

## [Data Acquisition](./data_acquisition)

In this folder resides a collection of scripts to facilitate the process of data acquisition. 
The idea is to first use the `dump_issues` script to scrape GitHub for issues and get a json file.
In order to translate this json dump into a fasttext data format, use the `json2fasttext` script.
If you need to split this dataset into a binary dataset, translate any multi-label fasttext data with the help of the `create_binary_datasets` script.
If you want to use WEKA/MEKA, you will need to use the ARFF-Conversion scripts.  


### [Github Scraper](./data_acquisition/dump_issues.py)

 >Make Sure to edit the variables `username` and `token` in the script to match your GitHub API credentials before using the script, as it will not work otherwise.
        
    $ python dump_issues.py <OWNER_NAME> <REPO_NAME>

### [Fasttext Converter](./data_acquisition/json2fasttext.py)
    $ python json2fasttext.py <INPUT.json> <OUTPUT.txt>

### [Binary Data Set Splitter](./data_acquisition/create_binary_datasets.py)
    $ python create_binary_datasets.py <INPUT.json> <OUTPUT.txt>


### [Multilabel ARFF Conversion](./data_acquisition/arffConverter_Multilabel.py)

Creates an arff data set, in which exactly one label has to be classified
(either bug, enhancement or question). Use the commands below in the directory of the converters.

```
$ python arffConverter_Multilabel.py <INPUT.txt> <OUTPUT.arff>
```
> A `StringToWordVector filter` (found in the preprocessing section of the WEKA or MEKA GUI) must be applied to the 
output files, because this preprocessing step is not done automatically.


### [Binary Relevance ARFF Conversion](./data_acquisition/arffConverter_BinaryRelevance.py)
The label we used in WEKA is split up, such 
that each possiblity is its own label (0 or 1).

```
$ python arffConverter_BinaryRelevance.py <INPUT.txt> <OUTPUT.arff>
```
> A `StringToWordVector filter` (found in the preprocessing section of the WEKA or MEKA GUI) must be applied to the 
output files, because this preprocessing step is not done automatically.

## [Stemming](./stemming)
Stemming is the process of reducing words to its stem or root (e.g., swims, swimming, and swam are reduced to swim). Some machine learning models perform better if we use the same data for words that are similar. So stemming is a simple technique to preprocess text-based data in order to have a better model performance.

### [Porter Stemming](./stemming/porter_stemming.py)
The porter stemming algorithm is a well known technique in computer linguistics for stemming words. The algorithm is based on the idea to shorten a word with the objective to minimize the amount of syllables. Below you find the required python modules to run the script and you will also find an example.


```
$ python porter_stemming.py <INPUT.txt> <OUTPUT.txt>
```

### [Snowball Stemming](./stemming/snowball_stemming.py)
The snowball stemming algorithm is an improved version of the porter stemming algorithm. The key differences between those two algorithms are the following: The snowball stemmer is more agressive than the porter stemmer and the snowbal algorithms stems more accurately (e.g., fairly -> fair vs. fairly -> fairli).


```
$ python snowball_stemming.py <INPUT.txt> <OUTPUT.txt>
```

### [Stopword Removal](./stemming/stopword_removal.py)
Stopwords, words that are often used such as 'the', 'a', 'in' or 'in' can introduce some unnecessary noise to the data which do not help to model the classification. In order to increase the performance a stopord removal step can help. The algorithm to remove such stopwords is quite simple since it will check each word in the data if it is contained in a predefined list of known stopwords.


```
$ python stopword_removal.py <INPUT.txt> <OUTPUT.txt>
```

