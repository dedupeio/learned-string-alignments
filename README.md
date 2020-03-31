# Learning String Alignments for Entity Aliases #

Part of the [Dedupe.io](https://dedupe.io/) cloud service and open source toolset for de-duplicating and finding fuzzy matches in your data.

Paper can be found here: http://www.akbc.ws/2017/papers/28_paper.pdf

## Dependencies ##
Python 3.5\
anaconda\
Pytorch 0.2\
numpy 1.13.3\
Matplotlib 2.0.2


## Setup ##

In each shell session run, `source bin/setup.sh` to set environment variables.

## Training Models ##

To train a model, first create a config JSON file (sample file for AlignCNN at `config/wiki/align_cnn_wiki.json`).

Then, you can run the `bin/train/train.sh` script with the config as an argument to train the model. 
Wrapper training scripts for the models are:

```

# AlignCNN
# Config example file: config/wiki/align_cnn_wiki.json
# Training script example: 
sh bin/train/wiki/train_align_cnn_wiki.sh


# AlignBinary
# Config example file: config/wiki/align_binary_wiki.json
# Training script example: 
sh bin/train/wiki/train_align_binary_wiki.sh

# AlignLinear
# Config example file: config/wiki/align_linear_wiki.json
# Training script example:
sh bin/train/wiki/train_align_linear_wiki.sh

# AlignDot
# Config example file: config/wiki/align_dot_wiki.json
# Training script example: 
sh bin/train/wiki/train_align_dot_wiki.sh

# Specify which GPU to run on (CUDA_VISBILE_DEVICES) as (for gpu 0)
sh bin/train/wiki/train_align_cnn_wiki.sh 0 
```

Currently the code must be run on the GPU. We will soon support CPU training.

## Evaluating Models ##

To run evaluation on the test set, locate the best model on the dev set. The following two scripts will output the 
commands to run to score the models on the test set.

```bash
sh bin/util/find_all_json_scores.sh exp_out > all_dev_scores.json
python -m entity_align.utils.FindBestModels all_dev_scores.json
```

You can collect the test results with

```bash
sh bin/util/find_all_json_test_scores.sh exp_out > all_test_scores.json
```

## Dataset Format ##

Training is done by a ranking loss computed over triples of `<source string, true positive, true negative>`. These three aliases are tab separated and each example is on its own line.

```
William Paget, 1st Baron Paget  William Lord Paget      William George Stevens 
William Paget, 1st Baron Paget  William Lord Paget      William Tighe  
William Paget, 1st Baron Paget  William Lord Paget      Edward Paget   
William Paget, 1st Baron Paget  William Lord Paget      The Earl of Uxbridge   
William Paget, 1st Baron Paget  William Lord Paget      East Francia   
William Paget, 1st Baron Paget  William Lord Paget      Irish  
```

Test performance is evaluated by IR metrics such as MAP / Hits at K. The format is `<source string, candidate string, label (1 if candidate string is alias of source string and 0 if it is not)>`. These are also tab separated.

```
peace agreement peace negotiation       1      
peace agreement interim peace treaty    1      
peace agreement Peace Accord    1      
peace agreement peace treaty    1      
peace agreement Peace and Truce 0      
peace agreement Treaty of Apamea        0      
peace agreement Peace and conflict studies      0      
peace agreement peace-keepers   0
```


## Datasets ##

Please contact us for the datasets. Links to come soon.
