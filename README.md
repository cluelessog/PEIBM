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
# Credentials
The credentials folder contains a JSON file that has structure for proper credentials file. Replace the __url__ and __iam_apikey__ with your own.

# API Usage guide
## Authentication
