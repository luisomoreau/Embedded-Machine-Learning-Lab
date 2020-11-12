# Introduction to Embedded Machine Learning

With [Edge Impusle](https://www.edgeimpulse.com/) and your smartphone (for the data collection & the inference parts)

For this third lab, I wanted to show you device lifecycles concepts including: 

* What are makefiles
* OTA (Over The Air) Firmware Upgrades 
* How to manage a fleet of devices

However due to the exceptional conditions during the lock-down, this lab will be done remotely. Without access to the microcontrolers, we will be doing an **introduction to Embedded Machine Learning using [Edge Impusle](https://www.edgeimpulse.com/) solution**.

In this lab, we will see three different algorithms, matching different use cases:

1. **Continuous motion detection**: We will use this technique to detect *free-fall patterns*. It could be integrated in an assistance solution for the elderly for example.
2. **Image classification**: It's a famous technique to classify images, if you go further with neural networks, you will probably end up doing a tutorial to recognize a cat from a dog. Here I will give you freedom to classify any type of image, feel free to make something fun.
3. **Trigger word detection**: "Hey Google" or "Alexa" words can trigger your smart speaker. here, we will try to recognize your own word.

*Disclaimer: I am a beginner in AI & Deep Learning. This field got my attention recently and I only passed the Coursera [Neural Networks and Deep Learning Specification](https://www.coursera.org/account/accomplishments/verify/5WBR89U96WJ7) in April 2020. IoT has experienced, in recent years, a new craze boosted by AI (Artificial Intelligence), Blockchain and integration with SaaS applications. The real-time feedback of physical data, making it possible to constantly create new uses.*
 

You won't need the past laboratory for this lab but here they are in case you were missing them or you want to build a complete solution, feel free to have another look:

* [Embedded Programming - Lab 1](https://github.com/luisomoreau/Embedded-Programming-Lab-1)
* [IoT Communication Protocols - Lab 1](https://github.com/luisomoreau/IoT-Communication-Protocols-Lab-1)



## Prerequisites

* Having a smartphone running on Android or iOS

This lab will be graded and has to be done individually.

## What is Embedded Machine Learning

⚠️ Read this following parts carefully, it gives a clear understanding of what will we be doing during the laboratory.

*Source: [https://docs.edgeimpulse.com/docs/what-is-embedded-machine-learning-anyway](https://docs.edgeimpulse.com/docs/what-is-embedded-machine-learning-anyway)*

Machine learning (ML) is a way of writing computer programs. Specifically, it’s a way of writing programs that process raw data and turn it into information that is meaningful at an application level.

For example, one ML program might be designed to determine when an industrial machine has broken down based on readings from its various sensors, so that it can alert the operator. Another ML program might take raw audio data from a microphone and determine if a word has been spoken, so it can activate a smart home device.

Unlike normal computer programs, the rules of ML programs are not determined by a developer. Instead, ML uses specialized algorithms **to learn** rules from data, in a process known as **training**.

In a traditional piece of software, an engineer designs an algorithm that takes an input, applies various rules, and returns an output. The algorithm’s internal operations are planned out by the engineer and implemented explicitly through lines of code. To predict breakdowns in an industrial machine, the engineer would need to understand which measurements in the data indicate a problem and write code that deliberately checks for them.

This approach works fine for many problems. For example, we know that water boils at 100°C at sea level, so it’s easy to write a program that can predict whether water is boiling based on its current temperature and altitude. But in many cases, it can be difficult to know the exact combination of factors that predicts a given state. To continue with our industrial machine example, there might be various different combinations of production rate, temperature, and vibration level that might indicate a problem but are not immediately obvious from looking at the data.

To create an ML program, an engineer first collects a substantial set of training data. They then feed this data into a special kind of algorithm, and let the algorithm discover the rules. This means that as ML engineers, we can create programs that make predictions based on complex data without having to understand all of the complexity ourselves.

Through the training process, the ML algorithm builds a **model** of the system based on the data we provide. We run data through this model to make predictions, in a process called **inference.**

### Where can machine learning help?

Machine learning is an excellent tool for solving problems that involve pattern recognition, especially patterns that are complex and might be difficult for a human observer to identify. ML algorithms excel at turning messy, high-bandwidth raw data into usable signals, especially combined with conventional signal processing.

For example, the average person might struggle to recognize the signs of a machine failure given ten different streams of dense, noisy sensor data. However, a machine learning algorithm can often learn to spot the difference.

But ML is not always the best tool for the job. If the rules of a system are well defined and can be easily expressed with hard-coded logic, it’s usually more efficient to work that way.


⚠️ **Limitations of machine learning**

*Machine learning algorithms are powerful tools, but they can have the following drawbacks:*

* They output estimates and approximations, not exact answers
* ML models can be computationally expensive to run
* Training data can be time consuming and expensive to obtain
* It can be tempting to try and apply ML everywhere—but if you can solve a problem without ML, it is usually better to do so.

### What is embedded ML?

Recent advances in microprocessor architecture and algorithm design have made it possible to run sophisticated machine learning workloads on even the smallest of microcontrollers. Embedded machine learning, also known as TinyML, is the field of machine learning when applied to embedded systems such as these.

There are some major advantages to deploying ML on embedded devices. The key advantages are neatly expressed in the unfortunate acronym BLERP, coined by Jeff Bier. They are:

**Bandwidth** — ML algorithms on edge devices can extract meaningful information from data that would otherwise be inaccessible due to bandwidth constraints.

**Latency** — On-device ML models can respond in real-time to inputs, enabling applications such as autonomous vehicles, which would not be viable if dependent on network latency.

**Economics** — By processing data on-device, embedded ML systems avoid the costs of transmitting data over a network and processing it in the cloud.

**Reliability** — Systems controlled by on-device models are inherently more reliable than those which depend on a connection to the cloud.

**Privacy** — When data is processed on an embedded system and is never transmitted to the cloud, user privacy is protected and there is less chance of abuse.


## Edge Impulse Overview

🎙️ Interview of [Aurelien Lequertier](https://www.linkedin.com/in/alequertier/), Lead User Success Engineer at Edge Impulse:

### Create an account & a project

Start by creating an account on [Edge Impusle](https://studio.edgeimpulse.com/signup):

![signup](assets/signup.png)

Validate your email and go to the dashboard, on the upper right corner, click on `Create a new project`:

![dashboard](assets/dashboard.png)

Name your first project `Free Fall Detection`:

![create-project](assets/create-project.png)

## Free Fall Detection

### Data acquisition with your mobile phone

The first step will be to acquire data, **the dataset**, for this we will use your smartphone sensors and for this particular use case, the accelerometer. 

Your dataset will be split into the training set and the test set. We will use a rough **80/20** ratio between the **training** set and the **testing** set:

![dataset](assets/dataset.png)

*Image source: [https://medium.com/@lhessani.sa/what-is-the-difference-between-training-and-test-dataset-91308080a4e8](https://medium.com/@lhessani.sa/what-is-the-difference-between-training-and-test-dataset-91308080a4e8)*

Sets explanation:

* Training Set: Here, you have the complete training dataset. You can extract features and train to fit a model and so on.

* Validation Set: This is crucial to choose the right parameters for your estimator. We can divide the training set into a train set and validation set. Based on the validation test results, the model can be trained(for instance, changing parameters, classifiers). This will help us get the most optimized model.

* Testing Set: Here, once the model is obtained, you can predict using the model obtained on the training set.

Back to the dashboard, click on `Let's collect some data`:

![start-collection-data](assets/start-collecting-data.png)

and `Show QR Code`

![use-your-mobile-phone](assets/use-your-mobile-phone.png)

![qr-code](assets/qr-code.png)

Flash the code with your favorite QR Scanner app, your phone should then be connected:

![phone-connected](assets/phone-connected.png)

And on your phone:

![device-connected-2](assets/device-connected-2.jpg)

Click on `Get Started!` you should see `1 Device(s) connected` on the right tab:

![device-connected](assets/device-connected.png)

Jump the the `Data acquisition` tab:

You will see on the upper left corner two tabs: `Training data` and `Test data`:

![training-vs-test](assets/training-vs-test.png)

Make sure you start with the Training data` tab.

We will now acquire three types of data:

* Unknown movements pattern (around 50) -> `unknown` label
* Free fall movements pattern (around 15) -> `freefall` label
* No movements pattern (around 15) -> `idle` label

To differentiate our movements, we will use the labels:

![label-freefall](assets/label-freefall.png)

We will set `3000 ms` for the `Sample length (ms.)` and obviously the `Accelerometer` for the `Sensor` field.

When you will start sampling your data, just click on `Start sampling` on the following screen should appear on your phone:

![phone-start-sampling](assets/phone-start-sampling.jpg)

Here are the three types of data overview:

![labeled-data-1](assets/labeled-data-1.png)

![labeled-data-2](assets/labeled-data-2.png)

![labeled-data-3](assets/labeled-data-3.png)

Time to work! 

* Get 15 `freefall` data, 15 `idle` data and 50 `unknown` Training data 
* Get 4 `freefall` data, 4 `idle` data and 10 `unknown` Test data 

⚠️ Warning: For the free-fall data acquistion, do it above your bed or your sofa, I will not be hold responible if you break your phone! ;) 

![training-data](assets/training-data.png)

![test-data](assets/test-data.png)

Great, we have our dataset to start training our model!

### Training our model

We won't enter too much into details here as it could get too complicated.

However, Edge Impulse have this incredible feature which pre-processes your data to generate features. Then, theses features will be passed to your Neural Network.

Navigate to the `Impulse design` menu tab and create an impulse:

![create-impulse-1](assets/create-impulse-1.png)

Replace the default parameters with Window increase: `215 ms`, then add a `Spectral Analysis` processing block:

![create-impulse-2](assets/create-impulse-2.png)

Add `Neural Network` learning block:

![create-impulse-3](assets/create-impulse-3.png)

You should have the following results:

![create-impulse-4](assets/create-impulse-4.png)











## Image classification


## Trigger word detection
