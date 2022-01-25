---
layout: post
title:  "TensorflowASR for People's Speech"
date:   2022-01-24 16:16:53 -0800
categories: blog
---
[People’s Speech](https://mlcommons.org/en/peoples-speech/) is among the world’s largest English speech recognition datasets licensed for academic and commercial use. It includes 30,000+ hours of transcribed speech in English with a diverse set of speakers and acoustic environments. This open dataset by [MLCommons.org](https://mlcommons.org/en/) is large enough to train speech-to-text systems and is available with a permissive license.

In this tutorial, we will learn to use this dataset to fine tune a pre-trained model. We’ll train this model locally and leverage pre-trained models released by [TensorflowASR](https://github.com/TensorSpeech/TensorFlowASR).


# 1. Install peoples-speech-tf-conformer
1. Clone the [peoples-speech-tf-conformer repo](https://github.com/sudnya/peoples-speech-tf-conformer) using the following command<br /><br />
 `git clone https://github.com/sudnya/peoples-speech-tf-conformer.git`<br /><br />
2. Create a new virtual environment using the following command<br /><br />
`python3 -m venv ~/environments-virtual/your-preferred-environment-name`<br /><br />
3. Activate the virtual environment created in the above step using the following command<br /><br />
`source ~/environments-virtual/your-preferred-environment-name/bin/activate`<br /><br />
4. Install the [peoples-speech-tf-conformer] as follows<br /><br />
`pip install git+git://github.com/sudnya/peoples-speech-tf-conformer`<br /><br />

# 2. The pretrained model
The [TensorflowASR repository](https://github.com/TensorSpeech/TensorFlowASR) provides a pre-trained model of the subword Conformer ready to be downloaded from [here.](https://drive.google.com/drive/folders/1VAihgSB5vGXwIVTl3hkUk95joxY1YbfW)
The [peoples-speech-tf-conformer repo](https://github.com/sudnya/peoples-speech-tf-conformer) contains the necessary files of this pretrained model [here.](https://github.com/sudnya/peoples-speech-tf-conformer/tree/master/peoples_speech_tf_conformer/pretrained_subword_conformer).

# 3. Download the dataset
#### 3.1 Download the manifest file
The manifest file can be directly downloaded from the [People’s Speech](https://mlcommons.org/en/peoples-speech/) website.

#### 3.2 Download the dataset
A small subset of the peoples speech dataset was downloaded by running the following command for a few minutes.<br /><br />
```wget 'https://the-peoples-speech-west-europe.bj.bcebos.com/part-00000-07a8f0d3-6d27-4299-887a-dc12a6d72f8d-c000.tar\?authorization\=bce-auth-v1/0ef6765c1e494918bc0d4c3ca3e5c6d1/2021-12-03T06%3A30%3A22Z/-1/host/444b9c082ceffd10f38bb965679ed9ec12202836831e111dd193fde281062d26'```

```curl 'https://the-peoples-speech-west-europe.bj.bcebos.com/part-00000-07a8f0d3-6d27-4299-887a-dc12a6d72f8d-c000.tar\?authorization\=bce-auth-v1/0ef6765c1e494918bc0d4c3ca3e5c6d1/2021-12-03T06%3A30%3A22Z/-1/host/444b9c082ceffd10f38bb965679ed9ec12202836831e111dd193fde281062d26'}```


# 4. Update the config file
An example configuration file can be found [here](https://github.com/sudnya/peoples-speech-tf-conformer/blob/master/peoples-speech-dataset-config.yml)<br />
1. Update the path to the vocabulary file under the `decoder_config` section to the file downloaded in the pretrained model step above.<br /><br />
`vocabulary: ~/path-to-the-pretrained-subword-conformer/conformer.subwords`<br /><br />
2. Update the path to the `corpus_files` in the `decoder_config` to the dataset file for the dataset mentioned in the [download the dataset section](#3. Download the dataset). Note that the dataset file is expected to be jsonlines. Here is an example line that represents the expected format<br /><br />
`{"audio_path": "/path-to-the-audio-file.mp4", "train": true, "test": false, "uid": "1a5fb807f32c6dbb1d3302793a6c55fe", "labeled": true, "label": "one two three", "image_path": "s3://can-be-empty.png", "label_path": "/path-to-the-/da0df04b6ad9112b72839d65a9e2966b.json" }`<br /><br />The label path file expects the following format
`{"label": "one two three"}`<br /><br />
3. Update the path to the `data_paths` in the `test_dataset_config` to point to the dataset file.

# 5. Run a pretrained model to evaluate on dataset
Run the pretrained model downloaded [in an earlier step](# 2. Download the pretrained model) in predict mode on the dataset downloaded in the [download dataset step](# 3. Download the dataset) with the the configuration file [described above](# 4. Update the config file).<br /><br />
`~> python peoples_speech_tf_conformer/run-peoples-speech.py --subwords`<br />

The defaults are assumed to be<br />
`--saved` pretrained model at the path `peoples_speech_tf_conformer/pretrained_subword_conformer/latest.h5`<br /><br />
`--config` file at the path `peoples_speech_tf_conformer/peoples-speech-dataset-config.yml`<br /><br />

You can override these with your local paths if desired.

# 6. Understanding the results
#### TODO

