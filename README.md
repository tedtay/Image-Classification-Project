# Image-Classification-Project
## __**Introduction**__
Image recognition has a vast array of applications: facial recognition, google lens and image organisation are just a few of the possibilities. The technology driving image recognition is improving rapidly as the technology begins to become more prevalent in our daily lives. 
There are various approaches that can be taken when implementing image recognition, there are unsupervised methods such as K-Means and Gaussian clustering, supervised methods like Regression, Support Vector Machines and Neural Networks as well as dimensionality reduction to gain deeper insight into the structure of the data.

## __**Methodology**__
To carry out these methods, appropriate feature extraction must take place. This will be carried out using Histogram of Oriented Gradients (HOG) extraction. HOG extraction focuses on the change in gradient between pixels analysing the images gradient direction and magnitude to produce relevant data and provide an insight to the shape of an object in a picture. This is a good starting place for our implementation as the shape of the image is the key indication as to what the image represents in addition to factors such as colour.

### K-Means and Gaussian
K-Means and Gaussian approaches are used more for clustering data than classifying, so they are not very suitable for this problem and can often yield unexpected results. A good example of this would be the K-Means algorithm. K-Means can be very harsh in terms of decision making, it often does not account for outliers and can regularly miss cluster objects. Gaussian Mixture Modelling (GMM) is slightly better as it does not partition the data, rather it scales the data between likelihoods of belonging to a specific cluster. However, GMM suffers from the same drawbacks as K-Means as both are unsupervised.

### LDA and PCA
Linear Discriminant Analysis (LDA) and Principal Component Analysis (PCA) are two other techniques that can be tested. After researching the use of these two methods in image recognition, primarily facial recognition, it is suggested that PCA is favourable on small data sets whereas LDA is better on larger datasets. This was studied in a paper published by Aleix Martinez and Avinash Kak Titled “PCA versus LDA”, which claimed the performance of both vary depending on the size of dataset and validated this claim in their paper. As the dataset in question is rather large, I will disregard PCA and focus on LDA.
Although these two methods are unlikely to match up to other techniques it can be useful to offer insight into the structure of the data and how I should carry out the other approaches.

### SVM
Supervised techniques are more suitable when tackling a problem such as image recognition. Support Vector Machines (SVMs) are a good way to go and can be effective in applications such as facial recognitions. This is discussed in a paper published at Nanyang Technological University Titled ‘Face Recognition by Support Vector Machines’, concluding ‘it appears that the SVMs can be effectively trained for face recognition’. The experimental results show that the SVMs are a better learning algorithm than the nearest center approach for face recognition. In addition, one paper published at NEX Laboratories America reported a 71.8% classification accuracy in the ImageNet image set. The research suggests SVMs are one appropriate method which is relatively simple to implement when compared to other algorithms. SVMs also have further applications in areas such as motion recognition as detailed in a paper published by the Royal Institute of Technology (KTH).

### Neural Networks
Deep learning techniques such as Convolutional Neural Networks (CNN) are often used in image recognition and have wide reaching application such as analysing images containing malaria or cholera to and decipher which is present. One study found a classification accuracy of their CNN to be as high as 94% [5]. However, neural networks can have a long execution time if the application is not correct. This will need to be avoided as having an algorithm that executes in a feasible amount of time on a large data set is just as important as the accuracy. Neural networks also work well to analyse very minute changes in images, performing well on datasets such as CUB-200-2011, a data set containing images of different species of bird.

When implementing NNs or CNNs it is important to consider the order of the data as some of the libraries I will be using require data in a specific order. This means I will have to use a line of code such as np.transpose(trnData,(3,0,1,2) to re-order the train and test data to SxWxHxC . Also, neural networks often work better on shuffled data sets, so it would be a good idea to add a function such as the numpy shuffle() function to randomize the order of the data.
The CNN requires the data to be fed slightly differently, so I will add a section in the code to properly organise the data. The main thing is to ensure the user of keras.utils.to_categorical(ytrain, 10) and ytestCNN = keras.utils.to_categorical(ytest, 10) to ensure 10 categories are used.


## __**Results**__
I began by testing PCA to see if this unsupervised method could provide insight into the characteristics of the data. My results showed PCA with n_components=2 produces a score of 14272, I then tested with n_components=10 yielding a score of 14632. This shows how much data was able to be retained while massively reducing the dimensions. This is probably because the data is often related which benefits dimensionality reduction, showing it can be a very useful tool when approaching problems such as this.

I then moved on to supervised approaches starting with LDA. I attempted to classify the data using LDA but only yielded a score of 0.375. The below confusion matrix shows the large number of false positives and false negatives produced. The main categories to suffer from this were categories 3,4,5,6,7. This is likely because they are all animal images and have a relatively similar structure making them hard to differentiate.

![image](https://user-images.githubusercontent.com/56178841/145639043-d4aac808-07e9-41bb-acd5-dedc92943a04.png)

After implementing SVM it is clear my hypothesis was incorrect and that SVM it is not the preferred method. Although it is a good technique for image recognition, SVMs suffer from the ‘curse of dimensionality’ meaning the execution time is very long for large data sets. The curse of dimensionality describes the difficulty of analysing data in larger dimensional space (such as this data set), causing computational time to suffer greatly taking over 15 minutes to complete execution. The results I achieved in my prediction were however good. Achieving an accuracy of 0.5145, the confusion matrix below shows the number of true positive and true negatives (along the diagonal) to be relatively high when compared to the number of false positives and false negatives. 

![image](https://user-images.githubusercontent.com/56178841/145639088-3695a5f1-4ed9-4e6b-8f40-d7304746fe19.png)

I then progressed to deep learning techniques; I first tested the suitability of a Neural Network (NN). I then decided to move on to testing a convolutional neural network.

![image](https://user-images.githubusercontent.com/56178841/145639121-ea8889d2-7c66-474d-a2ee-bdb601c8fe2d.png)

Finally, I tested the performance of a Convolutional Neural Network (CNN). The CNN performed the best out of all the methods I have proposed. My CNN achieved and accuracy of ~0.618. This was achieved using various tweaks to the model to provide the best outcome. The first tweak I used was to add an additional filter layer (124 filters) (more filters are added to deeper networks as discussed in this article from pyimagesearch.com [8]). I also altered the model’s padding parameter, padding allows us to specify whether the special dimensions can be reduced [8], and so I added padding = “same” to my CNN model. Before these tweaks were added I was achieving ~0.55 proving these sorts of tweaks can have a great impact on performance. Finally, I changed the activation from Relu, which I initially used, to LeakyRelu. After carrying out some research I found that LeakyRelu allows for the use of small (non-zero) values, this is important because as the dataset is normalised there are a large amount of these sort of values [9].

![image](https://user-images.githubusercontent.com/56178841/145639141-7194f96f-75a1-4e9d-828f-1336bb0f9260.png)

The CCN and the addition of a convolution layer likely performs better for two reasons. First, the convolution layer provides simpler processing, improving time complexity dramatically. Second, the convolution layer benefits datasets with a lot of related data, images are a great example of this.

## __**Conclusion**__
As predicted a supervised approach is favourable to this problem, especially since we are given labels in our dataset. Our SVM and LDA implementation yielded reasonable accuracies, but suffered from extremely long execution times, partly due to the ‘curse of dimensionality’. Out of the two, SVM would be the preferred option as it had a similar running time to LDA, but a higher accuracy of 0.514 (whereas LDA yielded 0.315). Similarly, PCA is useful for gaining insight into the dataset. However deep learning techniques, namely CNNs, are the most suitable approach to this problem as they work well on datasets that contain related data and execute in reasonable time.

The accuracy of these algorithms can be improved with various tweaks and changes. For example, a paper published by Harbin University of Science and Technology, China, concluded that an improved SVM using two mixed kernel functions of Wavelet and Gaussian functions improved efficiency and accuracy [7].  In addition to this, more computing power will improve the results for SVM and LDA which currently suffer from long execution times, however this us due to the limitations of my hardware. If this experiment were to be carried out again, I would recommend more computing power and find out what other tweaks can be made to improve the methods.

