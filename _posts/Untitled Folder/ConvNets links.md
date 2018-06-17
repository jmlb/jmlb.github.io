ConvNets links:
http://brohrer.github.io/how_convolutional_neural_networks_work.html (posted by @abtin)




DL framework such as Torch

TensorFlow

ROS bag?

What is the type of ECU installed on the trunk ? The white box from #hardware channel
```
A NexCom NISE 3700E

what is the i7 GTX1070 machine used for?
Right now its the main ROS driver. Soon it will move to be the DVR

@tommy.lewis:  I think what they are trying to do when the prediction for the steering captured as strong then it will send to the ROS nodes to control the DBW or SBW system. Therefore it's required to have two separate system running in parallel as one for controller and second for running DL or ML network
drive by wire steer by wire



 RNNs: comma.ai has some interesting examples of plugging CNN's + RNN's