# Classifying White Blood Cells with CNN

## Business Understanding

White blood cell counts (WBC) are used in diagnosing many conditions such as viral infections, autoimmune diseases and certain types of cancer. Blood tests are conducted in labs where results require at least 24 hours, but often take longer due to volume constraints. Doctors Without Borders would like to partner with our team for a proof of concept alternative to blood tests in assessing white blood cell counts. They need a solution that can be deployed in the field where no local labs exist. Our solution will use blood cell images to identify white blood cells using a convolutional neural network (CNN).

## Data Understanding

The dataset comes from Kaggle (https://www.kaggle.com/datasets/paultimothymooney/blood-cells) and includes approximately 12,000 augmented JPEG images of cells from a microscpe. Images came prepackaged in train and test sets that were equally balanced across the four classes. The training set was further split into training and validation sets where validation contained 20% of the training images. Images were loaded into the keras preprocessing package ImageDataGenerator to produce the image pixel arrays and labels.

## EDA

For EDA, the distribution of pixel intensities for each class was graphed using seaborn and examined for any noticeable differences. Inspection of the distributions showed a couple of interesting trends. First, Lymphocytes and Monocytes seem to have slightly wider distributions with more pixel values pushed out towards the tails in opposite directions. This tendency toward lighter and darker pixel intesities may make them easier for the model to detect. The second aspect of the distributions that raised flags is that Eosinophil and Neutrophil are both more densly packed around the mean. This suggests that they are similar to each other and will be more difficult for the model to distinguish. An ANOVA test was also run to confirm the classes come from different populations and have statistically significant differences in mean pixel intensity.


![Pixel Distribution](https://user-images.githubusercontent.com/66101132/229366057-8718a4e4-43fe-4a7f-9073-989b3ef1b701.png)


## Baseline Model

For the baseline model, a very simple CNN with only one convolutional layer, max pooling, and one dense layer was used. Models will be evaluated based on their accuracy score and categorical cross-entropy loss. The baseline model returned 45% accuracy and 0.4796 loss on the test data.

## Modeling Process

In each iteration of the process, different aspects of the model were changed. For the first couple iterations, the complexity of the model was altered by adding and removing convolutional and dense layers with varying numbers of filters and nodes. Once an ideal structure was identified, the size of the images, batch sizes and filter sizes were tuned. Next, a learning rate scheduler was implemented to help the model fine tune weights at later epochs. Lastly, a couple different regularization techniques like Dropout commands and L2 penalty terms were trialed. In total, 10 models after the baseline were run. Model 7 was chosen as the best and final model. The final model returned an 81% accuracy score and loss of 0.545 on the test data. The progression of accuracy and loss for the training data over 100 epochs is shown below:  


![Accuracy](https://user-images.githubusercontent.com/66101132/229366067-b30797f6-28a2-467e-9206-fa2633058170.png)

![Loss](https://user-images.githubusercontent.com/66101132/229366073-59b4a685-8602-4f1d-8be5-c843f19c9dbf.png)

## Model Evaluation

The final model confirmed our early hypothesis that Eosinophil and Neutrophil would be difficult to distinguish from each other. Looking at the confusion matrix results, we can see that classes 0 and 3 are most commonly misclassified as each other. Both the matrix and classification report also confirmed our hypothesis that the relatively light pixel intensities of the Lymphocytes would make them easier to classify (98% f1-score). The model also performed better on Monocytes than the Eosinophils and Neutorphils, but not as well as expected given the noticeable skew in the Monocyte pixel distribution. In total, the model performs well, but still suffers from overfitting which will need to be addressed in future iterations 

![Classification Report](https://user-images.githubusercontent.com/66101132/229366083-eeb03eb9-dcbe-479f-8a04-9cfc89d0c95e.png)


![Confusion Matrix](https://user-images.githubusercontent.com/66101132/229366101-49672200-749d-4f61-839a-a11f9a7fff77.png)


## Recommendations

My recommendation for Doctors Without Borders is to deploy the model as a frontline tool for measuring WBC. Additional images collected can be used to further train the model and improve performance.

## Next Steps

As next steps, the model could be further improved by adding more training data and trialing more regularization techniques to address the overfitting. To make the model truly ready for the field, training would also need to be expanded to include red bloods to see if the model can separate out white and red blood cells in a real world sample.
