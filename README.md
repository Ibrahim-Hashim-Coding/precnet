# PreCNet

This repository contains complete code and trained models related to the article Straka, Z.; Svoboda, T. & Hoffmann, M. (2023), [PreCNet: Next-frame video prediction based on predictive coding](https://doi.org/10.1109/TNNLS.2023.3240857), IEEE Transactions on Neural Networks and Learning Systems. [Arxiv](https://arxiv.org/abs/2004.14878). The content is sufficient to generate main results and figures from the paper.

PreCNet is a deep hierachical reccurent network for next frame video prediction which embodies predictive coding schema proposed by Rao and Ballard ([Predictive coding in the visual cortex: a functional interpretation of some extra-classical receptive-field effects](https://www.nature.com/articles/nn0199_79)).

## How to proceed
0. Install prerequisities
1. Clone this repository
2. Get datasets
3. Train or download a network
4. Get desired evaluation or figures 

## Prerequisities
Versions used during training/testing are shown in the parenthesis.
* Python 3 (3.6.6)
* Keras (2.2.4)
* Tensorflow (1.13.1)
* Hickle (3.4.5)
* Numpy (1.15.0)
* Matplotlib (3.1.2)
* Pillow (6.2.1)
* Six (1.11.0)  

## Datasets
The model was trained on (i) **KITTI** dataset, (ii) large subset of **Berkeley DeepDrive dataset (BDD100K)** with size 2M frames (*bdd_large*), (iii) small subset of **BDD100K** with size 41K frames (*bdd_small*). Evaluation of the network was performed on test part of **Caltech Pedestrian Dataset**.

Dataset location is set in *{kitti/bdd_large/bdd_small}_settings.py*.


### Getting datasets
Please see the links bellow for information about the datasets and their terms of use.
#### KITTI Dataset (http://www.cvlibs.net/datasets/kitti/)
Run ```python3 process_kitti.py```.

#### Caltech Pedestrian Dataset (http://www.vision.caltech.edu/Image_Datasets/CaltechPedestrians/)
Perform: 
1) Execute ```./download_caltech_pedestrian_dataset.sh```.
2) Download and install [Piotr's Computer Vision Matlab Toolbox](https://pdollar.github.io/toolbox/).
3) Run ```cal_ped_seq2imgs.m``` in Matlab.
4) Run ```python3 process_cal_ped_test.py```.  

#### BDD100K Dataset (https://bdd-data.berkeley.edu/) 
As the dataset is very large, only (randomly) selected subsets were used for creating train and validation datasets. Therefore, it is necesarry to use sources files to get the exactly same datasets as were used during training.  
Perform:
1) Execute ```./download_bdd100k_selected.sh```.
2) Run ```python3 process_selected_bdd100k_val.py``` for getting validation dataset. 
2) Run ```python3 process_selected_bdd100k_train0-4999.py``` (```python3 process_selected_bdd100k_train_40K.py```) for getting large (small) subset of *BDD100K* as a training set -- 2M (41K) of frames.


## Training of a network
The model can be train, depending on training dataset, by ```python3 kitti_train.py```, ```python3 bdd_large_train.py``` or ```python3 bdd_small_train.py```. 

Already trained models, which were evaluated in the article, can be found in the folders *model_data_{kitti/bdd_small/bdd_large}*. **These models will be overwritten by newly trained models if you run the training.** You can prevent it, for instance, by changing their names.  

Model location is set in *{kitti/bdd_large/bdd_small}_settings.py*.  


## Evaluation and figures
See comments in the code for choosing a model (trained on *kitti/bdd_large/bdd_small*). Results will be saved in the folder *{kitti/bdd_large/bdd_small}_results* (defined in *{kitti/bdd_large/bdd_small}_settings.py*).

### Next frame video prediction evaluation

Run ```python3 caltech_pedestrian_evaluate.py``` for getting SSIM, PSNR, MSE values on *Caltech Pedestrian Dataset* (Tables 3, 4 in the article) and getting randomly selected predicted sequences. 

Execute ```python3 caltech_pedest_plot_selected_seq.py``` for obtaining a selected sequence prediction (Fig. 5, 6 in the article). 
 

### Multiple frame video prediction evaluation
Run ```python3 caltech_pedest_evaluate_extrap.py``` for getting SSIM, PSNR, MSE values for multiple frame prediction on *Caltech Pedestrian Dataset* (Table 5 in the article) and obtaining randomly selected predicted sequences.

Execute ```python3 caltech_pedest_plot_selected_seq_extrap_fig.py``` for obtaining a selected sequence with multiple frame prediction.



## Additional Notes
A size of input images has to be divisible by 2^(nb of layers - 1) because pooling operation halves size of its input in each layer and the sizes have to be integers in all layers.

Network states can be obtained by setting output mode to desire units and layer (e.g. ```output_mode = 'Etd1'``` for getting error units states in second layer after top down pass).


## Acknowledgements
We would like to thank to the authors of [PredNet](https://arxiv.org/abs/1605.08104) for making their [source code](https://github.com/coxlab/prednet) public which significantly accelerated the development of PreCNet.







