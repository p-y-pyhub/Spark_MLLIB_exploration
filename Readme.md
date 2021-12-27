This repository will be used to store my notebook to explore the library MLLIB from Spark. 

The first project that I worked on was to produce a predictive model for the Kaggle Challenge : Malware detection. 
https://www.kaggle.com/c/malware-detection/overview

The data includes software signature seemingly gathered by Meraz'18 malware security partner Max Secure Software. There is a flag "Legitimate" indicating wether the software signature is of malicious nature.

In the overview of the kaggle challenge, it is mentioned : 

    "_Final evaluation of the candidates considered for the cash price which will be done manually by a jury, if your model predicts any legitimate files as malicious - strong penalty will be imposed which may lead you to lose the competition even if you rank well on kaggle score boards. Ensure that the false positive predictions are as low as 0.001%._"

The first thing I did was to explore the data. First observation made : the classed are well balanced ( ~ 60/40 split) which I find odd. Intuition tells me that there mmust naturally be more legitimate software than malicious ones in a specific ecosystem. This leads me to think that a first filter was apply to the data, a subsampling of the legitimate for example. 

Second observation : some MD5 hashes are repeated in the dataset. The majority of them are simple duplicates, although there are instances of incoherence accross specific variables, including the _legitimate_ flag. Some were removed because of that. 

Third observation : many variables are dominated by their modal value ( > 70%). For the majority of those variables, the values outside of the mode are simply irrelevant to the _legitimate_ flag. For the ones with significance, a simple binary binning was applied : modal value vs not modal value. Other kind of binning were suboptimal since the modal value would sit at "random" within the variable's domain. 

Fourth and final observation : many variables values are spread across multiple orders of magnitues ( 1 / 10 / 100 / 1000 / 100000 / etc.). These variables were LOG10 transformed. 

The data was split in Training (0.7), Test (0.2) and validation set (0.1).

A K-mean clustering was fitted to the training set and applied to it. Looking at the silhouette socres, the optimal number of clusters could have either been 3 or 8. The number 8 was chosen. The clusters were then ploted against the two first principal components of the data along side with the classes on the same axis. When comparing the two images, the clusters seemed relevant. 

Finally (for now) a GBTClassifier was applied to the data and produced an accuracy of 98,4% on the test set. Unfortunately, with the default threshold, the false positive rate obtained is 1,25 % way higher than what was expected. Nevertheless, the results are satisfying since it is an exploratory attempt and obtained on the very first try !
