# CSE6250 - Group C4
CSE 6250 BD4H Final Project

Group Members: 
Eng Meng Tan (etan63@gatech.edu)
Min Cai (mcai61@gatech.edu)

This project is for fall 2024. The paper replicated is "Variationally Regularized Graph-based Representation Learning for Electronic Health Records" and the accompanying repo can be found under: https://github.com/NYUMedML/GNN_for_EHR

## Data and Preprocessing
The data used for this project is the MIMIC-III EHR dataset and approval needs to be first granted before accessing the data.
Access to the data can be found from the physionet database: https://physionet.org/about/database/

You will need to register and get approval to access the dataset. 
After getting access the dataset, the next thing is to download the relevant information. For the MIMIC-III dataset, we are only using 4 csv/excels, namely:
1. Admissions.csv
2. Diagnoses_ICD.csv
3. Procedures_ICD.csv
4. Labevents.csv

The MIMIC-III dataset is very large and time can be saved by downloading only the relevant information.

An example of the file path setup is given below:

Once you have downloaded the information, put the data under Preprocessing/mimic/input_data
The file structure should look like this:
```
.
└── Preprocessing/
    └── mimic/
        ├── preprocess_mimic.py
        └── input_data/
            ├── Admissions.csv
            ├── Diagnoses_ICD.csv
            ├── Procedures_ICD.csv
            └── Labevents.csv
```

So once the data has the following structure, you can perform the preprocesing using the following commands:
```
python preprocess_mimic.py --input_path input_data --output_path {storage_path}
```
You will need to perform this in the command line in the mimic folder

The storage path is up to you to decide. For our example we have made another folder called output and used it as our destination for the processed information. If done correctly the file structure should look something like this:
```
.
└── Preprocessing/
    └── mimic/
        ├── preprocess_mimic.py
        ├── input_data/
        │   ├── Admissions.csv
        │   ├── Diagnoses_ICD.csv
        │   ├── Procedures_ICD.csv
        │   └── Labevents.csv
        └── output/
            ├── dx_map.p
            ├── lab_map.p
            ├── proc_map.p
            ├── test.tfrecord
            ├── test_csr.pkl
            ├── train.tfrecord
            ├── train_csv.pkl
            ├── validation-tfrecord
            └── validation_csr.pkl
```
After doing this you should have the preprocessed files needed to run the script.

## Processing
After getting the preprocessed data, you can go to the notebook to perform the runs. The code is done in a jupyter notebook so that the user can run it both on a local machine as well as directly on collab or kaggle, where jupyter notebooks are used.

To run the script simply change the folder under the parameters to the folder with the outputs. In the notebook it has the path in our example. For the hyperparameters you can tune it from there. Note that the embedding size is highly dependent on your VRAM. To prevent the VRAM consumption from exploding, a dynamic VRAM management is in place to ensure that OOM error does not occur.

Evaluation occurs while running and you can see the results of the run. For each run, the relevant information and the model is saved in the folder of your choice. The folder path can be given in the parameters section.

After running the training, you should get a folder structure similar to this:
```
.
└── output/
    └── lr_0.0001-input_128-output_128-dropout_0.2/
        ├── parameter0_1389
        ├── parameter1_1389
        ├── parameter2_1389
        ├── parameter3_1389
        ├── parameter4_1389
        └── train.txt
```
In this example we have ran a model with the hyperparameters for 5 epochs. The model saves the checkpoints at every epoch and at the end of the training, a train.txt file is generated with the relevant evaluation information. This information can be used for further processing.

## Hyperparameter Tuning(Optional)
Should you want to perform hyperparameter tuning, simply use the hyperparameter tuning section and give the list of hyperparameters you wish to vary on. A grid search method is used in this context due to the relatively high training times. Using bayesian optimization would take too long before the bayesian optimization can be effective.

## Summary
So in summary to run the code, you will need to:
1. Get access to the MIMIC-III dataset
2. Download the relevant information
3. Perform preprocessing using command line
4. Perform training using notebook
5. Obtain results via .txt file for further analysis

## Dependencies
The dependencies can be found under the requirements.txt
The base requirements have been adopted from the original repo
The most noteworthy changes are the usage of the most recent versions of PyTorch and TensorFlow (to generate the preprocessed files) and bitsandbytes for the 8bit adam optimizer.