# Meta Learning (few-shot-learning) explained
> - An Introduction and exploration of Meta learning architucture specifically **few-shot-learning**. 
> - Our goal is to be able to classify new objects never seen it in the training data with very few examples.

## Tables of contents
1. [Motivation for Meta learning](#motivation-for-meta-learning)
1. [General idea about few-shot-learning](#few-shot-learnig)
1. [Defining common terms for few-shot-learning](#defining-common-terms-in-few-shot-learnig)
1. [How the model learn ??](#how-the-model-learn)
1. [Contacts](#contacts)
1. [Rsources](#resources)
1. [License](#license)

## Motivation for Meta learning
> Learning visual models of object categories requires thousands of training examples; this is due to the diversity and richness of object appearance which requires models containing hundreds of parameters.
- Informal observation tells us that Humans can learn a new category of animals both fast and easy
- Human can learn to recognize an animal that they have never seen before by seeing 2 or 3 images of it.
- ***In The other side:*** Computer requires thousands if not tens of thousands of training images to be able to reconize a new category.
- Could computer vision algorithms be similarly efficient?
- One possible explanation of human efficiency is that when learning a new category we take advantage of prior experience.
  - The appearance of the categories we know and, more importantly, the variability in their appearance, gives us important information on what to expect in a new category. This may allow us to learn new categories from few(er) training examples.
- And here we will try to explain how we can **some how** give the copmuter the ability to mimic what Humans do.

---

## Few shot learnig
***Defination:*** Few-shot learning is the problem of making predictions based on a limited number of samples. 
- Few-shot learning is different from standard supervised learning.
  - **The goal of Here** is not to let the model recognize the images in the training set and then generalize to the test set. 
  - Instead, the goal is to learn. **Learn to learn**
- In supervised learnig the goal is to build a model around class recognition.
- So the goal isn't to recognize the cat or dog the goal is to know what is the diffrences between them
- ***In other simple words:*** 
> - The goal is to learn how to measure the similarity and differences between **2 classes**, here we accomplish the **learn to learn**.
> - That's how we can recognize unseen ***categories*** in the training set.
> - (e.g) Training into (Dogs, Cats, and Tigers) classes and then use the same model to recognize a new class like Elephant by providing it with additional information a support set (few shot).
> - We will get to the technical details of how the model can do this later.
> <img src="./assets/query_and_sim.png" width="600" height="350">

---

## Defining common terms in few shot learnig
+ **What is a support set ?**
  - After training the similarity function of our model we want to classify new image **(Query img)**, this image as we know from above doesn't belong to any of our classes so the support set will contain a set of images and at least one of them belong to the same class as the query img, so the model could compute a similarity_score for every img in the support set and pick the highest score as the class of the new img.
    > (e.g) our model has three classes (Cats, dogs, and tigers) and we want to query it using an image of elephant, so we'll need to provide a support set as our  additional info for the model (hence the name few shot, the small dataset) them the model will compare the query img with every single image and will provide similarity score, and we will choose the highest sim_score as our result the below image shows an example of what i did explain.

+ What is this mean **K-way, n-shot, support set (4way-3shots support set)**
  - **k-way:** are the number of classes in the support set, 4way means we have four classes (cat, dog, pegion, elephant).
  - **N-shot:** are the number of samples per class, 3shots means we have a 3 imgs per class in the support set.

---
## How the model learn
> - We'll be using the Siamase learning network to train our model, and there is two methods we can use to train the model.
> - You can read more about Siamase network by refering to the [Rsources](#resources) section.

### 1. Using pairwise similarity score
- ***The below mind map explains the learning process.***
<img src="./assets/simaseNet_simscores.png" height="421" width="750">

- ***The algorithm explaination***
  1. Preprocessing step:
      - We divide the images into a set of pairs to feed them to the network e.g `(img_1, image_2, 1)` this means we have two images **(positive pair)** belong to the same class assigned label 1 for them, `(image_a, image_b, 0)` those two images are **(negative pair)** and we assigned 0 to them.

  1. Then feeding the set of pairs to ***The same ConveNet*** for extracting our feature vector
  1. Then we get the ***absolute diff*** between the two feature vectors 
  1. Feeding the result to dense layers getting a scaler
  1. The scaler goes into a ***sigmoide*** getting a value between 0 and 1
  1. this value represnets the prediction of similarity between the two images
  1. Calculating our **loss** and gradient
  1. Then back-propagate the results to update the params of the ***dense layers and the ConvNet***
  1. Repeat Until we converge.
- ***In predictions***
    - Using the support set and the query image I can compare the query image with every image in the support set
    - Getting a value between 0 and 1
    - Then the maximum value is our predicted class
    > note that it may be all the images in the support set don't belong to any of the classes that we trained the model on.

### 2. Using the triplet loss method
- **Preprocessing step:**
    - We divide the images into a set of three pairs to feed them to the network e.g `(anchor_img, positive_img, negative_img)`
    - `Anchor_img` a randomly selected img that can belong to any of our classes 
    - `Positive_img` a randomly selected img that must have the same class as anchor
    - `Negative_img` a randomly selected img that belongs to any class ***Excluding*** the anchor class
- **Algorithm Explanation:**
- Here's a graph that gives a quick idea:
  > <img src="./assets/Triplet_loss.png" height="421" width="750">

- We'll be using the same idea as in **pairwise** For feature extraction, but here for three images to get a feature vector for every image
- Then, will calculate the D+ as the squared absolute difference between the `anchor feature vector and the positive feature vector`, and `D- as the squared absolute difference between the anchor feature vector and the negative feature vector`
- We need `D+` to be as small as possible and `D-` to be as large as possible
- Then we feed those results to our loss function
- ***Triplet Loss Function:***
    - As we can extract from the above we need D+ to be smaller than D- `D+ < D-`
    - So we will try to make our whole NN to work in this, starting by the loss function
    - But by how much we need this difference -> `Alpha` our most important hyper-parameter here Such that 
    - <img src="./assets/triplet_loss_Equation.jpg">
    - If the above equation is true so the loss will **be zero.**      
    - **If not:**  <img src="./assets/tirplet_loss_ifnot.jpg">
    - So our final Loss_function will be <img src="./assets/final_tripletLoss.jpg">
    - The feature space of the triplet loss will look like the below Image <img src="./assets/feature_space_of_triplet_loss.png" height="421" width="750">
- Then we will use gradient descent and back-propagate the results to update our parameters.

### 3. Summary of how the model learn
1. First we train our siamase network on a large data set.
1. Dividing the data into a two or three paris based on the method we choose for learning
1. In predictions we give the model a Query_Img and a support_set (k-way n-shot)
> - Note that the Query_Img not belongs to the classes we trained our network on.
> - k-way means k classes, n-shot means number of samples for every class, The prediction will be more effecient If **k** is small and **n** is large.
1. The model will output a similarity_score or a distance_value based on the method of learning.

## Contacts
> You can reach out for me in twitter [@amshrbo](twitter.com/amshrbo) or via mail `amshrbo@gmail.com`

## Resources
- You can Watch **Shusen Wang** videos on [YouTube](https://www.youtube.com/playlist?list=PLgtf4d9zHHO8YjSSkkBT55XN8xsIvb-ku).
  - You can find the slides associated with videos [here](https://github.com/wangshusen/DeepLearning) Just go to this repos and search for Meta Learning.
- [One-Shot Learning of Object Categories](https://www.computer.org/csdl/journal/tp/2006/04/i0594/13rRUxC0SXe) Lei Fei-Fei, Fergus, Perona, IEE, 2006
- [Few shot learnig article from Analytics vedhya](https://www.analyticsvidhya.com/blog/2021/05/an-introduction-to-few-shot-learning/)
- [Siamese neural network in wikipedia](https://en.wikipedia.org/wiki/Siamese_neural_network)
- [Siamese Neural Networks for One-Shot Image Recognition](http://www.cs.toronto.edu/~gkoch/files/msc-thesis.pdf)
- [FaceNet: A Unified Embedding for Face Recognition and Clustering](https://www.cv-foundation.org/openaccess/content_cvpr_2015/html/Schroff_FaceNet_A_Unified_2015_CVPR_paper.html)
- [online-latex-equation-editor for generating image equations](http://www.sciweavers.org/free-online-latex-equation-editor)

## License
[GPL-3.0 License](./LICENSE)
