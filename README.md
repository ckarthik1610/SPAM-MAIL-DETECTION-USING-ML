# SPAM-MAIL-DETECTION-USING-ML

```python
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score

raw_mail_data = pd.read_csv('/content/mail_data.csv')

# Raw Data
print(raw_mail_data)

# Filtering Null Datasets
mail_data = raw_mail_data.where((pd.notnull(raw_mail_data)),'')
mail_data.head(15)

# Remaining Datasets after filtering Null datasets out
mail_data.shape

# Assigning Labels as 0 and 1
mail_data.loc[mail_data['Category'] == 'spam', 'Category',] = 0
mail_data.loc[mail_data['Category'] == 'ham', 'Category',] = 1

#Seperating data as text and Label
X = mail_data['Message']
Y = mail_data['Category']

#Splitting Training and Testing Data (80/20)
X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.2, random_state=5)
print(X.shape)
print(X_train.shape)
print(X_test.shape)

# transform the text data to feature vectors that can be used as input to the Logistic regression
feature_extraction = TfidfVectorizer(min_df = 1, stop_words='english', lowercase=True)
X_train_features = feature_extraction.fit_transform(X_train)
X_test_features = feature_extraction.transform(X_test)

# convert Y_train and Y_test values as integers
Y_train = Y_train.astype('int')
Y_test = Y_test.astype('int')
```
### TRAINING THE LOGISTIC REGRESSION MODEL
```
model = LogisticRegression()
model.fit(X_train_features, Y_train)

```
### Training Data
```python
# Prediction
prediction_on_training_data = model.predict(X_train_features)
accuracy_on_training_data = accuracy_score(Y_train, prediction_on_training_data)
     

# Accuracy
print('Accuracy on training data : ', accuracy_on_training_data)
```

### Testing Data
