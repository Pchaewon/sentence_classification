## [DACON COMPETITION : 문장 유형 분류 AI 경진대회](https://www.dacon.io/competitions/official/236037/overview/description)<br>
[**Competition Abstract**] <br>
   A model development competition to classify the type, tense, polarity, and certainty of input sentences.<br>
  
### 1. Dataset <br>
A total of 3 csv files are provided in the competition. <br>
 1) train.csv 
    - ID : Unique ID per sample sentence
    - 문장 : One sentence per sample
    - 유형 : Type of sentence
    - 극성 : Tense of sentence
    - 시제 : Polarity of sentence
    - 확실성 : Certainty of sentence
    
 2) test.csv 
    - ID : Unique ID per sample sentence
    - 문장 : One sentence per sample
    
 3) sample_submission.csv -> Submission file
    - ID : Unique ID per sample sentence
    - Label : Predicted Sentence Class

We used re-partitioning at a ratio of 8:2 to create train and val files for the provided training data.<br>
```bash
train, val, _, _ = train_test_split(df, df['label'], test_size=0.2, random_state=CFG['SEED'])
```
To do data pre-processing, we first vectorized sentences and then proceeded with label encoding.
After sentence vectorization using TfidfVectorizer, the shape examined is as follows.
```bash
print(train_vec.shape, val_vec.shape, test_vec.shape)
# (13232, 9351) (3309, 9351) (7090, 9351)
```
