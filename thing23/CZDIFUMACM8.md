
# Federated Learning on FVM - TechieTeee

<https://youtube.com/watch?v=CZDIFUMACM8>

![image for Federated Learning on FVM - TechieTeee](/thing23/CZDIFUMACM8.jpg)

## Content

So today we're going to learn about federated machine learning on FEVM.

So I'll share a little bit about me, talk about the impact of machine learning and how
it's growing every day tremendously, and how we combine federated machine learning and

blockchain. So here's a little bit about me. I'm a data engineer and blockchain developer. I'm crazy about data. I have a very eclectic engineering experience, from academic research to consulting, entrepreneurship,

working with CPG brands. And to me, the biggest part of my development in Web3 has definitely been participating in hackathons and DAOs, including being involved with D-ORG, which is a DAO that provides services

to different startups and businesses in Web3 for software engineering and design, all that

good stuff. And you can find me on social and all that good stuff under Techie T. So why is this so important? So according to McKinsey's AI report, by 2030, the global economic impact of AI, machine

learning, and automation is going to be more than $13 trillion. That's a lot of money, right? But we still have a lot of unanswered questions about how this will impact a lot of different industries. Think about all the models that have become really popular over just the past few months
and have become just like everyday language and everyday experiences. From stable to fusion, chat GPT, mid-journey, BARD, Bing. And these models are not just going to be isolated to single industries. They're going to have a tremendous impact on things like public policy, safety management,
decision making, law, medicine. So how can we think about pushing this boundary even further and integrating this into blockchain?

So this is why I'm really excited about federated machine learning. So with the average machine learning model that you have right now, the model and the data have to work pretty close together. So you have to share data from your devices to these models and then use that to train the model and then develop the model, right? But with federated machine learning, it takes a completely different approach. It says instead of us sending, you can see here, sad clown, right? So the data is being sent away from the devices to the model. And we can actually change it to where the model is being sent to the device instead of the data being sent. This allows for more data privacy for the user. This allows for us to expand new use cases in different industries where data privacy is really important. Think about law and medicine where you have a lot of different regulatory things and laws that prohibit how data can be shared. But we're missing out on a lot of those machine learning insights because of the regulatory things around that. FNL will allow us to explore what those use cases look like in these highly regulated
spaces while still protecting the privacy of the user. It also makes you think about everyday items like your Google Assistant or Siri or your
different IoT devices that are at home. You don't want your data that you have made at home and different things you've asked,
you know, Alexa and Siri to be shared with all the other users in that network, right?
But you might want to still benefit from the machine learning insights on your individual data. So let's actually build one during this little workshop and deploy it to FEVM.

So if you really want to follow along, you can scan this link right here, and then that will give you all the materials. It will give you the Google Colab link and also give you the repo for the model in the

smart contract. So I like interaction. I like feedback. So if you're doing good, say, I'm doing good. So every time I ask, give me a thumbs up. Let me know you're breathing your last on me, okay?
So if everybody who wants to follow along has it, give me a thumbs up. Are you good? You're good? Okay. So I'm going to switch to the Google Colab. So the reason why I use Google Colab is because we can run this completely online without having to deal with the local environment. So as long as you go to the link right here, we're going to go through each step, and I'll explain what's happening in each part, okay? So first we got to install our packages, right? So just press the little play button right there. It's going to do all the installation of the packages that we need. The main package that we'll be using is something called Flowers. Flowers is a very user-friendly federated machine learning package in Python.
And to me, it's one of the best libraries I found from the ones I tested out, because

you don't have to do a lot of configuration before running it, and you can run it in a nice environment at Google Colab. You can also run it in your local environment, too, if that's what you choose. So this takes a few seconds for it to go. It's loading, it's thinking, it's doing all that good stuff in the background. Then after that, we're going to do all the imports that we need. And I'll also tell you about the dataset that we're using. The dataset that we're using is basically used for object detection.

It's used in a lot of visual detection algorithms.
It's about over 66,000 images in 10 different classes. It's used pretty widely. So now we've done all the imports and the installs, and now we're going to load the data right here. CIFAR10 is the dataset we're using. So if you want to learn more about the dataset, just go here and check it out. These are the classes that we'll be using. Common everyday objects. You know, plane, car, bird, cat. Normal stuff, right? So we define our classes right here. Now we're going to determine the number of clients. So for this demonstration, I'm only defining 16. It's a pretty small number, right? But if we get into large numbers, like 100 or so, I tried out different numbers of classes and batches. Oh. Let's see.
Is that better? Okay. So when you go beyond, like, around 20 or so, Google Colab starts to freak out.
So keep it small if you're going to be running it in an online environment. But for this one, I defined 16 clients. This would be like the equivalent of having 16 cell phones. So for our little experiment, we're going to imagine we have 16 cell phones, and we want to be able to detect the different objects that are in the images of the cell phone without actually sharing the personal data of each cell phone, right? So after that, we want to define our batch size. We want to load and split the data. This is just like your regular approach you do for regular machine learning models so far. So we're batch size of 50. We're loading the data set. We're splitting it up. So when we're doing the splitting and training, that means we're going to use some of our data for training the model, and then we're going to use some of our data for testing the model. So your usual approach of how you're doing machine learning stuff. It's finished, and now we need to configure our labels.
So we're going to use the classes that we defined earlier, like the cat, dog, plane, all that good stuff, and we're going to use that for the labels to actually figure out, okay, well, what is each thing in this image? And it's going to spit out some examples for us. So you see here, we got some images, and we see how well was it able to determine what
each thing was in the image. We got a little doggy here, a little plane, a little ship. So our model's working pretty well, like it's supposed to, right? But now we need to develop this with a convolutional neural network, which is like a pretty robust
image identification algorithm, defining our classes.
Again, we just press the play button here. Let me know if I'm going too quickly, or you want me to stop and explain something further.

And we need to find our training and test functions. So this will determine basically how many times that the algorithm is going to go over the data and keep going over it to train itself in what it's looking at.

And you can change a lot of things here for the flower library. You can change the epic, and you can change how much entropy loss that you want to have.
It's a lot of things that you can personalize based off what you want for your model and for your dataset. And then also, what's the acceptable accuracy and loss? All that good stuff. So then here, we're going to train the model.

This one takes a little bit longer. So you can see here, each epic is going through and trying to improve its accuracy of identifying
the objects, and it's outputting each time. So the accuracy right now isn't very great, because we would have to train it for a longer time. But if you did give it more rounds to train itself, then the accuracy would continue to improve. But for the first time around, it's pretty good. We got a little bit under 40% accuracy. And so now we set up the federated learning with the flower, the flower package.

So we find our parameters. We define the dictionary, the load, all that good stuff.
The most important part right here is where we implement the flower client. And so this is where it takes the data that we all provided, and then it divides it up.
It partitions it into smaller sets, because this is simulated to be different devices.
If you had a real dataset coming from different cell phone devices, it would already be split up. But to simulate that, we were just partitioning it to make it seem like it's coming from 16 separate devices. Then we turn the client, and then we start the training for the flower client.

You can see the output here.

And it's telling you which step of the process it's on. So how many clients have been sampled out of 16. So this is another thing about FML. So it's randomly deciding which of the 16 it's going to use to work with the model on.
This potentially could be a downside of FML in its early stages, because what if you have
one cell phone that has a lot more data on it than another cell phone, right? And so that could sway the development of the model. But to me, I think some of those things can be controlled at the smart contract level. Like you can say, we only want to accept this amount of data from this particular device, or we can even say that we only want to accept data of this quality before it can be used for the model. I think the benefits of FML way outnumber the downsides of it.

So this one takes a little bit. It's going. Sometimes it can take like two or three minutes.

But you can see the progress that's happening in real time.

Oh, also, one thing that will make it run faster too is when you go to your runtime

here, you can go to... I'm not going to change it right now, but I'll show you how to change it. You can change your runtime right here. So by default, when you go to Google Colab, it's set at none.
But if you set it to GPU, that will make it run a lot faster. And it's freaking free, which is great.

So it's going on to its next round. It's sampled five clients out of 16 so far. Okay. Now it's getting close to the end of the sampling.

It's going. It's going.

After this step is completed right here, then we'll use a virtual client and then we can get the accuracies and examples. This is the second to last step right there. Then after that step is done, we'll get the dictionaries and then we'll export the model

and we can use that in the smart contract. Okay. It's going. It's going. It takes a minute. So we're on round four.

Sometimes also if you're on the free tier for Google Colab, if there are a lot of people

using it at that time, it's a little bit slower. Usually it gets done in two minutes. Now it's on three. But it should be done pretty shortly. Then we can move on to the next step.

It also gives you the failure rates of each round of training. So it's all showing you in real time how well the model is training itself.

It's going.

Okay.

Close to finishing. We can continue on with the next step and then it will catch up with us.

So after that step is completed, then you'll have to use the virtual client engine in Flower.

And this is going to be a further evaluation of the accuracy and the examples provided.
So we'll go ahead and process that. And so, you know, we'll just put it in the queue to be processed.
Then we want to create a strategy to call the function whenever receives an evaluation metric. So you can define each of these things based off what you want for your data, based off what you want for the accuracy, based off what you want for the fit. So you can see here, fraction fit, fraction evaluation, the number of clients you want

to be used for the evaluation, the minimum number of clients you want to be used. So that means basically, like, unless there's at least that number of clients there, separate devices, then you if that's not met, then it wouldn't run.
Okay. Let's see. Both of those finished. So now we create the strategy. This step goes pretty quickly. So now I'm creating the simulation with the Flowers package.

With all the initial parameters that were defined.

So right after that, this step is done right here. Then we'll save the model and export it. So all these steps that we've done right now are pretty combination of traditional machine learning methods you see, irregular training, splitting the data, checking for accuracy.

All that's your regular machine learning approach. The part that's different is the partitioning, right? So we were able to partition the data and make it be simulated to look like it's coming

from separate devices, right? And so also, like, how the model is shared with the data, right?
So instead of the data being sent to the model, the model is coming to the data.
So that's very different than that's a very different part from federal machine learning
compared to regular machine learning. So this is the last step for the training evaluation.
After this, we just export the model. And if you are on the GitHub page, I already exported the model and put it in the folder.
So it's in the root folder, so you don't have to go through all the steps here to get the model to follow through the next part.

Okay.

It's gone.

So after that part is finished, I'm just gonna put this in the queue for it to run this next. So unless you have, like, you know, an infrastructure already set up where you're doing, like, CICD

for your model or something, you need to export your model and save it somewhere. Because otherwise, you're gonna lose all the freaking progress you just made on training your model. So we're gonna save it. We save it to this model path right here. Then we load the model path. And we need to put it into a format that can actually be usable for the smart contract. So there's the pretty robust, like, format called ONIX that's used in machine learning.

So once we basically export it as an ONIX file, this will allow us to basically put
the model, define the model in terms of bytes so that we can call on the model within the smart contract. And with our model service.
So we just do that. Doing the same thing. Importing and installs what we need to create the ONIX file. And then we need to convert it to binary code.
And then we're gonna read that model from the file. And I'm saving it as a text file. And then that ends up being used. But I already have the outputs from when I ran this earlier. Basically this is what it ends up on being. So this is a gigantic file with a byte code in it that represents the model.

And then we can use this to actually make a model service with a smart contract.

Everybody following? So I'm gonna review what just happened. So first we made our model, right? We were able to create the model using our open source data. And we were able to simulate it coming from 16 separate devices. And we were able to make the FML package using flowers package. But the model needs to be put into a format that can be understood by smart contracts. So that's why we had to export it as an ONIX file and put it into byte code so we can use
it in our smart contract. So here's the next part. So now we're going to put this on chain.

So I think everybody in the room probably knows, because we're all really into data and all that good stuff. About Protocol Labs, you know, recently released the main net for FEVM. And I'm really excited about it. I just randomly thought about this one day. Oh, okay. What about machine learning on chain? So we can create a model service with this smart contract right here.
So I just forked the basic FEVM hard hat kit.
And I just changed the simple coin contract to a contract that could be used to deploy the model and also could be used to send predictions to it. All right. I forgot to show you one thing. So when you're in Google Colab, you have to export your model. And so if you go to the side here, this is where your files will be. And then you can click on any of the folders. And you can download the folder from there.

ľAnd so if we had ran it all the way to here, then it would show the file. You export that, and then you just put that in your local hard hat project. You can also run your hard hat project on Gitpod. I use IntelliJ, because that's my favorite IDE, but it really doesn't matter. As long as you can run hard hat in it, you can use anything you want. But if you fork the GitHub project, it will have everything you need in it. Here's the smart contract right here. It's a simple smart contract. So the one thing that you can do is you can

deploy the model and then choose to send predictions to it in terms of bytes. And then
you can see here, right here, this is where I put the output for the model. So everything you need

is already in the GitHub repo. You just clone it, and then everything is regular. And then you just

deploy it just like regular.ľ Oh, and I'm sure you know, you need your.env with your private key in it.

So you can define your private key from the command line, or you can just go to a new file
and then put your private key in there. And please, please, please make sure to add that to Gitignore. You don't want your private key on GitHub. There's a little bit of an error happening. One second. Let's see. So I made one update to it. Now it's acting crazy. I have another version

of the file before the change. There we go. Okay, so it's deploying it. So I didn't change any of

the other default contracts that are in there, like the storage deal, all that good stuff. So it's deploying it right now. It's a little bit slow. So first, we'll deploy the model service, and then we'll deploy the other default contracts that are in there. But voila, now you have an FML model on chain where you can make predictions as long as you make sure it's in the right format.

And to me, what's the whole freaking purpose of this? Why do we need to do this on chain? I just literally had this conversation with somebody two days ago when we were walking from dinner. There's a lot of benefits to having these machine learning models on chain. So to me,
like I see more and more everyday decisions being influenced by machine learning, like public policy,

budgets for city council, really important issues for like healthcare, right? All these things.

A lot of these decisions are made pretty arbitrarily, right? But what if there was accountability for these organizations to actually be able to show like, hey, these are the actual decisions we're making. Here's a record of these things on chain. Here's the accountability of how these models work to make sure there's no bias in these models, make sure that the data quality is
there. Now, will most people actually go and look at these things? If you could go to your local city council website and see it? No, but it's there, right? So maybe it will take a few like really

passionate citizens to be involved and then, you know, translates to the rest of the world who may
not be technical to say like, hey, this is what's happening. And, you know, we have like a good
infrastructure, good ethics in place for how we're developing these models. So, yeah. So, yeah. And

I'll just go here really quickly. So, I'm gonna finish. Yeah. So, yeah. Again, I'm part of D-ORG.

We're building the decentralized web. We're working on a lot of cool stuff like this. And I don't know if we have any time for questions. But, yeah. I hope this piqued your
curiosity a little bit for federated machine learning and that you deploy your own model. Definitely check out the flowers package. It's very like user-friendly. You can literally have your own FML model on chain for like under an hour. So, yeah. Thank you.

Thank you. That was great. You kind of lost me at the ONNX to MPC

part there. Like you just said, it's convert to bytecode. Can you kind of go through that and you
explain exactly what that bytecode is? Yeah. So, if you just put in like the raw model,
like how default comes out in Python, like it's not understandable for the smart contract. So,

I tried a lot of different formats. And it wasn't until I tried ONNX, the ONNX format,
that I was actually able to get the smart contract to compile and actually deploy to chain. But ONNX is like pretty, I would say pretty commonplace. Like it's like basically for interoperability. So, you can switch between platforms for your machine learning models and then do a lot of things like outside of even blockchain space. Like it's a pretty like sturdy

format you can use. Okay. So, tell me if I'm wrong. I'm trying to recap what you said. Okay. So,

I cannot understand what ONNX is, right? So, basically you're taking the model in the ONNX format and then compiling it into a smart contract bytecode. And that's why you had like that

assembly kind of like block. And you're just running that in a smart contract. Yeah. Right.
So, isn't that like slow if the model is big? It's going to be kind of like awful, right? Yeah.

So, like with the example, like even for that library, it's like over 66,000 images. And images

already are pretty like hefty in terms of memory. I was able still to compile within like a few minutes. I think that there probably has to be considerations in the future about even larger
files, right? So, there needs to be kind of like this interface of like what will be done off chain
and what can be done on chain. Like as you saw, like all of the development of the model itself was off chain. But actually like putting the model itself, like the output of that, of the training
on chain doesn't take up that much time to compile. That's why I think all the talks today

were interesting about like Bacalao and different ways of thinking about compute over compute on

data. I don't know. I mean, what would it be like if we had video? I think video would be very
interesting because that would be very memory intensive. Like I want to test it out. I want to see like these are new areas of combining technologies together. So, we just have to keep
trying it out and experimenting and see what happens. Thank you.

