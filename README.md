# Anomaly-Detection by Feature Segregation
This work is on developing an algorithm embedded on simple parametric and non-parametric machine learning algorithms that makes them perform better even in the presence of anomalies.

# Features of the algorithm
1. The algorithm splits the datset based on anomalies into subsets and applies seperate parametric or non-parametric algorithms depending on the type of anomaly detected. 
2. Any parametric model can be used as a non-parametric model by embedding the Feature Segregator.
3. This vastly reduces the inherent bias error of the parametric model and translates into the variance error. 
4. The architecture of the algorithm is structured in a way to have a balance between the bias and variance error and also minimise it as much possible. 
5. The irreducible error in a model comes due to the improper definition of the problem statement and the failure to look a unknown features that have an impact on the observation. But, these features may not impact all the observations at once. This is the reason behind the abnormalities and anomalies. 
6. Upon mathematical analysis, it was found that the error function was a max(variance error, irreducible error) for the algorithm.
7. From the results of embedding the algorithm on various parametric and non-parametric models, it was found that the algorithm outperforms almost every other ML algorithms by producing an average test accuracy: 0.96 and average training accuracy: 0.98 without overfitting the data, with very minimal variance in the output predictions.
8. The algorithm also proved to be worthy in case the data was sourced from different sources with no clue on how the data was collected or generated. The classification into subsets and training of each subset on different micro classifiers, automatically classified the different sources into different subsets regardless of the class labels. This is mainly achieved by making any model non-parametric.

# Terms to be aware of:
1. Normal Observations: This type of observation occurs most frequently. The difference to be understood is that this class label has no correlation to the final classification or regression problem, unless the only motive is to detect the anomalies in the dataset.
For example, in a brain MRI dataset, a clear MRI corresponds to a normal observation. It says nothing about the information contained in the MRI. In other words, it just states that this is how a normal MRI should look like, rather than this is how a normal AD patient's MRI should look like. 
2. Abnormal Observation: This type of observation occurs not that often, but is important enough to not be ignore. These set of observations will not be spanned by the set of normal observations, but it is likely to come across such observations in reality.
For example, if a room temperature sensor has a average observation of 27 degree C during the day of any month, it is abnormal to see a observation like 16 degree C or 40 degree C on any regular day. But it can't be neglected that it is a possibility to occur.
3. Anomaly Observation: This type of observation mostly occurs due to some inherent error in the measuring device and are safe to ignore unless specified otherwise. 
In the same example above, if the temperature goes to 0 degree C or 100 degree C, it is safe to assume that there is an issue with the device and neglect any such observation. But in the case of the brain MRI, it is rare to get a blurry image, but any piece of medical data will almost always provide some insight which is not advicable to neglect.

# Overview of the Steps Involved:
1. Divided the dataset depending on the max(class labels) into subsets using One-Class SVM to two subsets - majority and minority subsets.
For example, if the dataset has a distribution of class label 1:500; class label 2:150; class label 3: 100; then train the OC-SVM with just the class label 1(has the maximum distribution) and predict new labels for the entire dataset. Append the original class labels to the feature space of the dataset. This will detect any abnormalities and anomalies in the dataset. The finer detection is done in the subsequent steps.
2. Using a clustering algorithm of choice, divide each of the new subsets into three subsets - Normal Subset, Abnormal Subset, Anomaly/ Outlier subset. Traditionally, it should follow the distribution pattern as 0.9 for Normal Subset, 0.09 for Abnormal Subset, 0.01 for Anomaly Subset. This number can change depending on the application. It is adviced to find the legacy distribution pattern from a subject-matter expert. 
3. Each of these subsets are further divided into test and train sets. Each type of subset is trained on a seperate model. The order matters. If a Abnormal Subset is trained over a model that was previous trained by either normal or anomaly subset, it will mess up the tuning parameters and result in bad predictions. So, it is important to train each type of subset seperately on each model chosen. 
4. Repeat the steps seperately for majority and minority sets.
5. The results of all the individual micro classifiers are aggreagated to give the final prediction of the dataset. 


With due respect to the complaince agreement, no part of the code or the dataset that I used, can be hosted in any public repository. 
Sorry for the inconvenience, but the overview of the algorithm is just enough to program it in any language. 

The inspiration for this algorithm was derived from the work done by Sašo Karakatic et al. on 'Improved classification with allocation method and multiple classifiers'.
