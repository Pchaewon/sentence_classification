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

### 4. KOBERT <br>
Before explaining [KOBERT](https://github.com/SKTBrain/KoBERT), I will explain BERT. BERT stands for Bidirectional Encouragement Representations from Transformer, and is a model for processing natural languages by checking text in both directions. And since it is an open source developed by Google, it has the advantage that anyone can use a good performance model.
However, since BERT was pre-trained in English, it is difficult to apply Korean. Therefore, SKT Brain team developed a Korean version of the natural language processing model.   <br>
******
The following attempts were made to apply KOBERT to the competition. <br>
1. Transform the Label Encoder by type <br>
2. Concatenate the sentence with Label Encoder and put it as input <br>
3. We judged that the four types had no covariance and used their own separate models. <br>
4. Cross Entropy was used as Loss, and the weight was passed by class to improve performance by adjusting the degree of learning according to the data ratio <br>
5. Using AdamW as Optimizer <br>
6.	Use cosine_schedule_with_warmup as scheduler <br>
 <img width="678" alt="aa" src="https://user-images.githubusercontent.com/77375401/209630110-f5e9e91b-c9ce-4a9a-bddc-da3986cc9c2e.png">
7. The training performance is evaluated by separating the validation set from the training set <br>

### 6. Winner's method
This method is the method of the team that won this competition. <br>
- Citeation : <https://github.com/HaloKim/competitions> <br>
1.	 Text Augmentation <br>
-	replaced 2 words in one sentence and made 2 or 3 sentences.(randomly) <br>
2.	Ensemble <br>
3.	Customize model layers <br>
-	Residual Blocks <br>
4.	Change loss function <br>
-	[Focal Loss](https://github.com/Alibaba-MIIL/ASL/blob/main/src/loss_functions/losses.py) <br>
-	[Asymmetric Loss](https://paperswithcode.com/paper/asymmetric-loss-for-multi-label) <br>
5.	Fold(just 5 folds) <br>
6.	Huggingface Custom Trainer <br>



