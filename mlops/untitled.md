# Machine Learning in Production

## Introduction

> **MLOPS**: discipline of building and maintaining production systems, includes processes and tools

In ML production systems you need to handle whole range of issues, including things like data drift, where the distribution of the data you trained on maybe eventually become different, very different from the distribution of the data that you're running inference on.. The world changes and your model needs to be aware of that change.&#x20;

If you're working on a machine learning team and industry, you really need expertise in both machine learning and software to be successful. This is because your team will not just be producing a single result. You'll be developing a product or service that will operate continuously and maybe a mission critical part of your company's work.

Oftentimes the most challenging aspects of building machine learning systems turn out to be the things that you least expected like deployment. It's all very well being able to build a model, but getting that into people's hands and seeing how they use it can be very eye-opening. You might think you have the perfect model for the perfect scenario, but your users could have different opinions, and it's always really good to learn from them. They might be, for example, okay with a round robin trips with server for a frequently updated model. Or they might insist that their data never leaves their device, so you need to know the best ways to keep their own device models really fresh.&#x20;

## Overview of the ML Lifecycle and Deployment

### References

#### Overview of the ML Lifecycle and Deployment

If you wish to dive more deeply into the topics covered this week, feel free to check out these optional references. You won’t have to read these to complete this week’s practice quizzes.

1. [Concept and Data Drift](https://towardsdatascience.com/machine-learning-in-production-why-you-should-care-about-data-and-concept-drift-d96d0bc907fb)
2. [Monitoring ML Models](https://christophergs.com/machine%20learning/2020/03/14/how-to-monitor-machine-learning-models/)
3. [A Chat with Andrew on MLOps: From Model-centric to Data-centric](https://youtu.be/06-AZXmwHjo)

**Papers**

* Konstantinos, Katsiapis, Karmarkar, A., Altay, A., Zaks, A., Polyzotis, N., … Li, Z. (2020). Towards ML Engineering: A brief history of TensorFlow Extended (TFX). [http://arxiv.org/abs/2010.02013 ](http://arxiv.org/abs/2010.02013)
* Paleyes, A., Urma, R.-G., & Lawrence, N. D. (2020). Challenges in deploying machine learning: A survey of case studies. [http://arxiv.org/abs/2011.09926](http://arxiv.org/abs/2011.09926)
* Sculley, D., Holt, G., Golovin, D., Davydov, E., & Phillips, T. (n.d.). Hidden technical debt in machine learning systems. Retrieved April 28, 2021, from Nips.c[ https://papers.nips.cc/paper/2015/file/86df7dcfd896fcaf2674f757a2463eba-Paper.pdf](https://papers.nips.cc/paper/2015/file/86df7dcfd896fcaf2674f757a2463eba-Paper.pdf)

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

![Visual Inspection example](<../.gitbook/assets/image (77) (1).png>)

A second challenge of deploying machine learning models and production is that it takes a lot more than machine learning code.

![](<../.gitbook/assets/image (76).png>)

Beyond the machine learning codes, there are also many other components for managing the data, such as data collection, data verification, feature extraction. Beyond the machine learning codes, there are also many other components for managing the data, such as data collection, data verification, feature extraction.

![](<../.gitbook/assets/image (81).png>)

### Full life cycle of a machine learning project

#### Major steps of a Machine Learning project

![](<../.gitbook/assets/image (73) (1).png>)

First is scoping, in which you have to define the project or decide what to work on. What exactly do you want to apply Machine Learning to, and what is X and what is Y. After having chosen the project, you then have to collect data or acquire the data you need for your algorithm. This includes defining the data and establishing a baseline, and then also labeling and organizing the data

After you have your data, you then have to train the model. During the model phase, you have to select and train the model and also perform error analysis. During the process of error analysis, you may go back and update the model, or you may also go back to the earlier phase and decide you need to collect more data as well. As part of error analysis before taking a system to deployments, I'll often also carry out a final check, maybe a final audit, to make sure that the system's performance is good enough and that it's sufficiently reliable for the application.

when you deploy a system for the first time, you are maybe about halfway to the finish line, because it's often only after you turn on live traffic that you then learn the second half of the important lessons needed in order to get the system to perform well. To carry out the deployment step, you have to deploy it in production, write the software needed to put into production, Then also monitor the system, track the data that continues to come in, and maintain the system.&#x20;

> Example: if the data distribution changes, you may need to update the model.

After the initial deployment, maintenance will often mean going back to perform more error analysis and maybe retrain the model, or it might mean taking the data you get back. Now that the system is deployed and is running on live data, and feeding that back into your dataset to then potentially update your data, retrain the model, and so on until you can put an updated model into deployment.

### Case study: speech recognition

![](<../.gitbook/assets/image (70).png>)

![](<../.gitbook/assets/image (72) (1) (1).png>)

![](<../.gitbook/assets/image (74) (1) (1).png>)

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

![](<../.gitbook/assets/image (79) (1).png>)

> **Example**: here's something that happened to me once my team had built a speech recognition system and it was trained mainly on adult voices. We pushed into production, random production and we found that over time more and more young individuals, kind of teenagers, you know, sometimes even younger seem to be using our speech recognition system and the voices are very young individuals just sound different. And so my speech systems performance started to degrade. We just were not that good at recognizing speech as spoken by younger voices. And so he had to go back and find a way to collect more data are the things in order to fix it.

### Deployment

#### Data Drift / Concept Drift issue

One of the key challenges when it comes to deployment is **concept drift or data drift**, which is what happens when the data distribution changes after deployment, such as there are more and more young voices being fed to the speech recognition system.

![](<../.gitbook/assets/image (71).png>)

> Example: I train a few speech recognition systems, and when I built speech systems, quite often I would have some purchased data. This would be some purchased or licensed data, which includes both the input x, the audio, as well as the transcript y that the speech system supports it's output. In addition to data that you might purchase from a vendor, you might also have historical user data of user speaking to your application together with transcripts of that raw user data. Such user data, of course, should be collected with very clear user opt-in permission and clear safeguards for user privacy. After you've trained your speech recognition system on a data set like this, you might then evaluate it on a test set, but because speech data does change over time, when I build speech recognition systems, sometimes I would collect a dev set or hold out validation set as well as test set comprising data from just the last few months. You can test it on fairly recent data to make sure your system works, even on relatively recent data. After you push the system to deployments, the question is, will the data change or after you've run it for a few weeks or a few months, has the data changed yet again? The data has changed, such as the language changes or maybe people are using a brand new model of smartphone which has a different microphone, so the audio sounds different, then the performance of a speech recognition system can degrade.

It's important for you to recognize how the data has changed, and if you need to update your learning algorithm as a result. When data changes, sometimes it is a gradual change, such as the English language which does change, but changes very slowly with new vocabulary introduced at a relatively slow rate. Sometimes data changes very suddenly where there's a sudden shock to a system.  When you deploy a machine learning system, one of the most important tasks, will often be to make sure you can detect and manage any changes. Including both Concept drift, which is when the definition of what is y given x changes. As well as Data drift, which is if the distribution of x changes, even if the mapping from x or y does not change.&#x20;

{% hint style="warning" %}
Knowing how to put in place appropriate monitors to spot such problems and then also how to fix them in a timely way is a key skill needed to make sure your production deployment creates a value.
{% endhint %}

> Example: When COVID-19 the pandemic hit, a lot of credit card fraud systems started to not work because the purchase patterns of individuals suddenly changed. Many people that did relatively little online shopping suddenly started to use much more online shopping. The way that people were using credit cards changed very suddenly, and his actually tripped up a lot of anti fraud systems. This very sudden shift to the data distribution meant that many machine learning teams were scrambling a little bit at the start of COVID to collect new data and retrain systems in order to make them adapt to this very new data distribution.&#x20;

> Example: sometimes the term data drift is used to describe if the input distribution x changes, such as if a new politician or celebrity suddenly becomes well known and he's mentioned much more than before. The term concept drift refers to if the desired mapping. From x to y changes such as if, before COVID-19. Perhaps for a given user, a lot of surprising online purchases, should have flagged that account for fraud. After the start of COVID-19, maybe those same purchases, would not have really been any cause for alarm, in terms of flagging. That the credit card may have been stolen. Another example of Concept drift, let's say that x is the size of a house, and y is the price of a house, because you're trying to estimate housing prices. If because of inflation or changes in the market, houses may become more expensive over time. The same size house, will end up with a higher price. That would be Concept drift. Maybe the size of houses haven't changed, but the price of a given house changes. Whereas data drift would be if, say, people start building larger houses, or start building smaller houses and thus the input distribution of the sizes of houses actually changes over time.

#### Software engineering issues

You are implementing a prediction service whose job it is to take queries x and output prediction y, you have a lot of design choices as to how to implement this piece of software. Here's a checklist of questions, that might help you with making the appropriate decisions for managing the software engineering issues.

![](<../.gitbook/assets/image (79).png>)

* Do you need Real time predictions or are Batch predictions?&#x20;
  * if you are building a speech recognition system, where the user speaks and you need to get a response back, in half a second, then clearly you need real time predictions
  * Hospitals that take patient records, Take electronic health records and run an overnight batch process to see if there's something associated with the patients, that we can spot. In that type of system, it was fine if we just ran it, in a batch of patient records once per night
* does your prediction service run into clouds or does it run at the edge or maybe even in a Web browser?&#x20;
  * &#x20;A lot of speech systems within cars, actually run at the edge. There are also some mobile speech recognition systems that work, even if your Wi-Fi is turned off. Those would be examples of speech systems that run at the edge.&#x20;
  * When I am deploying visual inspection systems in factories, I pretty much almost always run that at the edge as well. Because sometimes unavoidably, the Internet connection between the factory, and the rest of the Internet may go down. You just can't afford to shut down the factory, whenever its Internet connection goes down, which happens very rarely but maybe sometimes does happen.
  * With the rise of modern Web browsers, there are better tools, for deploying learning algorithms, right there within a Web browser as well.
* When building a prediction service, it's also useful to take into account, how much computer resources you have.
  * There have been quite a few times where I trained a neural network on a very powerful GPU, only to realize that I couldn't afford an equally powerful set of GPUs for deployments, and wound up having to do something else to compress or reduce the model complexity.
  * If you know how much CPU or GPU resources and maybe also how much memory resources you have for your prediction service, then that could help you choose the right software architecture.
* Depending on your application especially if it's real-time application, latency and throughputs such as measured in terms of QPS, queries per second, will be other software engineering metrics you may need to hit.&#x20;
  * In speech recognition is not uncommon to want to get an answer back to the user, within half a second or 500 milliseconds. Of this 500 millisecond budget you may be able to allocate only say, 300 milliseconds to your speech recognition. That gives a latency requirement for your system.
  * Throughput refers to how many queries per second do you need to handle given your compute resources, maybe given a certain number of Cloud Service. For example, if you're building a system that needs to handle 1000 queries per second, it would be useful to make sure to check out your system so that you have enough computer resources
* Next is logging, when building your system it may be useful to log as much of the data as possible for analysis and review as well as to provide more data for retraining your learning algorithm in the future.
* Finally, security and privacy, I find it for different applications the required levels of security and privacy can be very different.
  * For example, when I was working on electronic health records, patient records, clearly the requirements for security and privacy were very high because patient records are very highly sensitive information.
  * Depending on your application you might want to design in the appropriate level of security and privacy, based on how sensitive that data is and also sometimes based on regulatory requirements

To summarize, deploying a system requires two broad sets of tasks: there is writing the software to enable you to deploy the system in production. There is what you need to do to monitor the system performance and to continue to maintain it, especially in the face of concepts drift as well as data drift.

The practices for the very first deployments will be quite different compared to when you are updating or maintaining a system that has already previously been deployed. Unfortunately, I think the first deployment means you may be only about halfway there, and the second half of your work is just starting only after your first deployment, because even after you've deployed there's a lot of work to feed the data back and maybe to update the model, to keep on maintaining the model even in the face of changes to the data.

### Deployment patterns

![](<../.gitbook/assets/image (75) (1).png>)

One type of deployment is if you are offering a new product or capability that you had not previously offered. For example, if you're offering a speech recognition service that you have not offered before, in this case, a common design pattern is to start up a small amount of traffic and then gradually ramp it up.&#x20;

A second common deployment use case is if there's something that's already being done by a person, but we would now like to use a learning algorithm to either automate or assist with that task. For example, if you have people in the factory inspecting smartphones scratches, but now you would like to use a learning algorithm to either assist or automate that task. The fact that people were previously doing this gives you a few more options for how you deploy. And you see shadow mode deployment takes advantage of this.

finally, a third common deployment case is if you've already been doing this task with a previous implementation of a machine learning system, but you now want to replace it with hopefully an even better one. In these cases, two recurring themes you see are that you often want a gradual ramp up with monitoring. In other words, rather than sending tons of traffic to a maybe not fully proven learning algorithm, you may send it only a small amount of traffic and monitor it and then ramp up the percentage or amount of traffic. And the second idea you see a few times is rollback. Meaning that if for some reason the algorithm isn't working, it's nice if you can revert back to the previous system if indeed there was an earlier system.

![](<../.gitbook/assets/image (85).png>)

![](<../.gitbook/assets/image (77).png>)

> Example: In visual inspection where perhaps you've had human inspectors inspect smartphones for defects for scratches. And you would now like to automate some of this work with a learning algorithm. When you have people initially doing a task, one common deployment pattern is to use shadow modes deployment. And what that means is that you will start by having a machine learning algorithm shadow the human inspector and running parallel with the human inspector. During this initial phase, the learning algorithms output is not used for any decision in the factory. So whatever the learning algorithm says, we're going to go the human judgment for now. So let's say for this smartphone the human says it's fine, no defect. The learning algorithm says it's fine. Maybe for this example of a big stretch down the middle person says it's not okay and the learning algorithm agrees. And maybe for this example with a smaller stretch, maybe the person says this is not okay, but the learning algorithm makes a mistake and actually thinks this is okay. The purpose of a shadow mode deployment is that allows you to gather data of how the learning algorithm is performing and how that compares to the human judgment. And by something the it offers you can then verify if the learning algorithm's predictions are accurate and therefore use that to decide whether or not to maybe allow the learning algorithm to make some real decisions in the future. So when you already have some system that is making good decisions and that system can be human inspectors or even an older implementation of a learning algorithm. Using a shadow mode deployment can be a very effective way to let you verify the performance of a learning algorithm before letting them make any real decisions.
>
> When you are ready to let a learning algorithm start making real decisions, a common deployment pattern is to use a canary deployment. in a canary deployments you would roll out to a small fraction, maybe 5%, maybe even less of traffic initially and start let the algorithm making real decisions. But by running this on only a small percentage of the traffic, hopefully, if the algorithm makes any mistakes it will affect only a small fraction of the traffic. And this gives you more of an opportunity to monitor the system and ramp up the percentage of traffic it gets only gradually and only when you have greater confidence in this performance. The phrase canary deployment is a reference to the English idiom or the English phrase canary in a coal mine, which refers to how coal miners used to use canaries to spot if there's a gas leak. But with canary the deployment, hopefully this allows you to spot problems early on before there are maybe overly large consequences to a factory or other context in which you're deploying your learning algorithm.&#x20;
>
> Another deployment pattern that is sometimes used is a blue green deployment. Let me explain with the picture. Say you have a system, a camera software for collecting phone pictures in your factory. These phone images are sent to a piece of software that takes these images and routes them into some visual inspection system. In the terminology of a blue green deployments, the old version of your software is called the blue version and the new version, the Learning algorithm you just implemented is called the green version. In a blue green deployment, what you do is have the router send images to the old or the blue version and have that make decisions. And then when you want to switch over to the new version, what you would do is have the router stop sending images to the old one and suddenly switch over to the new version. So the way the blue green deployment is implemented is you would have an old prediction service may be running on some sort of service. You will then spin up a new prediction service, the green version, and you would have the router suddenly switch the traffic over from the old one to the new one. The advantage of a blue green deployment is that there's an easy way to enable rollback. If something goes wrong, you can just very quickly have the router go back reconfigure their router to send traffic back to the old or the blue version, assuming that you kept your blue version of the prediction service running. In a typical implementation of a blue green deployment, people think of switching over the traffic 100% all at the same time. But of course you can also use a more gradual version where you slowly send traffic over.&#x20;
>
>

whether use shadow mode, canary mode, blue green, or some of the deployment pattern, quite a lot of software is needed to execute this. MLOps tools can help with implementing these deployment patterns

One of the most useful frameworks I have found for thinking about how to deploy a system is to think about deployment not as a 0, 1 is either deploy or not deploy, but instead to design a system thinking about what is the appropriate degree of automation.&#x20;

![](<../.gitbook/assets/image (84) (1).png>)

> For example, in visual inspection of smartphones, one extreme would be if there's no automation, so the human only system. Slightly mode automated would be if your system is running a shadow mode. So your learning algorithms are putting predictions, but it's not actually used in the factory. So that would be shadow mode. A slightly greater degree of automation would be AI assistance in which given a picture like this of a smartphone, you may have a human inspector make the decision. But maybe an AI system can affect the user interface to highlight the regions where there's a scratch to help draw the person's attention to where it may be most useful for them to look. The user interface or UI design is critical for human assistance. But this could be a way to get a slightly greater degree of automation while still keeping the human in the loop.&#x20;
>
> And even greater degree of automation maybe partial automation, where given a smartphone, if the learning algorithm is sure it's fine, then that's its decision. It is sure it's defective, then we just go to algorithm's decision. But if the learning algorithm is not sure, in other words, if the learning algorithm prediction is not too confident, 0 or 1, maybe only then do we send this to a human. So this would be partial automation. Where if the learning algorithm is confident of its prediction, we go the learning algorithm. But for the hopefully small subset of images where the algorithm is not sure we send that to a human to get their judgment. And the human judgment can also be very valuable data to feedback to further train and improve the algorithm.
>
>

partial automation is sometimes a very good design point for applications where the learning algorithms performance isn't good enough for full automation.

![](<../.gitbook/assets/image (74) (1).png>)

there is a spectrum of using only human decisions on the left, all the way to using only the AI system's decisions on the right. And many deployment applications will start from the left and gradually move to the right. And you do not have to get all the way to full automation. You could choose to stop using AI assistance or partial automation or you could choose to go to full automation depending on the performance of your system and the needs of the application.&#x20;

> On this spectrum both AI assistance and partial automation are examples of human in the loop deployments. I find that the consumer Internet applications such as if you run a web search engine, write online speech recognition system. A lot of consumer software Internet businesses have to use full automation because it's just not feasible to someone on the back end doing some work every time someone does a web search or does the product search. But outside consumer software Internet, for example, inspecting things and factories. They're actually many applications where the best design point maybe a human in the loop deployments rather than a full automation deployment.&#x20;
>
>

### Monitoring

The most common way to monitor a machine learning system is to use a dashboard to track how it is doing over time. Depending on your application, your dashboards may monitor different metrics.

![](<../.gitbook/assets/image (80).png>)

> For example, you may have one dashboard to monitor the server load, or a different dashboards to monitor diffraction of non-null outputs. Sometimes a speech recognition system output is null when the things that users didn't say anything. If this changes dramatically over time, it may be an indication that something is wrong, or one common one I've seen for a lot of structured data task is monitoring the fraction of missing input values. If that changes, it may mean that something has changed about your data. When you're trying to decide what to monitor, my recommendation is that you sit down with your team and brainstorm all the things that could possibly go wrong. Then you want to know about if something does go wrong. For all the things that could go wrong, brainstorm a few statistics or a few metrics that will detect that problem. For example, if you're worried about user traffic spiking, causing the service to become overloaded, then server loads maybe one metric, you could track and so on for the other examples here. When I'm designing my monitoring dashboards for the first time, I think it's okay to start off with a lot of different metrics and monitor a relatively large set and then gradually remove the ones that you find over time not to be particularly useful.

![](<../.gitbook/assets/image (72) (1).png>)

First are the software metrics, such as memory, compute, latency, throughput, server load, things that help you monitor the health of your software implementation of the prediction service or other pieces of software around your learning algorithm. But these software metrics will help you make sure that your software is running well. Many MLOps tools will come over the bouts already tracking these software metrics.

In addition to the software metrics, I would often choose other metrics that help monitor the statistical health or the performance of the learning algorithm. Broadly, there are two types of metrics you might brainstorm around. One is input metrics, which are metrics that measure has your input distribution x change.&#x20;

> For example, if you are building a speech recognition system, you might monitor the average input length in seconds of the length for the audio clip fed to your system. You might monitor the average input volume. If these change for some reason, that might be something you'll once to take a look at just to make sure this hasn't hurt the performance of your algorithm.&#x20;
>
>

&#x20;number or percentage of missing values is a very common metric. When using structured data, some of which may have missing values, or for the manufacturing visual inspection example, you might monitor average image brightness if you think that lighting conditions could change, and you want to make sure you know if it does, so you can brainstorm different metrics to see if your input distribution x might have changed. A second set of metrics that help you understand if your learning algorithm is performing well are output metrics. Such as, how often does your speech recognition system return null, the empty string, because the things the user doesn't say anything, or if you have built a speech recognition system for web search using voice, you might decide to see how often does the user do two very quick searches in a row with substantially the same input.

Or you could monitor the number of times the user first try to use the speech system and then switches over to typing, that could be a sign that the user got frustrated or gave up on your speech system and could indicate degrading performance. Of course, for web search, you would also use maybe very course metrics like click-through rate or CTR, just to make sure that the overall system is healthy.&#x20;

I encourage you to think of deployments as an iterative process as well. When you get your first deployments up and running and put in place a set of monitoring dashboards. But that's only the start of this iterative process. A running system allows you to get real user data or real traffic. It is by seeing how your learning algorithm performs on real data on real traffic that, that allows you to do performance analysis, and this in turn helps you to update your deployment and to keep on monitoring your system.

&#x20;it usually takes a few tries to converge to the right set of metrics to monitor. Sometimes have deploy the machine learning system, and it's not uncommon for you to deploy machine learning system with an initial set of metrics only to run the system for a few weeks and then to realize that something could go wrong with it that you hadn't thought of before and into pick a new metric to monitor. Or for you to have some metric that you monitor for a few weeks and then decide they're just metric, hardly ever changes in does is inducible, and to get rid of that metric in favor of focusing attention on something else. After you've chosen a set of metrics to monitor, common practice would be to set thresholds for alarms. You may decide based on this set, if the server load ever goes above 0.91, that may trigger an alarm or a notification to let you know or let the team know to see if there's a problem and maybe spin up some more servers. Or if the fashion of non-null plus goals above or beyond certain thresholds that might trigger an alarm. Or if they're not, fraction of missing values goes above or below some set of thresholds, maybe that should trigger an alarm, and it is okay if you adapt the metrics and the thresholds over time to make sure that they are flagging to you the most relevant cases of concern. If something goes wrong with your learning algorithm, if is a software issue such as server load is too high, then that may require changing the software implementation, or if it is a performance problem associated with the accuracy of the learning algorithm, then you may need to update your model. Or if it is an issue associated with the accuracy of the learning algorithm, then you may need to go back to fix that that's why many machine learning models will need a little bit of maintenance or retraining over time. Just like almost all software needs some level of maintenance as well. When a model needs to be updated, you can either retrain it manually, where in Engineer, maybe you will retrain the model perform error analysis and the new model and make sure it looks okay before you push that to deployment. Or you could also put in place a system where there is automatic retraining. Today, manual retraining is far more common than automatically training for many applications developers are reluctant to learning algorithm be fully automatic in terms of deciding to retrain and pushing new model to production, but there are some applications, especially in consumer software Internet, where automatically training does happen.

key takeaways are that it is only by monitoring the system that you can spot if there may be a problem that may cause you to go back to perform a deeper error analysis, or that may cause you to go back to get more data with which you can update your model so as to maintain or improve your system's performance.

### Pipeline monitoring

Many AI systems are not just a single machine learning model running a prediction service, but instead involves a pipeline of multiple steps.

![](<../.gitbook/assets/image (84).png>)

> Let's continue with our speech recognition example, you've seen how a speech recognition system may take as input audio and I'll put a transcript. The way that speech recognition is typically implemented on mobile apps is not like this, but instead is a slightly more complex pipeline. Where the audio is fed to a module called a VAD or a voice activity detection module, whose job it is to see if anyone is speaking. And only if the VAD module, the voice activity detection module thinks someone is speaking, does it then bother to pass the audio on to a speech recognition system whose job it is to then generate the transcript. And the reason we use a voice activity detection or VAD module is because if say, your speech recognition system runs in the cloud. You don't want to stream more bandwidth than you have to to your cloud server. And so the voice activity detection module looks at the long stream of audio on your cell phone and clips or shortens the audio to just the part where someone is talking and streams only that to the cloud server to perform the speech recognition
>
> let's say that because of the way a new cell phone's microphone works. The VAD module ends up clipping the audio differently. Maybe it leaves more silence at the start or end or less silence at the start or end. And does if the VAD's output changes, that will cause the speech recognition systems input to change. And that could cause degraded performance of the speech recognition system

When you have two learning algorithms, changes to the first module may affect the performance of the second module as well.&#x20;

![](<../.gitbook/assets/image (74).png>)

> Let's look an example involving user profiles. Maybe I've used the data such as clickstream data showing what users are clicking on. And this can be used to build a user profile that tries to capture key attributes or key characteristics of a user. For example, I once built user profiles that would try to expect many attributes of users including whether or not the user seemed to own a car. Because this would help us decide if it was worth trying to offer car insurance office to that user. And so whether the user owns a car could be yes or no or unknown, or maybe other final graduations and these. And the typical way that the user profile is built is with a learning algorithm to try to predict if this user of the car. This type of user profile, which can have a very long list of predicted attributes, can then be fed to recommend a system. Another learning algorithm that then takes this understanding of the user to try to generate product recommendations. Now, if something about the click stream data changes, maybe this input distribution changes, then maybe over time if we lose our ability to figure out if a user owns a car, then the percentage of the unknown tag here may go up. And because the user profiles output changes, the input to the recommended system now changes and this might affect the quality of the product recommendations. When you have a machine learning pipelines, these cascading effects in the pipeline can be complex to keep track on. But if the percentage of unknown labels does go up, this could be something that you want to be alerted to so that you can update the recommend the system if needed to make sure you continue to generate high quality product recommendations.

So, metrics to monitor include software metrics for perhaps each of the components in the pipeline, or perhaps for the overall pipeline as a whole. As well as input metrics and potentially output metrics for each of the components of the pipeline. And by brainstorming metrics associated with individual components of the pipeline as well. This could help you spot problems such as the voice activity detection system of putting longer or shorter audio clears over time or the user profile system suddenly having more unknown attributes for whether the user owns a car. And thereby alert you to changes in the data that may require you to take action to maintain the model.

Finally, how quickly does data change? The rate at which data changes is very problem dependent.&#x20;

#### Metrics to monitor

![](<../.gitbook/assets/image (72).png>)

> For example, let's see it built to face recognition system. Then the rate at which people's appearances changes usually isn't that fast. People's hairstyles and clothing does change with fashion changes. And as cameras get better, we've been getting higher and higher resolution pictures of people over time. But for the most part, people's appearances don't change that much. But there are sometimes things that can change very quickly as well, such as if a factory gets a new batch of material for how they make cell phones and so all the cell phones not to change in appearance. So some applications will have data that changes over the time scale of months or even years. And some applications with data that could suddenly change in a matter of minutes.&#x20;

user data, if a large number of users will usually change relatively slowly. There are a few exceptions, of course, COVID-19 being one of them were a shock to society actually cause a lot of people's behavior that all change at the same time. And if you look at web search traffic, you will see trends maybe a holiday or a new movie and people start searching for something new. They just became popular. So there are exceptions, but on average, if you have a very large group of users, there are only a few forces. They can simultaneously change the behavior of a lot of people or at the same time. In contrast, if you work on a B2B or business to business application, I find an enterprise data or business data can shift quite quickly. Because the factory making cellphones may suddenly decide. So use a new coating for the cell phones and suddenly the entire dataset changes because the cell phones suddenly all look different. But if you're providing a machine learning system to a company, then sometimes if the CEO of that company decides to change the way that business operates, all of that data can shift very quickly.&#x20;

