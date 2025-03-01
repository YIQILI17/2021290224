# 2021290224
Cloud Computing Final Project
# GCI
参考论文： *Everything Has a Cause: Leveraging Causal Inference in Legal Text Analysis* (NAACL 2021 oral paper).

## Dependencies
 - Python>=3.7
 - Spacy (for word segmentation)
 - Numpy
 - Pandas
 - Sklearn
 - Pke (for keyword extraction)
 - Py-Causal (for causal discovery)
 - Networkx (for analyzing causal graphs)
 - Pydot (for saving causal graphs)
 - Dowhy (for causal inference)
 - Pytorch>=1.1
 - Gensim (for loading word vectors)
 - Pgmpy (for calculating BIC score)

## Prepare Data
The dataset used is provided in `data/data.zip`. 
Before running the models, some preprocessing is needed:
 - Unzip `data/data.zip` into `data/data.json`.
 - Download word embedding from https://ai.tencent.com/ailab/nlp/en/data/tencent-ailab-embedding-zh-d200-v0.2.0.tar.gz  and extract them into `data/`.
 - Run `preprocess.py` to prepare data for GCI.
 
(Optional) If you want to start from the raw data, please run:
 - Download original data from https://cail.oss-cn-qingdao.aliyuncs.com/CAIL2018_ALL_DATA.zip and extract them into `data/`.
 - Run `prepare_dataset.py` to build the dataset and store it in `data/data.json`.

## Fake News Detection Data
We have integrated a dataset for fake news detection. The dataset is based on GossipCop, which is proposed by FakeNewsNet. Some preprocessing has been done, and the dataset gossipcop_v3-2_content_based_fake.json is used for fake news detection.

The dataset includes two main fields: original_text and generated_text. The original_text contains the original legitimate news, and generated_text contains the modified fake news.

The statistics for the dataset are as follows:
Task Name：Content-based Fake	
Total：11941
The related images of the GossipCop dataset are also included. The name rule of the image is {news_id}_top_img.png, such as gossipcop-541230_top_img.png. Each task has a field named has_top_img to indicate whether the news has an image. 1 means the news has an image, and 0 means the news does not have an image.

Download datasets from this link:  [Google Drive](https://drive.google.com/drive/folders/1rLrh5x5UlYskfbhhVyz523MKgmCDyuX2?usp=sharing)

Run prepare_dataset.py to preprocess and prepare the dataset for GCI.


## Run GCI
```
python gci.py --charge DATASET_NAME --ratio TRAINING_DATA_RATIO
```
Argment `DATASET_NAME` is chosen from:

 - II-M-N (Personal Injury: Intentional Injury & Murder & Involuntary Manslaughter),
 - R-K-S (Violent Acquisition: Robbery & Kidnapping & Seizure),
 - F-E (Fraud & Extortion),
 - E-MPF (Embezzlement & Misappropriation of Public Funds),
 - AP-DD (Abuse of Power & Dereliction of Duty).
 
And `TRAINING_DATA_RATIO` is chosen from {0.01, 0.05, 0.1, 0.3, 0.5}.

## Integrate GCI with Neural Networks
This part of code is modified from https://github.com/649453932/Chinese-Text-Classification-Pytorch. As causal knowledge is needed, GCI should be executed first.
### Impose Strength Constraint
```
python run_nn.py --model BiLSTM_Att_Cons --charge DATASET_NAME --ratio TRAINING_DATA_RATIO
```
### Leverage Causal Chains
```
python run_nn.py --model CausalChain --charge DATASET_NAME --ratio TRAINING_DATA_RATIO
```

## Citation
Please cite our paper if this repository inspires your work.
```
@inproceedings{liu2021everything,
  title={Everything Has a Cause: Leveraging Causal Inference in Legal Text Analysis},
  author={Liu, Xiao and Yin, Da and Feng, Yansong and Wu, Yuting and Zhao, Dongyan},
  booktitle={Proceedings of the 2021 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies},
  pages={1928--1941},
  year={2021}
}
```

## Contact
If you have any questions regarding the code, please create an issue or contact the owner of this repository.
