# Mask_Detection_Project
Classify images upon the presence of people wearing or not a face mask.

## First approach
At the beginning we tried to follow a "creative" approach, dividing the problem into three
subparts. Firstly, we searched for a premade network (MTCNN) that could extract faces from
pictures. Secondly, we trained a network to categorize the chunks given by the first one into
masked and non-masked faces. Lastly, we counted the number of predictions for each of the two
classes of the second network. Therefore, the final prediction on a complete picture was made by
applying this chain of operations. To train our net, we used the images from class 0 (no mask) and
1 (all mask), in order to assign a sure label to each patch automatically.

![image](https://user-images.githubusercontent.com/37812489/128538862-9fcf6abf-b758-4b20-a53b-a3f28d2806f6.png)

Unfortunately, this approach was not much successful. It turned out that the first network wasn't
sufficiently precise in recognizing faces, particularly masked once. Since this heavily constrained
the possible accuracy, we decided to change approach.


## Second approach
Then we developed a unique network to classify the input images directly in the 3 classes.
Two main constraints guided our choices: the limited computationl power available and the small
size of the dataset. Therefore, we chose to start from a very simple model and build up
complexity upon it. We started from a VGG-like network, based on the idea of small-size filters
(3x3) and multiple consecutive convolutions. The overall structure is based on the stacking of
multiple convolutional blocks (Conv-{Conv}*-Activation-MaxPool).

We followed a three-step procedure to increase accuracy:
1. increase the model complexity (by increasing the number of initial filters, the consecutive
convolutional layers and the neurons in the FC part)
2. reduce overfitting with dropout. We applied this technique first because we found out that it
was more stable and fast than Data Augmentation.
3. starting from the network trained at the previous step, we applied data augmentation, in
order to achieve better generalizing results. We used the pre-trained network (with dropout)
because data augmentation was very slow and unstable. In this way we managed to
drastically reduce its training time.
Here there is an example of how the accuracy and loss changed over the three phases:

![image](https://user-images.githubusercontent.com/37812489/128539067-cbc5bc4d-c4e1-4503-817d-3deeef8d1bb4.png)

![image](https://user-images.githubusercontent.com/37812489/128539120-66e876b0-d5e9-4014-862a-7d8f55e4a51e.png)

![image](https://user-images.githubusercontent.com/37812489/128539148-68e7cda5-7777-44b5-96d1-ac1e9dc175ba.png)

After some iterations of improvements, we got to this net, which scores 0,8266 on the test set on
Kaggle.

## Comments
* We chose not to use transfer learning because we preferred to make an effort to improve as
much as we could a network built from scratch.
* We chose not to try many times the model on the test set, in order not to bias our result
toward it. We mostly relied on the validation set, built from the training set given.
* We chose to start from a VGG-like net because we analysed this model during classes and we
wanted a simple structure to work on.
