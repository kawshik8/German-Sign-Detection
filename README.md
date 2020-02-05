# German-Traffic-Sign-Detection

Link to Dataset: http://benchmark.ini.rub.de/

Final Accuracy: 99.45%
Framework: Pytorch

References:
1) J. Stallkamp, M. Schlipsing, J. Salmen, C. Igel,
   Man vs. computer: Benchmarking machine learning algorithms for traffic sign recognition, Neural Networks, Volume 32, 2012, Pages 323-332, ISSN 0893-6080,
   https://doi.org/10.1016/j.neunet.2012.02.016. (http://www.sciencedirect.com/science/article/pii/S0893608012000457)
2) P. Sermanet and Y. LeCun, "Traffic sign recognition with multi-scale Convolutional Networks," The 2011 International Joint Conference on Neural Networks, San Jose, CA, 2011, pp. 2809-2813.
   doi: 10.1109/IJCNN.2011.6033589
   keywords: {computer vision;image classification;image colour analysis;traffic engineering computing;traffic sign recognition;multiscale convolutional network;traffic sign classification;GTSRB competition;multistage architecture;hierarchy learning;vision approach;hand-crafted features;HOG;SIFT;greyscale images;Image color analysis;Accuracy;Training;Color;Feature extraction;Neural networks;Computer architecture},
   URL: http://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=6033589&isnumber=6033131
3) \bibitem[Lin et al.(2017)]{2017arXiv170802002L} Lin, T.-Y., Goyal, P., Girshick, R., et al.\ 2017, arXiv e-prints, arXiv:1708.02002

Notes:


1) Trained a basic model consisting of 3 convolutional layers all with 3x3 filters and two dense layers including the output. 
2) Referenced and experimented with 7x7 and 5x5 filters which worked better. 7x7 for the first layer , 5x5 for the second and 3x3 for the third convolutional layer respectively work best.
3) Included batch normalization after each layer. Resulted in better val accuracy
4) Flatten vs Global Average Pooling. Gave better val accuracy with flattening the final vector
5) Tried to use Focal loss with various gamma values to handle classifying hard classes (in this case classes having low number of images). Gamma = 1 worked best.
6) Experimented with the number of neurons in the last before dense layer. 256 neurons worked best.
7) Dropouts after conv layers did not work well (does not make sense but many papers use this). However dropouts after dense layers seem to increase the accuracy.
8) Tried to train on the given dataset and then finetune on a manually created balanced set containing equal number of images (Least number of images in all classes will be the number of images per class). It did not improve accuracy.
9) Added a learning rate scheduler which would reduce the learning rate when validation accuracy flattens out.
10) Saved the models only if val accuracy or loss increases or decreases respectively.
11) Experimented with various input sizes (48 and 32) 48 gave the best accuracy.
12) Created an augmented dataset consisting of 1200 images per class using randomly applying rotation, shearing, translation or scaling to the image. Used a balanced random sampler to achieve this using class weights.
12a) Used Contrast limited adaptive histogram equalization for reducing illumination variation over the images.
12b) Trained on the augmented dataset using the same network and achieved 99.7% accuracy on the val set. Final result was 99.45% accuracy on the test set 
