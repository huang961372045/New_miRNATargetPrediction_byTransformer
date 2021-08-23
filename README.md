# New_miRNATargetPrediction_byTransformer
This is a new deep learning based method for miRNA target prediction

1. About the method

I have developed a miRNA target prediction method in which hybrid model of CNN and Tranformer are implemented.

2. Data collection and processing

The original data is completely copied from article "miRAW: A deep learning-based approach to predict microRNA targets by analyzing whole microRNA transcripts".


      a. Train Dataset: ~/PLOSComb/Data/CrossVal/RelaxedPW60_14K_35K/set_{i}_train.csv, i∈[0,9]
      b. Test Dataset (site level): ~/PLOSComb/Data/CrossVal/RelaxedPW60_14K_35K/set_{i}_test.csv, i∈[0,9]
      c. Test Dataset (gene level): ~/PLOSComb/Data/TestData/balanced10/randomLeveragedTestSplit_{i}.csv, i∈[0,9]


Details of data set division: Due to the imbalance of the samples in the experimental data, the original text down-sampled the positive samples and grabbed the possible binding sites in the negative sample sequence. Finally, 33142 positive samples and 32284 negative samples were obtained for training.  More details on data processing can be obtained in the original text


Attention:
      The input data of the model is a pair of miRNA::mRNA sequences, where the length of the mRNA is 40nt and the length is fixed. 
Therefore, it is necessary to manually cut the mRNA sequence obtained at the gene level. Different cutting pieces from the same mRNA sequence form samples from the unique miRNA. When judging the label of miRNA and mRNA, the cut samples are OR relationship with each other.
 
 

3. Model construction


Input (miRNA_sequence+padding+mRNA_sequenc+padding) -> Embedding -> 1dcnn -> ReLU -> Maxpooling -> Transfomer(Only Encoder) -> Fully connected layer(2 layers) -> ReLU -{SoftMax}-> probability of 0/1 class



4. Model evaluation


=================Compare with miRAW=================


a. Site level (10 Fold Cross Val, 14000 sample, pos : neg = 1:1)	

![image](https://user-images.githubusercontent.com/49811864/130411016-9b76f7d6-4697-4267-976e-07584433e118.png)

b. Gene level (548 pos, 548 neg; 10 Fold Cross Val)		

![image](https://user-images.githubusercontent.com/49811864/130411180-cf923ee0-f29d-4e7b-80e9-a8db376a0fc3.png)



=================Compare with benchmark=================


a. genelevel_10 fold_deepTarget:

![image](https://user-images.githubusercontent.com/49811864/130411427-31dff93d-3290-403d-970f-ef5875501c40.png)

b. genelevel_10 fold_new model 

![image](https://user-images.githubusercontent.com/49811864/130411550-618c2552-d463-46b7-8780-14d8c753c5b2.png)



5. conclusion

In this study, my model followed miRAW's article rules and used its data to achieve better results. The key structure of the model is the feature extractor Transformer of which role is to allow the model to selectively pay attention to the key information. In this case, it is the sequence interaction information between miRNA and mRNA. 

But the model did not solve the problem of good prediction effect at isite level and bad prediction at gene level. 
The reason lies in two points. The first method of artificially segmenting the gene sequence introduces noise, and the second model is unreasonable in the process of judging the sample label (in vivo, only combining with an accurate position can lead to gene down-regulation)

In addition, there is room for improvement in the construction of the model. The first is input. The sine and cosine information added to the Transformer can distinguish the same nucleotide in different positions, but it lacks the distinction between miRNA and mRNA.

6. Discussion
 
The next step is to try out three directions. The first is to try Multi-Instance Learning. The training data uses gene-level data and cuts it by my rules. The purpose is to train at the gene level to predict at the gene level; The second point is to change the input method. Use the full-length mRNA sequence without additional cutting; the third point, build a new model, first use the miRNA sequence to input into the current model to get the probability value P1, and then only use the mRNA sequence to input into the current model to get the probability value P2. Combined values of P1 and P2 decide the predicted label of the model. The purpose is to detect whether the training set is biased.
