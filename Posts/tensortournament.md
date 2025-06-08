---
blog-title: The Tensor Tournament 2023
blog-subtitle: Hackathon at FAU - organized by Machine Learning and Data Analytics Lab
blog-published: 2023-02-03
blog-tags:
  - EN
  - Hackathon
  - Python
  - ML
  - Data Science
---

I had the opportunity to take part in [The Tensor Tournament Hackathon](https://www.mad.tf.fau.de/2023/01/26/the-tensor-tournament-t3-recap/) (organized by the [Machine Learning and Data Analytics Lab](https://www.mad.tf.fau.de/) at FAU) and I am happy to announce that our team, [Maximilian Kasper](https://www.linkedin.com/in/maximilian-kasper-693648174/) and [me](https://www.linkedin.com/in/marcjulian/), emerged as the winners!
The hackathon was a six-hour event where teams had to solve three machine learning and deep learning problems. It was an intense and challenging experience, but also incredibly rewarding to put our skills to the test and come out on top.

![Winning Team and Organizers](/images/ttt_winners.jpg)

## First Problem: The Gingerbread Chef 

The first problem was to build a machine learning model that could accurately **predict the quality** of gingerbread on a scale of 0 (garbage) to 4 (delicious) based on the ingredients. This required us to use regression or classification models. After trying some models we ended up using a simple regression model. Our scores were good but not as good as we'd like, so we decided to try a rather unconventional approach by leveraging the AutoML package [AutoGluon](https://auto.gluon.ai/stable/index.html) to automatically train and evaluate lots of models with various hyperparameters. In the end, the model produced by AutoGluon was better than any of the other submissions. Apparently, simple problems become less relevant as automatic modeling gets better.

![Score Leaderboard](/images/ttt_scores.jpg)

## Second Problem: Keep Your Distance

> Adaptive cruise control is a driver assist feature that automatically keeps a set distance to the car in front, or maintains the set speed if there is none. The correct estimation of the distance between yourself and other vehicles is thus paramount. Instead of costly radar or lidar sensors you intend to use a simple video camera for that purpose.

The dataset for the next problem consisted of a total of 1074 images with 1442 vehicle bounding boxes and their measured distance to the car.

The goal of this problem was to **predict the distance** from the given images and bounding boxes for each vehicle. To get a good first baseline we decided to use the tabular annotation data and leave the images for a later model. 
To our surprise, after some feature engineering, a simple random forest regressor evaluated to a score of 0.95 on the test set. We suspect that the  features *"area of  bounding box"* and "*angle to camera*" were enough to explain the distance. The area has a direct causal relationship with the distance. 

![Example Train Image with cars and annotations](/images/ttt_cars.jpg)

## Final Problem: Find The Numbers

The final problem was by far the hardest one as it had an unusual train test split. The train dataset consisted of 24 images while the test set had over 10,000 images. Each image contained a random number of [MNIST](https://www.tensorflow.org/datasets/catalog/mnist) digits between 0 and 9 with random sizes in random positions. 
The task was to **recognize all digits** in the given images. The prediction for the following image could for example be *776186792825*. The order of digits doesn't matter, so *277698718625* would also be a valid prediction. 

![Example Train Image](/images/tensor_tournament.jpg)

The first approach we tried was to use an OCR-tool like [Python-tesseract](https://pypi.org/project/pytesseract/) to directly recognize the digits embedded in the image. After some preprocessing of the image (e.g. inverting the colors) we were able to classify some images, but the accuracy wasn't great. 

We know that the images are from the MNIST dataset. As a lot of good models already exist for classifying MNIST digits, we decided to try cutting out the digits and classify each digit separately. 
We used [OpenCV](https://opencv.org/) functions to cut out the digits, scale them to the correct size, sharpen the images, and perform other preprocessing steps to make them look as similar as possible to the MNIST digits. 
We then trained a simple MNIST model and used it to classify each image in the test set. This approach worked much better than the previous OCR attempt. 

## Conclusion

The hackathon was an incredible experience and I am grateful to have had the opportunity to be part of it. I would like to thank the organizers of the hackathon for putting on such a great event, and to Maximilian for being an incredible teammate. I am excited to see what the future holds for us in the world of data science. 

![The Tensor Tournament Banner](/images/ttt_plakat.jpg)