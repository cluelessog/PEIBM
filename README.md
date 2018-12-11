# PEIBM
Usage of IBM Watson Visual Recognition API along with opencv 3.4.2 to recognize Indian Coins in an Image. This repository consist of two folders.  
1. EuroCoins - The original Recipe Dataset
2. IndiaCoins - Dataset I clicked using my mobile phone  

To clone the repository, execute the following command:  
~~~
git clone https://github.com/sourabhkumar0308/PEIBM.git
~~~
# Requirements
The requirements are in the "requirements.txt" files, but here is a list of most needed items:
* Python 3.4,3.5,3.6 (I have used 3.6)
* OpenCv 3.4.2
* Watson Python SDK >= 1.3.5
* Set of pictures to train the custom classifier (Available in the repository)
* Visual Recognition and Text to Speech credentials  

To install the dependencies, execute the following command:  
~~~
pip install -r requirements.txt
~~~
After installing python, it is a good practice to create a virtual environment  
~~~
python3.6 -m virtualenv <name_folder>

#To "enter" in the virtualenv
cd <name_folder>
source bin/activate

#Now, the begginning of your prompt will look like:
(name_folder) user@machine $>
~~~
__Note__ : Make sure that you first create the virtual enviroment and then install the dependencies from __requirements.txt__ file.
# Directory Structure

# Credentials
The credentials folder contains a JSON file that has structure for proper credentials file. Replace the __url__ and __iam_apikey__ with your own.
```json
{
  "visual": {
       "url": "YOUR URL",
       "iam_apikey": "YOUR API KEY",
       "classifier": "YOUR CLASSIFIER ID"
  },
  "tts": {
       "url":"YOUR URL",
       "iam_apikey":"YOUR API KEY"
  }
}

```
# API Usage guide
## Authentication
To authenticate using credentials file  
```python
import json
from watson_developer_cloud import VisualRecognitionV3 as VisualRecognition
creds = json.load(open('credentials'+os.sep+'watson_credentials.json', 'r'))['visual']
url = creds['url']
api_key = creds['iam_apikey']
visual = VisualRecognition(url=url, iam_apikey=api_key)
```
There is an optional version parameter (__'version=YYYY-MM-DD'__) taken by __VisualRecognition()__ to use the API version for the date specified.
To authenticate without credentials file
```python
import json
from watson_developer_cloud import VisualRecognitionV3 as VisualRecognition
visual = VisualRecognition(version='{version}',url='{url}',iam_apikey='{apikey}')
```
## Creating a custom classifier
This section shows how to train a custom classifier. The __print__ command prints the model details like classes, classifier id etc.
```python
import json
from watson_developer_cloud import VisualRecognitionV3 as VisualRecognition
creds = json.load(open('credentials'+os.sep+'watson_credentials.json', 'r'))['visual']
url = creds['url']
api_key = creds['iam_apikey']
visual = VisualRecognition(url=url, iam_apikey=api_key)
with open('train'+os.sep+'zips'+os.sep+'mixed.zip', 'rb') as coin,\
            open('train'+os.sep+'zips'+os.sep+'rupee1.zip', 'rb') as rupee1, \
            open('train'+os.sep+'zips'+os.sep+'rupee2.zip', 'rb') as rupee2, \
            open('train'+os.sep+'zips'+os.sep+'rupee5.zip', 'rb') as rupee5, \
            open('train'+os.sep+'zips'+os.sep+'rupee10.zip','rb') as rupee10, \
            open('train'+os.sep+'zips'+os.sep+'notCoins.zip', 'rb') as notCoins:
        
        model = visual.create_classifier('rupees',coin_positive_examples=coin, rupee1_positive_examples=rupee1,\
                                                 rupee2_positive_examples=rupee2,rupee5_positive_examples=rupee5,\
                                                 rupee10_positive_examples=rupee10,\
                                                 negative_examples=notCoins).get_result()
        print(json.dumps(model, indent=2))
```
Example Response after training  
```json
{
  "classifier_id": "rupees_1335655757",
  "name": "rupees",
  "status": "ready",
  "owner": "3d6cee2f-6c17-4ce6-b684-0b4617ce6916",
  "created": "2018-12-10T14:19:20.940Z",
  "updated": "2018-12-10T14:19:20.940Z",
  "classes": [
    {
      "class": "rupee5"
    },
    {
      "class": "rupee2"
    },
    {
      "class": "rupee1"
    },
    {
      "class": "coin"
    },
    {
      "class": "rupee10"
    }
  ],
  "core_ml_enabled": true
}
```
## Classification
Perform classification task once you have trained the classifier as shown above. __ID__ is the id returned after training the classifer.
```python
result = visual.classify(images_file=image, classifier_ids=ID,accept_language=language).get_result()
print(json.dumps(result, indent=2))
```
## Status Of Classifier and Deleting the classifier
The following statements gives the status of classifier and deletes the classifier.
```python
classifier = visual_recognition.get_classifier(classifier_id=ID).get_result()
print(json.dumps(classifier, indent=2))
```
Example Response  
```json
{
  "classifier_id": "rupees_1335655757",
  "name": "rupees",
  "status": "ready",
  "owner": "3d6cee2f-6c17-4ce6-b684-0b4617ce6916",
  "created": "2018-12-10T14:19:20.940Z",
  "updated": "2018-12-10T14:19:20.940Z",
  "classes": [
    {
      "class": "rupee5"
    },
    {
      "class": "rupee2"
    },
    {
      "class": "rupee1"
    },
    {
      "class": "coin"
    },
    {
      "class": "rupee10"
    }
  ],
  "core_ml_enabled": true
}

```
```python
visual.delete_classifier(ID)
```
