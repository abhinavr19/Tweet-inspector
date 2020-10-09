# Tweet Inspector


Online social networks are likewise generally utilized by abusers, for spreading hate or some propaganda about world events or local affairs, politics or religion, interests, affiliations, organizations, products, people. Numerous culprits 'take cover' behind the way that they will most likely be unable to be promptly distinguished, making statements that they wouldn't think about saying eye to eye, which could be viewed as fainthearted. Online maltreatment takes a few structures, and unfortunate casualties are not limited to open figures. They can carry out any responsibility, be of all ages, sex, sexual direction or social or ethnic foundation, and live anyplace. With the growth of social media platforms in the 21st century, it has become necessary for any individual or organization to maintain a good social reputation. Any kind of oppressive and abusive posts against someone can result in the decline of their social reputation. Such abusive posts, in the worst case, can also result in social riots and other events that can harm the peace of a region. This problem needs a good solution. Many social media platforms like Instagram have developed and deployed systems that can detect abusive and hate inducing posts and block the account that they have been posted. But we found that there are no systems or fewer systems that can detect abuses in Indian languages. Hindi being the most popular Indian language, we decided to create a system using machine learning algorithms that can classify tweets in Hindi as abusive and not abusive.


## Implementation

### Dataset used

We have made use of frameworks with python and JavaScript condition to make AI model, front-end/REST-API. We have also made use of a Twitter developer account to gather tweet information through the Twitter API. The twitter API has a breaking point of 300 tweets for every page, so we chose to utilize a [Twitter Scraper](https://github.com/jonbakerfish/TweetScraper) to scratch tweets from twitter. We gathered around 10k tweets in the Devanagari script and created a training and testing set from this set of tweets. The information collection additionally contained the tweet id, user id, twitter handle of the user, number of retweets, number of favorites, number of replies for every single tweet. The legitimacy of information is guaranteed as the tweets are right now accessible on the web and we have utilized the official Twitter API for the reason. 

>The data folder in this repo contains 4 files. The file tweet3.xlsx contains 7k abusive tweets. The file tweets4.xlsx contains 3.5k non-abusive. The file data.xlsx is the consolidated collection of abusive and non abusive tweets contained in the previously mentioned files. It was made using final_tweet.py. This data is labelled. The file processed.xlsx contains cleaned tweets. The tweets are cleaned using clean.py file.

### Model/Methodology

Firstly the data set was cleaned by removing stop words, special characters, hashtags, punctuations, blanks. As we only needed Hindi tweets, we removed the tweets that contained less than 40% Devanagari characters. Two machine learning models were used namely Multinomial Naïve Bayes and Logistic Regression. To train these models we first created a term frequency-inverse document frequency vector for the data set and fed this tf-idf matrix as input to these algorithms. We also used a deep learning Sequential Model using the Bag of words approach. We created a bag of words for the given data set using Keras Tokenizer and fed it as input to the model. The model has two layers. The first layer has 512 nodes and the second layer has two. Relu activation function has been used in the first layer and the SoftMax activation on the second layer. 

>All the models have been uploaded in .sav/.json format and are present inside the models folder. The models were created using the code in models.py


## Testing and Results


The models were tested on the test set derived from the data set described in the previous section. We achieved a pretty good accuracy score for all the algorithms that we used for the classification of abusive tweets. The Multinomial Naïve Bayes Algorithm achieved an accuracy of 81.5% while the Logistic Regression achieved an accuracy score of 85%. The Bag of Words Model using Keras, after 5 epochs, achieved a training accuracy score of 95%. The snapshots of the graphical visualization can be seen below for a better understanding of the output.

>The testing code is in test_models.py.

**Some Snapshots**

![Logistic Regression Confusion Matrix](https://github.com/gagantalreja/tweet-inspector/blob/master/images/Logistic%20Regression.png "Logistic Regression Confusion Matrix")

![Multinomial Naive Bayes Confusion Matrix](https://github.com/gagantalreja/tweet-inspector/blob/master/images/Multinomial%20Naive%20Bayes.png "Multinomial Naive Bayes Confusion Matrix")

![Accuracy Comparison of all algorithms](images/acc_comp.png "Accuracy Comparison of all algorithms")

![Bag of Words Accuracy Variation through all epochs](images/model_acc.png "Bag of Words Accuracy Variation through all epochs")

![Bag of Words Loss Variation through all epochs](images/model_loss.png "Bag of Words Loss Variation through all epochs")

## The Interface

The interface has been created using HTML, CSS on the frontend and Flask at the backend. The web application has been deployed on Heroku and can be accessed [here](https://tweet-inspector-app.herokuapp.com/).

We have also created an Application Programming Interface (API) for the models using Flask that is free to access and can be integrated with any programming language. The API has been designed using the concepts of REST. It accepts the tweet data from the user in JSON format and returns the predictions made by all users in JSON format. 

### UI Snapshots

![GUI-1](images/gui1.png)



![GUI-2](images/gui2.png)


### Using the REST-API

The API can be accessed by sending a request to the URL: `https://tweet-inspector-api-v2.herokuapp.com/predict`
> The API accepts JSON input and returns a JSON output.

**API call through curl**

```
curl -i -H "Content-Type: application/json" -X POST -d "[{\"text\":\"वे एक संवेदनशील लेखक, सचेत नागरिक, कुशल वक्ता तथा सुधी (विद्वान) संपादक थे\", \"value\": 0}]" https://tweet-inspector-api-v2.herokuapp.com/predict
```

**API call through python requests module**

```python3
import requests
import json

txt = "वे एक संवेदनशील लेखक, सचेत नागरिक, कुशल वक्ता तथा सुधी संपादक थे"
value = 0
json_inp = json.dumps([{'text': txt, 'value': value}])
r = requests.post("https://tweet-inspector-api-v2.herokuapp.com/predict", data = json_inp, headers = {'Content-Type': 'application/json'})
print(r.json())
```

## References

- Raghav Kapoor and Yaman Kumar and Kshitij Rajput and Rajiv Ratn Shah and Ponnurangam Kumaraguru and Roger Zimmermann, 2018. Mind Your Language: Abuse and Offense Detection for Code-Switched Languages. 1809.08652. arXiv. [Link](https://arxiv.org/pdf/1809.08652.pdf)

- J. Ratkiewicz, M. D. Conover, M. Meiss, B. Gonc¸alves, A. Flammini, F. Menczer. 2011. Detecting and Tracking Political Abuse in Social Media. [Link](http://www.cse.fau.edu/~xqzhu/courses/cap6777/political.abuse.social.media.pdf)

- Multi-Class Text Classification Model Comparison and Selection. 2018. Susan Li. Towards Data Science. [Link](https://towardsdatascience.com/multi-class-text-classification-model-comparison-and-selection-5eb066197568)

- Aditya Gaydhani, Vikrant Doma, Shrikant Kendre and Laxmi Bhagwat. 2018. Detecting Hate Speech and Offensive Language on Twitter using Machine Learning: An N-gram and TFIDF based Approach. arXiv. [Link](https://arxiv.org/pdf/1809.08651.pdf)

- Choudhary, Himanshu and Pathak, Aditya Kumar and Shah, Rajiv Ratn and Kumaraguru, Ponnurangam. 2018. Neural Machine Translation for English-Tamil. WMT in EMNLP 2018. [Link](http://precog.iiitd.edu.in/pubs/WMT-EMNLP-2018.pdf)

- Gupta, Divam and Sen, Indira and Sachdeva, Niharika and Kumaraguru, Ponnurangam and Balaji, Arun Buduru. 2018. Empowering First Responders through Automated Multimodal Content Moderation. 2018 IEEE International Conference on Cognitive Computing (ICCC). [Link](https://www.researchgate.net/publication/327497679_Empowering_First_Responders_through_Automated_Multimodal_Content_Moderation)


## Credits

This was a semester project made at Bennett University by our team of three members. The people that contributed in making this project:
- [Gagan Talreja](https://github.com/gagantalreja)
- [Aditya Bhardwaj]()
- Abhinav Rastogi

Our Project Mentor: [Dr. Sridhar Swaminathan](https://github.com/sridarah)


