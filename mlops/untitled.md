# Machine Learning in Production

## Introduction

> **MLOPS**: discipline of building and maintaining production systems, includes processes and tools

In ML production systems you need to handle whole range of issues, including things like data drift, where the distribution of the data you trained on maybe eventually become different, very different from the distribution of the data that you're running inference on.. The world changes and your model needs to be aware of that change.&#x20;

If you're working on a machine learning team and industry, you really need expertise in both machine learning and software to be successful. This is because your team will not just be producing a single result. You'll be developing a product or service that will operate continuously and maybe a mission critical part of your company's work.

Oftentimes the most challenging aspects of building machine learning systems turn out to be the things that you least expected like deployment. It's all very well being able to build a model, but getting that into people's hands and seeing how they use it can be very eye-opening. You might think you have the perfect model for the perfect scenario, but your users could have different opinions, and it's always really good to learn from them. They might be, for example, okay with a round robin trips with server for a frequently updated model. Or they might insist that their data never leaves their device, so you need to know the best ways to keep their own device models really fresh.&#x20;

## Overview of the ML Lifecycle and Deployment

### The Machine Learning Project Lifecycle

Machine learning models are great, but unless you know how to put them into production, it's hard to get them to create the maximum amount of possible value.&#x20;

After you have trained a neural network to take as input X pictures of phones and map down to y predictions about whether the phone is defective or not, you still have to take this machine learning model and put it in a production server, setup API interfaces and write all of the rest of the software in order to deploy this learning algorithm into production. This prediction server is sometimes in the cloud and sometimes the prediction server is actually at the edge. In fact in manufacturing we use edge deployments a lot, because you can't have your factory go down every time your internet access goes down. But cloud deployments also used for many applications.&#x20;

> **Example**: Let's start with an example, let's say you're using computer vision to inspect phones coming off a manufacturing line to see if there are defects on them. So this phone shown on the left doesn't have any stretches on it. But if there was a stretch of crack or something, a computer vision algorithm would hopefully be able to find this type of stretch, or defect. And maybe put the bounding box around it as part of quality control. If you get a data set of scratched phones you can train a computer vision algorithm maybe in your network to detect these types of defects.
>
> This would be an example of how you could deploy a system like this. You might have an edge device. By edge device, I mean a device that is living inside the factory that is manufacturing these smartphones. And that edge device would have a piece of inspection software whose job it is to take a picture of the phone, see if there's a stretch and then make a decision on whether this phone is acceptable, or not. This is actually commonly done in factories is called automated visual defect inspection. What the inspection software does is it will control camera that will take a picture of the smartphone as it rolls off the manufacturing line. And it then has to make an API call to pass this picture to a prediction server. And the job of the prediction server is to accept these API calls, receive an image, make a decision as to whether or not this phone is defective and return this prediction. And then the inspection software it can make the appropriate control decision whether to let it still move on in the manufacturing line. Or whether to shove it to a side, because it was defective and not acceptable.

![Deployment Server](<../.gitbook/assets/image (83).png>)

There can still be quite a lot of work and challenges ahead to get a valuable production deployment running.

> **Example**: Let's say your training sets has images that look like this. There's a good phone on the left, the one in the middle, it has a big scratch across it and you've trained your learning algorithm to recognize that things like this on the left are okay. Meaning that no defects and maybe draw bounding boxes around scratches or other defects that finds and films. When you deploy it in the factory, you may find that the real life production deployment gives you back images like this much darker ones. Because the lighting factory, because the lighting conditions in the factory have changed for some reason compared to the time when the training set was collected. This problem is sometimes called concept drift or data drift.
>
>

![Visual Inspection example](<../.gitbook/assets/image (77).png>)

A second challenge of deploying machine learning models and production is that it takes a lot more than machine learning code.

![](<../.gitbook/assets/image (76).png>)

Beyond the machine learning codes, there are also many other components for managing the data, such as data collection, data verification, feature extraction. Beyond the machine learning codes, there are also many other components for managing the data, such as data collection, data verification, feature extraction.

![](<../.gitbook/assets/image (81).png>)

### Full life cycle of a machine learning project

#### Major steps of a Machine Learning project

![](<../.gitbook/assets/image (73).png>)

First is scoping, in which you have to define the project or decide what to work on. What exactly do you want to apply Machine Learning to, and what is X and what is Y. After having chosen the project, you then have to collect data or acquire the data you need for your algorithm. This includes defining the data and establishing a baseline, and then also labeling and organizing the data

After you have your data, you then have to train the model. During the model phase, you have to select and train the model and also perform error analysis. During the process of error analysis, you may go back and update the model, or you may also go back to the earlier phase and decide you need to collect more data as well. As part of error analysis before taking a system to deployments, I'll often also carry out a final check, maybe a final audit, to make sure that the system's performance is good enough and that it's sufficiently reliable for the application.

when you deploy a system for the first time, you are maybe about halfway to the finish line, because it's often only after you turn on live traffic that you then learn the second half of the important lessons needed in order to get the system to perform well. To carry out the deployment step, you have to deploy it in production, write the software needed to put into production, Then also monitor the system, track the data that continues to come in, and maintain the system.&#x20;

> Example: if the data distribution changes, you may need to update the model.

After the initial deployment, maintenance will often mean going back to perform more error analysis and maybe retrain the model, or it might mean taking the data you get back. Now that the system is deployed and is running on live data, and feeding that back into your dataset to then potentially update your data, retrain the model, and so on until you can put an updated model into deployment.

### Case study: speech recognition

![](<../.gitbook/assets/image (70).png>)

![](<../.gitbook/assets/image (72).png>)

![](<../.gitbook/assets/image (74).png>)

> Example: first step of that was scoping have to first define the project and just make a decision to work on speech recognition, say for voice search as part of defining the project. That also encourage you to try to estimate or maybe at least estimate the key metrics. This will be very problem dependence. Almost every application will have his own unique set of goals and metrics. But the case of speech recognition, some things I cared about where how accurate is the speech system was the latency? How long does the system take to transcribe speech and what is the throughput? How many queries per second we handle. And then if possible, you might also try to estimate the resources needed. So how much time, how much compute how much budget as well as timeline. How long will it take to carry out this project?
>
> The next step is the data stage where you have to define the data and establish a baseline and also label and organize the data. I would probably prefer either the first or the second, not the third. But what what hurts your learning algorithm's performance is if one third of the transcription is used the first, one third, the second and one third third way of trans driving. Because then your data is inconsistent and confusing for the learning algorithm because how is the learning algorithm supposed to guess which one of these conventions specific transcription has happened to use for an audio clip. So Spotting correcting consistencies like that. Maybe just asking everyone to standardize on this first convention that can have a significant impact on your learning algorithm's performance.
>
> Other examples of data definition questions for an audio clip like today's whether, how much silence do you want before and after each clip after a speaker has stopped speaking. Do you want to include another 100 milliseconds of silence after that? Or 300 milliseconds or 500 milliseconds, half a second? Or how do you perform volume normalization? Some speakers speak loudly, some are less loud and then there's actually a tricky case of if you have a single audio clip with some really loud volume and some really soft volume, all within the same audio clip. So how do you perform volume normalization questions like all of these are data definition questions
>
> After you've collected your data set, the next step is modeling, in which you have to select and train the model and perform error analysis. **The three key inputs** that go into training a machine learning model are, the **code,** that is the algorithm or the neural network model architecture that you might choose, you also have to pick **hyper parameters** and then there's the **data **and running the code with your hyper parameters on your data gives you the machine learning model

{% hint style="info" %}
A lot of machine learning research was driven by researchers working to improve performance on benchmark data set. If you are working on a production system then you don't have to keep the data set fix. you may edit the training set or even the test set if that's what's needed in order to improve the data quality to get a production system to work better.
{% endhint %}

{% hint style="info" %}
In research work or academic work you tend to hold the data fixed and vary the code and may be vary the hyper parameters in order to try to get good performance.

In contrast, I found that for a lot of product teams, if your main goal is to just build and deploy a working valuable machine learning system, I found that it can be even more effective to hold the code fixed and to instead focus on optimizing the data and maybe the hyper parameters, in order to get a high performing model.

**Often if error analysis can tell you how to systematically improve the data, that can be a very efficient way for you to get to a high accuracy model**
{% endhint %}

![](<../.gitbook/assets/image (79).png>)

> **Example**: here's something that happened to me once my team had built a speech recognition system and it was trained mainly on adult voices. We pushed into production, random production and we found that over time more and more young individuals, kind of teenagers, you know, sometimes even younger seem to be using our speech recognition system and the voices are very young individuals just sound different. And so my speech systems performance started to degrade. We just were not that good at recognizing speech as spoken by younger voices. And so he had to go back and find a way to collect more data are the things in order to fix it.

One of the key challenges when it comes to deployment is **concept drift or data drift**, which is what happens when the data distribution changes, such as there are more and more young voices being fed to the speech recognition system.&#x20;

{% hint style="warning" %}
Knowing how to put in place appropriate monitors to spot such problems and then also how to fix them in a timely way is a key skill needed to make sure your production deployment creates a value.
{% endhint %}

* After reaching the milestone of developing a good model, actually putting your model into production, so that you have a working system that is making useful predictions that requires much more
