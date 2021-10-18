# New miRNA Target Prediction method based on deep learning model 


## 1. background：

### (1) definition and application of miRNA

microRNAs (miRNAs) are endogenous to 22 nt RNAs that combine target genes to cut targeted genes or inhibit the translation of targeted genes.

miRNA plays an important role in gene expression in the human body, including regulating metabolism, cell proliferation, immune system activation and other functions.

![image](https://user-images.githubusercontent.com/49811864/137681155-2a2cbeaf-c727-4580-bac9-9f7af24598bb.png)
source：https://interna-technologies.com/

miRNA is a chain randomly selected in the body by the precursor of the hairpin structure to bind to the AGO representative, and then target gene to regulate gene expression, there are many applications for the biological function of miRNA, such as biomarkers for early cancer detection, patient recruitment and treatment options. In addition, miRNA also has the potential for new drugs called personalized treatment of cancer patients. In these applications, the key is to find the target of miRNA, so it is important to accurately predict the target gene of miRNA.

![image](https://user-images.githubusercontent.com/49811864/137681005-27d00d81-dfee-45a2-aad1-308064e00a57.png)

(Sempere L F, Azmi A S, Moore A. microRNA‐based diagnostic and therapeutic applications in cancer medicine[J]. Wiley Interdisciplinary Reviews: RNA, 2021: e1662.)

### （2）feature of miRNA-gene combination

There are two main problems that need to be solved in the prediction of miRNA targets, gene level and site level. The problem that Gene level needs to solve is which gene's mRNA is targeted and regulates its expression, and the problem that site level needs to solve is where to target this mRNA sequence. It is necessary to achieve some precision by predicting the site level in order to predict whether miRNA regulates gene expression.

Through the study of miRNA targeting mechanisms from the last century, different species of miRNA evolved in a conservative region, in miRNA 5' to 3' ends 2 to 8 base lengths for a sequence of 7 nucleotides. This sequence, known as the seed region, has a pre-determined role in the miRNA regulation gene, and its sequence must be combined to mRNA in order to play a role in regulating gene expression. Experiments have shown that the seed region of miRNA has two kinds of rule matching and non-rule matching. Rule matching is a match that humans find to have certain rules to follow, rather than non-rule matching is a general term for a series of matches that appear mismatched, swollen, misaligned, etc. in the match within the seed region.
rule matching：8-mer, 7-mer-m8, 7-mer-A1, 6-mer, 6-mer-A1, offset-7-mer, offset-6-mer

![image](https://user-images.githubusercontent.com/49811864/137681916-1d0c469e-750b-43e8-b088-5bfc403e2460.png)

(Ellwanger D C, Büttner F A, Mewes H W, et al. The sufficient minimal set of miRNA seed types[J]. Bioinformatics, 2011, 27(10): 1346-1350.)

In addition to seed regions, miRNA target genes have other features, such as binding free energy, the degree of 13-16 bit matching, the sequence conservativeness of miRNA, the exposure of the target point, and so on.

![image](https://user-images.githubusercontent.com/49811864/137681821-6f076980-9930-4dfb-b989-3be787c77a52.png)

(Schäfer M, Ciaudo C. Prediction of the miRNA interactome–Established methods and upcoming perspectives[J]. Computational and structural biotechnology journal, 2020, 18: 548-557.)

### （3）Existing Method

![image](https://user-images.githubusercontent.com/49811864/137683010-2a41ff48-3131-4d6c-9427-c3c383553d9b.png)

### (4) Goal

1) Problem : Human-designed features have the limitation because it is based on the cognition of existing miRNA-gene bind and single deep learning model that predicts accuracy at the site level of negative samples is not high

2) Solution: Using RNA sequence as input to establish a deep learning model, automatically extract hidden features to predict potential sites, and establishing a more accurate model to improve accuracy of gene level prediction.


## 2. method：

### (1) Data collection and processing

The original data is completely copied from article "miRAW: A deep learning-based approach to predict microRNA targets by analyzing whole microRNA transcripts" (Pla A, Zhong X, Rayner S. miRAW: A deep learning-based approach to predict microRNA targets by analyzing whole microRNA transcripts[J]. PLoS computational biology, 2018, 14(7): e1006185).

![image](https://user-images.githubusercontent.com/49811864/137684431-0b12794e-21fd-4c8a-ba44-10248d079063.png)


      a. Train Dataset: ~/PLOSComb/Data/CrossVal/RelaxedPW60_14K_35K/set_{i}_train.csv, i∈[0,9]
      b. Test Dataset (site level): ~/PLOSComb/Data/CrossVal/RelaxedPW60_14K_35K/set_{i}_test.csv, i∈[0,9]
      c. Test Dataset (gene level): ~/PLOSComb/Data/TestData/balanced10/randomLeveragedTestSplit_{i}.csv, i∈[0,9]


Details of data set division: Due to the imbalance of the samples in the experimental data, the original text down-sampled the positive samples and grabbed the possible binding sites in the negative sample sequence. Finally, 33142 positive samples and 32284 negative samples were obtained for training.  More details on data processing can be obtained in the original text

![image](https://user-images.githubusercontent.com/49811864/137685362-4bfa7bbb-cc28-4ba3-8d70-85da2f145edf.png)



Attention:
      The input data of the model is a pair of miRNA::mRNA sequences, where the length of the mRNA is 40nt and the length is fixed. 
Therefore, it is necessary to manually cut the mRNA sequence obtained at the gene level. Different cutting pieces from the same mRNA sequence form samples from the unique miRNA. When judging the label of miRNA and mRNA, the cut samples are OR relationship with each other.
 
 

### (2) Model construction

Transformer-based


### (3) Model evaluation

#### Compare with miRAW
(miRAW: Pla A, Zhong X, Rayner S. miRAW: A deep learning-based approach to predict microRNA targets by analyzing whole microRNA transcripts[J]. PLoS computational biology, 2018, 14(7): e1006185)

a. Site level (10 Fold Cross Val, 14000 sample, pos : neg = 1:1)	

![image](https://user-images.githubusercontent.com/49811864/137686356-9c8b7fd6-80a4-4325-b6e3-e88458ed926c.png)

b. Gene level (548 pos, 548 neg; 10 Fold Cross Val)		

![image](https://user-images.githubusercontent.com/49811864/130411180-cf923ee0-f29d-4e7b-80e9-a8db376a0fc3.png)

#### Compare with other deep learning mehods deepTarget and miTAR
(deepTarget: Lee B. Deep Learning-Based microRNA Target Prediction Using Experimental Negative Data[J]. IEEE Access, 2020, 8: 197908-197916.; Gu T, Zhao X, Barbazuk W B, et al. ; miTAR: miTAR: a hybrid deep learning-based approach for predicting miRNA targets[J]. BMC bioinformatics, 2021, 22(1): 1-16.)
a. genelevel_10 fold_deepTarget:

![image](https://user-images.githubusercontent.com/49811864/130411427-31dff93d-3290-403d-970f-ef5875501c40.png)

b. genelevel_10 fold_new model 

![image](https://user-images.githubusercontent.com/49811864/130411550-618c2552-d463-46b7-8780-14d8c753c5b2.png)


5. Variant model valuation and selection of Hyperparameters


6. Case study


7. conclusion

In this study, my model followed miRAW's article rules and used its data to achieve better results. The key structure of the model is the feature extractor Transformer of which role is to allow the model to selectively pay attention to the key information. In this case, it is the sequence interaction information between miRNA and mRNA. 

But the model did not solve the problem of good prediction effect at isite level and bad prediction at gene level. 
The reason lies in two points. The first method of artificially segmenting the gene sequence introduces noise, and the second model is unreasonable in the process of judging the sample label (in vivo, only combining with an accurate position can lead to gene down-regulation)

In addition, there is room for improvement in the construction of the model. The first is input. The sine and cosine information added to the Transformer can distinguish the same nucleotide in different positions, but it lacks the distinction between miRNA and mRNA.

8. Discussion
 
The next step is to try out three directions. The first is to try Multi-Instance Learning. The training data uses gene-level data and cuts it by my rules. The purpose is to train at the gene level to predict at the gene level; The second point is to change the input method. Use the full-length mRNA sequence without additional cutting; the third point, build a new model, first use the miRNA sequence to input into the current model to get the probability value P1, and then only use the mRNA sequence to input into the current model to get the probability value P2. Combined values of P1 and P2 decide the predicted label of the model. The purpose is to detect whether the training set is biased.
