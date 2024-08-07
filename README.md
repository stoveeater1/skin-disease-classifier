# skin-disease-classifier
Quick, DIY diagnosis of certain skin diseases such as athlete's foot, nail fungus, and ringworm.

![image](https://github.com/user-attachments/assets/f31110d6-b2e2-47f4-b70f-de1b8adff67a)

## The Algorithm

For this project, I used the Jetson NANO to run a resnet18 AI model to classify these diseases. Resnet18 is a convolutional neural network that works 18 layers deep, which means it considers 18 different factors when coming up with a conclusion. 18 factors may seem like a lot at first, but for an AI to class so many different types of skin diseases, it would need much more than 18 layers in order to be as accurate as possible. I set my AI to only class 3 different types of diseases so that its accuracy will be as high as possible for the resources available to me. 

This AI is meant to classify a skin disease based off of a sample image of the affected area. For example, in the image included above, the AI accurately determined that the person is affected by ringworm. It can only classify athlete's foot, nail fungus, and ringworm at the moment. 

## Problem and Impact

I decided to work on this project because of my own personal experience with skin problems. I have had many skin problems in the past due to different factors such as allergies and an uncomfortable environment. Last year, I had an extremely bad case with my skin rashing when it made contact with another surface in the form of scratching, itching, or even touching certain materials or allergens. It went on for about 5 months, from November through April. I never knew what was up with my skin through this time period, and I had booked an appointment with a doctor in December. My actual visit to the doctor was almost four months later, in early April, due to delays, time conflicts, and a slow Canadian healthcare system. Throughout this time, I was unsure of what was wrong with my skin and did not know what I could do about it. After all these months, I was told that my skin was extremely sensitive to allergens and to take allergy pills daily until my condition improved.

The fact that just a single, simple diagnosis could take up to 4 months to make is absolutely absurd and should not be happening in any country. This AI would help with this problem because it can make a quick diagnosis and give a clue as to what is happening and whether or not you need to book an appointment. This is a much faster diagnosis than the 4 month diagnosis I received, and could potentially save people with skin conditions a lot of unnecessary suffering. 


## Running this project

1. Set up an SSH conection with your Jetson Nano.
2. Open a new terminal.
3. Run this command to update your installer: sudo apt-get update. Here, you will enter your password.
sudo apt-get update

Run this command to install cmake
sudo apt-get install git cmake

Use cd commands to change directories until you are in your jetson-inference/python/training/classification/data
cd jetson-inference/python/training/classification/data

Run this command to download the image dataset.
git clone --recursive https://github.com/isamarquezg/skincancerdata

cd back to 'classification' directory, and then cd into the 'models' directory
cd .. cd models

Run this command to download the skin cancer classification model.
git clone --recursive  https://github.com/isamarquezg/skincancer

cd back to 'classification' directory
cd ..

Use the following command to make sure that the model is on the nano. You should see a file called resnet18.onnx.
ls models/skincancer/

Set the NET and DATASET variables by running each of these commands separately
NET=models/skincancer

DATASET=data/skincancerdata

Run this command try the model and see how it operates on an image from the test folder!! Change 'NAME HERE' to name your output file and rename 'NAME OF CATEGORY' and 'IMAGE NAME'
imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/NAME OF CATEGORY/IMAGE NAME .jpg $DATASET/output/NAME OUTPUT.jpg

This is an example of what your command should look like after you replace the fill-ins.

imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/vasc/vasc7.jpg $DATASET/output/test1.jpg

Look at your results by opening the image that just saved in the 'output' folder! This folder should be located in jetson-inference/python/training/classification/data/skincancerdata/output

[View a video explanation here](video link)
