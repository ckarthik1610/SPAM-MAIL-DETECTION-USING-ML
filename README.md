# SPAM MAIL DETECTION USING MACHINE LEARNING

The SPAM MAIL DETECTION is a Machine Learning Model based on Logistical Regression. This system invloves differentiating the Mail text into lebels called SPAM and HAM.

**Sample Dataset:**

![image](https://github.com/ckarthik1610/SPAM-MAIL-DETECTION-USING-ML/assets/160818854/7c98f711-08a3-481d-a4a4-502be8392040)

The system imports the dataset and filters NULL datasets out, then initialises the labels into 0 and 1's (SPAM = 0, HAM = 1).

![image](https://github.com/ckarthik1610/SPAM-MAIL-DETECTION-USING-ML/assets/160818854/66d72c46-7b92-4683-bea9-f7f084130362)


**TRAIN AND TEST DATA:**
        The system splits the dataset into training and testing datasets. The model we incorporated uses a 80% training and 20% testing dataset.

**FEATURE EXTRACTION**
        This system extracts the words from the mail text and finds words in the mail that act as important features in differentiating SPAM and HAM mail. This method uses a English words repository that omits words like 'of', 'are', 'with' that does not neessarily impact in the distinction. The text in the mail is converted to a numerical value, that later helps in the prediction.

![image](https://github.com/ckarthik1610/SPAM-MAIL-DETECTION-USING-ML/assets/160818854/d3462943-02a6-4456-aa9a-15b1af6a2018)

**TRAINING THE MODEL**
        The logistical regression model is trained, using the Feature Extraction data, this model is now capable of making predictions.

![image](https://github.com/ckarthik1610/SPAM-MAIL-DETECTION-USING-ML/assets/160818854/f6e292e7-f01f-4738-a072-32888d061d5d)


**EVALUATING THE MODEL**
        The Model is then cross-verified with the test data labels when tested with the testing data (remaining 20% data). 

![image](https://github.com/ckarthik1610/SPAM-MAIL-DETECTION-USING-ML/assets/160818854/66a1b6ba-7719-4868-aae1-e132eb23fee7)

**OUTPUT**

![image](https://github.com/ckarthik1610/SPAM-MAIL-DETECTION-USING-ML/assets/160818854/948865ed-d303-4c2b-ace4-d8c2a8f5349e)





