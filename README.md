# __`metriculous_forked_repo`__

Measure, visualize, and compare machine learning model performance without the usual boilerplate.
Breaking API improvements to be expected.

# Compare Classification Models
## Documentation for using config.py script and creating a configuration file

*  With **config.py** script we create the configuration (INI) file. Essentially, the file consist of sections, each of which contains keys with values. There are three sections that we have created:

1. **Quantity** : Where the key is name of the quantity, such as: "_Accuracy_", "_ROC AUC Macro Average_", "_ROC AUC Micro Average_", "_Log Loss_", etc. The value of the keys is "_yes_" or "_no_", which indicates whether we want to compute and show the corresponding quantity or not.

2. **Figures** :  Where the key is name of the figure (graph), such as: "_Class Distribution_", "_Confusion Matrix_", "_ROC_", "_PR_", etc. The value of the keys is "_yes_" or "_no_", which indicates whether we want to compute and show the corresponding quantity or not.

3. **Threshold** :  Where the key is name of the model, such as: "_model_1_", "_model_2_", "_model_3_", etc. The value of the keys is a numeric value (0.0 - 1.0), which represents a threshold value for the correspondig model name.


* _Running `config.py` script_ 

```console
$ python3 config.py 
```

After running the `config.py` script we get the `configuration.ini` file with the quantities and figures that we want to include and exclude from the calculation and a graphical representation. Also, in the configuration file we get the thresholds for each model. 

## Documentation for using main.py script

* When we run the `main.py` script on the command line we enter the configuration file (`configurations.ini`) which contains quantities and figures that we have selected to be included and excluded from the calculation and representation, and the thresholds for each  model. The rest of the arguments that we read from command line are model files in **.csv** format. The model files (.csv files) contain informations about: dilution, ground truth and predictions for each class. The first two coulumns (dilution and ground truth) must have equal values in each model file for each instance (sample). Dilution coulumn has numeric values or it could be empty, if a sample has an empty dilution value, then we set it to 1. Ground truth column contains a name of the class, that must match with the class names in the header of the model files and later we do a label encoding for the class names in order to encode them in a numeric value.
The values of the class predictions have to be between 0.0 and 1.0. All model files have the same class names. 
* `_def filter_inconclusive_` function filters the inconclusive samples (patients). We compare the probabilities of all class prediction with the model threshold. If all the predictions for a given sample are lower than model threshold, then that sample is inconclusive and we do not take that sample in the calculations.
* `_def filter_inconclusive_`  arguments:
    * **model_thresholds**: a list of thresholds for each model (one threshold per model).
    * **models_dic**: a dictionary of models predictions.
* `_def filter_inconclusive_`  returns:
    * **models_dic_final**: a model dictionary with removed inconclusive samples for each model.
    * **inconclusive_samples**: a dictionary with inconclusive samples for each model (where a model threshold is above the max probability of its predictions)   

* _Running `main.py` script_

```console
$ python3 main.py configurations.ini model1.csv model2.csv
```

* `main.py` script opens and reads all the files (configuration file and model files) from command line, extracts all the informations from them, such as: _dilution_ array, _ground truth_ array, _model_predictions_ ndarray, list of quanitites and figures that have to be excluded from calculation and thresholds for each model.