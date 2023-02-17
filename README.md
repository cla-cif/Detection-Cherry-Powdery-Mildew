## Dataset Content

The dataset contains 4208 featured photos of single cherry leaves against a neutral background. The images are taken from the client's crop fields and show leaves that are either healthy or infested by powdery mildew a biothropic fungus. This disease affects many plant species but the client is particularly concerned about their cherry plantation crop since bitter cherries are their flagship product. 

## Business Requirements

We were requested by our client Farmy & Foods a company in the agricultural sector to develop a Machine Learning based system to detect instantly whether a certain cherry tree presents powderry mildew thus needs to be treated with a fungicide. 
The requested system should be capable of detecting instantly, using a tree leaf image, whether it is healthy or needs attention. 
The system was requested by the Farmy & Food company to automate the detection process conducted manually thus far. The company has thousands of cherry trees, located on multiple farms across the country. As a result, this manual process is not scalable due to the time spent in the manual process inspection.
Link to the wiki section of this repo for the full [business interview](https://github.com/cla-cif/Cherry-Powdery-Mildew-Detector/wiki/Business-understanding-interview). 

Summarizing:

1. The client is interested in conducting a study to visually differentiate a cherry leaf that is healthy from one that contains powdery mildew.
2. The client is interested in predicting if a cherry tree is healthy or contains powdery mildew.

## Hypothesis and how to validate

1. **Hypotes**: Infected leaves have clear marks differentiating them from the healthy leaves.
   - __How to validate__: Research about the disease and build an average image study can help to investigate it.
2. **Hypotesis**: Mathematical formulas comparison: ```softmax``` performs better than ```sigmoid``` as activation function for the CNN output layer. 
   -__How to validate__: Understand the kind of problem we are trying to solve and the differences between matemathical functions used to solve that class of problem.  Train and compare identical models changing only the activation function of the output layer. 
3. **Hypotesis**: ```grayscale``` is better than ```RGB``` for image classification. 
   - __How to validate__: Understand how colours are represented in tensors. Train and compare identical models changing only the image color.

**WHY**A good model trains its ability to predict classes on a batch of data withouth adhering too closely to that set of data. In this way the model is able to generalize and predict future observation reliably because it didn't 'memorize' the relationships between features and labels as seen in the training dataset but the general pattern from feature to labels. 
Understand the concepts of overfitting and underfitting and how to steer away from them. 

### Hypotesis 1
> Infected leaves have clear marks differentiating them from the healthy leaves.

We suspect cherry leaves affected by powdery mildew have clear marks, typically the first symptom is a light-green, circular lesion on either leaf surface, then a subtle white cotton-like growth develops in the infected area. 
An Image Montage shows the evident difference between a healthy leaf and an infected one. 

Average Image, Variability Image and Difference between Averages samples did not reveal any clear pattern to differentiate one from another.
![average variability between samples](/workspace/Detection-Cherry-Powdery-Mildew/outputs/v1/avg_var_powdery_mildew.png)

Sources:
[Pacific Nortwest Pest Management Handbooks](https://pnwhandbooks.org/plantdisease/host-disease/cherry-prunus-spp-powdery-mildew)

### Hypotesis 2
> Mathematical formulas comparison: ```softmax``` performs better than ```sigmoid``` as activation function for the CNN output layer. 

First of all let's understand the problem our model is asked to solve. The model is required to assign a cherry leaf one of the two categories: healthy/infected, which makes it a classification problem. It could be seen as a binary classification (healthy vs NOT healthy) or a multiclass classification where each output is assigned one and only one label from more than two classes (just two in our case: healthy vs infected).

If the problem is seen as **binary classification** we will have 1 output node. The probability of the output belonging to one class or the other is within the range of 0 and 1 so if is <0.5 is considered class 0 (healthy) and if >=0.5 is considered class 1 (infected).<br />
These constraints are given by the ```sigmoid``` function which is also called the _squashing_ function as the classes will converge either to 0 or 1. It's computationally effective but used for binary classification problems only as it suffers major drawbacks which include sharp damp gradients during backpropagation. <br /> 
Backpropagation is where the “learning” or “adjustment” takes place in the neural network in order to adjust the weights of all the nodes throughout the layers of the network. The error value (distance between actual and predicted label) flows back through the network in the opposite direction as before, and it is then used in combination with the derivative of the Sigmoid function. <br />
The derivative of a function will give us the angle/slope of the graph that the function describes. This value will let the network know weathe to increase or decrease the value of the individual weights in the layers of the network but for a very high or very low value of the error, the derivative of the sigmoid is very low (hence the _squashing_ effect).  

If we see the problem as **multi class classification** we will have 2 output nodes (because I want to predict two classes healthy vs infected). In this case the ```softmax``` function is applied to the output layer. Like the previous case the output of this function lies in the range [0,1] but now we are looking at a probability distribution over the predicted classes which adds up to 1 with the target class having the highes probability. The probability distribution comes from normalizing the output for each class between 0 and 1 and divide by their sum. 

In our case the ```softmax``` function performed better. 


Sources:
- [Activation Functions Compared With Experiments](https://wandb.ai/shweta/Activation%20Functions/reports/Activation-Functions-Compared-With-Experiments--VmlldzoxMDQwOTQ) by [Sweta Shaw](https://wandb.ai/shweta)
- [Backpropagation in Fully Convolutional Networks](https://towardsdatascience.com/backpropagation-in-fully-convolutional-networks-fcns-1a13b75fb56a#:~:text=Backpropagation%20is%20one%20of%20the,respond%20properly%20to%20future%20urges.) by [Giuseppe Pio Cannata](https://cannydatascience.medium.com/)
- [Understanding The Derivative Of The Sigmoid Function](https://towardsdatascience.com/understanding-the-derivative-of-the-sigmoid-function-cbfd46fb3716#:~:text=The%20Sigmoid%20function%20is%20often,of%20the%20network%20or%20not.) by [Jacob Toftgaard Rasmussen](https://jacobtoftgaardrasmussen.medium.com/)
- [Activation Functions: Comparison of Trends in Practice and Research for Deep Learning](https://arxiv.org/pdf/1811.03378.pdf) by _Chigozie Enyinna Nwankpa, Winifred Ijomah, Anthony Gachagan, and Stephen Marshall_


### Hypotesis 3 
>```grayscale``` is better than ```RGB``` for image classification. 

An image is made of pixels.Every image has three main properties:
- Size — This is the height and width of an image. It can be represented in centimeters, inches or even in pixels.
- Color space — Examples are RGB and HSV color spaces.
- Channel — This is an attribute of the color space. For example, RGB color space has three types of colors or attributes known as Red, Green and Blue (hence the name RGB).

In an RGB image where there are three color channels, a pixel value has three numbers, each ranging from 0 to 255 (both inclusive). For example, the number 0 of a pixel in the red channel means that there is no red color in the pixel while the number 255 means that there is 100% red color in the pixel. A single RGB image can be represented using a three-dimensional (3D) NumPy array or a tensor.<br />
In a grayscale image where there is only one channel, a pixel value has just a single number ranging from 0 to 255 (both inclusive). The pixel value 0 represents black and the pixel value 255 represents white. Therefore a single grayscale image can be represented using a two-dimensional (2D) NumPy array or a tensor because it doesn't need an extra dimension for the color channel. <br />
Feeding a model with an RGB image or convert that image to grayscale, depends on the nature of the images and the information conveyd by the colour. 
If the color has no significance in the image to classify, indeed a grayscale image requires less computational power to process.
The same CNN applied to an RGB image dataset has _______________ parameters to train compared to 3,714,658 parameters when the same dataset is converted to grayscale. 

- Comparison of the same image
  
- Comparison of LSTM 

- Overall accuracy

In our case ____________ performed better. 


Sources:
- [How RGB and Grayscale Images Are Represented in NumPy Arrays](https://towardsdatascience.com/exploring-the-mnist-digits-dataset-7ff62631766a) by [Rukshan Pramoditha](https://rukshanpramoditha.medium.com/)
- 

## The rationale to map the business requirements to the Data Visualizations and ML tasks

### Business Requirement 1: Data Visualization

We will display the "mean" and "standard deviation" images for healthy and powdery mildew infected leaves.<\br>
We will display the difference between an average infected leaf and an average healthy leaf.
We will display an image montage for either infected or healthy leaves.

### Business Requirement 2: Classification
INDICATE HOW THESE BUSINESS REQUIREMENTS WERE MET WITH A ML TASK (SAME AS ABOVE)
We want to predict if a given leaf is infected, or not, with powdery mildew.
We want to build a binary classifier and generate reports.

## ML Business Case

### Powdery Mildew classificator
(TERMINOLOGY: labels, target, features, variables, attributes, train/fit, model output, model metrics, predictions... )</br>
- We want an ML model to predict if a leaf is infected with powdery mildew or not, based on the image database provided by the Farmy & Foods company. It is a supervised model, a 2-class, single-label, classification model.
- Our ideal outcome is to provide the farmers a faster and more reliable detector for powdery mildew detection.
- The model success metrics are
    - Accuracy of 65% or above on the test set.
- The model output is defined as a flag, indicating if the leaf has powdery mildew or not and the associated probability of being infected or not. The farmers will take a picture of a leaf and upload it to the App. The prediction is made on the fly (not in batches).
- Heuristics: The current detection method is based on a manual inspection. A farmer spends around 30 minutes in each tree, taking a few samples of tree leaves and verifying visually if the leaf tree is healthy or has powdery mildew. Vusual criteria is slow and it leaves room to procduce inaccurate diagnostics due to human error. 
- The training data to fit the model come from the leaves database provided by Farmy & Foody company and uploaded on Kaggle. This dataset contains 4208 images of cherry leaves. 

## Dashboard Design (Streamlit App User Interface)

### Page 1: Quick Project Summary
- Quick project summary
    - General Information:
        - Powdery mildew is a parasitic fungal disease caused by Podosphaera clandestina in cherry trees. When the fungus begins to take over the plants, a layer of mildew made up of many spores forms across the top of the leaves. The disease is particularly severe on new growth, can slow down the growth of the plant and can infect fruit as well, causing direct crop loss.
        - Visual criteria used to detect infected leaves are light-green, circular lesion on either leaf surface and later on a subtle white cotton-like growth develops in the infected area on either leaf surface and on the fruits thus reducing yeld and quality."
- Project Dataset
The available dataset provided by Farmy & Foody contains 4208 featured photos of single cherry leaves against a neutral background. The leaves are either healthy or infested by cherry powdery mildew.
Link to additional information
- Business requirements:
    1. The client is interested to have a study to visually differentiate between a parasite-contained and uninfected leaf.
    2. The client is interested in telling whether a given leaf contains a powdery mildew parasite or not.

### Page 2: leaves Visualizer
It will answer business requirement 1
- Checkbox 1 - Difference between average and variability image
- Checkbox 2 - Differences between average parasitised and average uninfected leaves
- Checkbox 3 - Image Montage

### Page 3: Powdery mildew Detector
- Business requirement 2 information - "The client is interested in telling whether a given leaf contains a powdery mildew parasite or not."
- Link to download a set of parasite-contained and uninfected leaf images for live prediction on [Kaggle](https://www.kaggle.com/datasets/codeinstitute/cherry-leaves)
- User Interface with a file uploader widget. The user should upload multiple powdery mildew leaf images. It will display the image and a prediction statement, indicating if the leaf is infected or not with powdery mildew and the probability associated with this statement.
- Table with the image name and prediction results.
- Download button to download table.

### Page 4: Project Hypothesis and Validation
- Block for each project hypothesis, describe the conclusion and how you validated it.

### Page 5: ML Performance Metrics
- Label Frequencies for Train, Validation and Test Sets
- Model History - Accuracy and Losses
- Model evaluation result

## Unfixed Bug

## Deployment

### Heroku
- The App live link is: https://YOUR_APP_NAME.herokuapp.com/
- Set the runtime.txt Python version to a Heroku-20 stack currently supported version.
- The project was deployed to Heroku using the following steps:
  1. Log in to Heroku and create an App
  2. At the Deploy tab, select GitHub as the deployment method.
  3. Select your repository name and click Search. Once it is found, click Connect.
  4. Select the branch you want to deploy, then click Deploy Branch.
  5. The deployment process should happen smoothly if all deployment files are fully functional. Click now the button Open App on the top of the page to access your App.
  6. If the slug size is too large then add large files not required for the app to the .slugignore file.

## Technologies used

## Platforms
- [Jupiter Notebook](https://jupyter.org/)
- [Streamlit](https://streamlit.io/)
- [Kaggle](https://www.kaggle.com/)

## Languages
- [Jupiter Notebook](https://jupyter.org/)
- [Python](https://www.python.org/)
- 
### Main Data Analysis and Machine Learning Libraries

- numpy 1.19.2          used for 
- pandas 1.1.2          used for 
- matplotlib 3.3.1      used for 
- seaborn 0.11.0        used for 
- plotly 5.12.0         used for 
- streamlit 0.85.0      used for 
- scikit-learn 0.24.2   used for 
- tensorflow-cpu 2.6.0  used for 
- keras 2.6.0           used for 

## Credits

In this section, you need to reference where you got your content, media and from where you got extra help. It is common practice to use code from other repositories and tutorials. However, it is necessary to be very specific about these sources to avoid plagiarism.
You can break the credits section up into Content and Media, depending on what you have included in your project.

### Content
- The leaves dataset was linked from [Kaggle](https://www.kaggle.com/datasets/codeinstitute/cherry-leaves) and created by [Code Institute](https://www.kaggle.com/codeinstitute)
- The powdery mildew description was taken from [garden design](https://www.gardendesign.com/how-to/powdery-mildew.html) and [almanac](https://www.almanac.com/pest/powdery-mildew)
- App pages for the Streamlit dashboard, data collection and data visualization jupiter notebooks were inspired by [Code Institute WP01](https://github.com/cla-cif/WalkthroughProject01)
- The [CRISP DM](https://www.datascience-pm.com/crisp-dm-2/) steps adopted in the [GitHub project](https://github.com/cla-cif/Cherry-Powdery-Mildew-Detector/projects?query=is%3Aopen) were modeled on [Introduction to CRISP-DM](https://www.ibm.com/docs/en/spss-modeler/saas?topic=guide-introduction-crisp-dm) articles from IBM.
- Model learning Curve - C is from [Stack Overflow](https://stackoverflow.com/questions/41908379/keras-plot-training-validation-and-test-set-accuracy) by [Tim Seed](https://stackoverflow.com/users/3257992/tim-seed)
  
### Media
The photos used on the home and sign-up page are from This Open-Source site.
The images used for the gallery page were taken from this other open-source site.

### Code
You need to add a comment in your code to make clear the following:
- The code you are using is not your original work.
- The source location of the code you borrowed, usually indicated with a URL.
For larger dependencies, also refer to the borrowed source in your README file with a short explanation of your intended use and a link to the source.
For 3rd party libraries, refer to the borrowed source in your README file with a short explanation of your intended use and a link to the source.

## Acknowledgements

Thank to [Code Institute](https://codeinstitute.net/global/)

