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
  
3. Run this command to update your installer: ```sudo apt-get update```

4. Enter your password to continue.

5. Run this command to install cmake: ```sudo apt-get install git cmake```

6. Clone the jetson-inference project: ```git clone --recursive https://github.com/dusty-nv/jetson-inference```

7. Change into the newly created jetson-inference folder: ```cd jetson-inference```

8. Update the contents of the folder: ```git submodule update --init```

9. Install the python processes necessary to run the AI: ```sudo apt-get install libpython3-dev python3-numpy```

10. Switch to the jetson-inference directory and make a build directory to build your project into: ```mkdir build```

11. Switch to the build directory: ```cd build```

12. Build the project with this command: ```cmake ../```

13. Switch to the build directory if not already in it. Run this command to run make the python files: ```make```

14. Run this command to install make. ```sudo make install```

15. Configure the make command: ```sudo ldconfig.```

16. Download the skin disease dataset at https://www.kaggle.com/datasets/stoveeater/skin-fungus-dataset

17. Extract the elements of the zip file and drag the extracted folder into jetson-inference > python > training > classification > data

18. cd back into jetson-inference.

19. Run this command to allot more memory for the program: ```echo 1 | sudo tee /proc/sys/vm/overcommit_memory```

20. Run the docker with this command: ```./docker/run.sh```

21. Enter your password when prompted.

22. cd into jetson-inference/python/training/classification.

23. Run this code to start training the AI based on the dataset: ```python3 train.py --model-dir=models/skin-disease-dataset data/skin-disease-dataset --epochs=35```

24. To increase or decrease the amount of training the AI model receives, increase or decrease the number in ```--epochs=35``` respectively.

25. Once the model has finished training, cd into jetson-inference/python/training/classification.

26. Export the model into onnx: ```python3 onnx_export.py --model-dir=models/skin-disease-dataset```

27. Use ctrl+D to exit the docker.

28. cd into jetson-inference/python/training/classification.

29. Run these commands to set up variables needed for image processing:

```
NET=models/skin-disease-dataset
```
```
DATASET=data/skin-disease-dataset
```

30. Run this command try an image from the test folder. Change 'NAME HERE' to name your output file, rename 'NAME OF CATEGORY' to the category of what you want to test, rename 'IMAGE NAME' to the name of the image. You can rename the image first in the side menu to customize the name.
```imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/NAME OF CATEGORY/IMAGE NAME .jpg $DATASET/output/OUTPUT NAME.jpg```

Here is an example of what your command should look like:
```imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/ringworm/ringworm_0.jpg ringworm_test_0.jpg```

The results should automatically go into the classification folder, and double click a picture to view the AI's classification and confidence.

[View a video explanation here](video link)
