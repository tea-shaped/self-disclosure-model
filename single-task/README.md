# Usage of Single-task Models

The easiest way to use our single-task models is based on the the [Differential Language Analysis Toolkit (DLATK)](http://dlatk.wwbp.org).
Please follow the guidelines [here](https://github.com/dlatk/dlatk) to install it in your environment before continuing.

## Preparing the dataset

Each dataset (in csv format) you want to evaluate needs to be split in a messages and a labels file, with corresponding keys (we called that column 'message_id' in this tutorial) in each file. Once you've done that, activate your DLATK environment (if necessary) with the command

```bash
source activate dlatk
```

and import your csv's to your MySQL database. Further information on how this is done in the command line or in Python can be found [here](http://dlatk.wwbp.org/tutorials/tut_import_methods.html?highlight=importmethods). You should import your messages and labels in two separate MySQL tables.

To check whether above operations have been successful, activate mysql by typing 

```bash
mysql
```

and using the following commands:

```bash
use username;
show tables;
describe tablename;
```

You should now see a table with a list of your imported data as MySQL tables.

## Generating 

## Generating 1- to 3gram features

The 1- to 3gram features and the combined table have been created by the following commands:

```bash
python dlatkInterface.py -d username -t tablename -c columnname --add_ngrams -n 1 2 3 --combine_feat_tables 1to3gram

python dlatkInterface.py -d username -t tablename -c columnname -f 'feat$1to3gram$tablename$columnname$16to16' --feat_occ_filter --set_p_occ 0 --group_freq_thresh 1
```

For the medical dataset, the following commands have been used:

```bash
python dlatkInterface.py -d username -t medical_sd_messages -c message_id --add_ngrams -n 1 2 3 --combine_feat_tables 1to3gram

python dlatkInterface.py -d username -t medical_sd_messages -c message_id -f 'feat$1to3gram$medical_sd_messages$message_id$16to16' --feat_occ_filter --set_p_occ 0 --group_freq_thresh 1
```

## Generating topic features

Based on the 1gram feature table, the following commands create tables that indicate how strongly the LIWC topics were present in the text. Here, we use the LIWC2015 lexica:

```bash
python dlatkInterface.py -d username -t medical_sd_messages -c message_id --add_lex_table -l LIWC2015 
```

To view the created feat$cat_LIWC2015$medical_sd_messages$message_id$1gra table, activate the mysql environment again and perform the following command:

```bash
select * from akreuel.feat$cat_LIWC2015_w$medical_sd_messages$message_id$1gra limit 10;
```


## Generating topic clouds

Because all weights in LIWC are 1, we need to create a feature table that 

The LIWC2015 topic clouds corresponding to the medical dataset can be found in the results folder. They were created using the following command in the activated DLATK environment:

```bash
dlatkInterface.py -d username -t medical_sd_messages -c message_id \
-f 'feat$cat_LIWC2015_w$medical_sd_messages$message_id$1gra' \
--outcome_table Medical_SD_Labels  --group_freq_thresh 1 \
--outcomes label_cont label_cat --output_name LIWC_output \
--topic_tagcloud --make_topic_wordcloud --topic_lexicon LIWC2015 \
--tagcloud_colorscheme bluered
```

The topic clouds can be found in the LIWC_output_topic_tagcloud_wordclouds folder in the home directory of the user.
NOTE: username has to be replaced with the internal user ID again.

## Performing 10-Fold Cross Validation

Again, the DLATK environment has to be activated before the following command is typed to generate the 10-Folg Cross-Validation. In this example, both the continuous and categorical labels were used. The respective results were reported individually in the Results folder.

```bash
python dlatkInterface.py -d akreuel -t medical_sd_messages -c message_id \
-f 'feat$cat_LIWC2015_w$medical_sd_messages$message_id$1gra' 'feat$1gram$medical_sd_messages$message_id$16to16' \
--outcome_table Medical_SD_Labels  --group_freq_thresh 1 \
--outcomes label_cont label_cat --output_name medical_sd_output  \
--combo_test_reg --model ridgecv --folds 10
```

NOTE: username has to be replaced with the internal user ID again.

## Saving the Model to get Across Dataset Results

To save the corresponding model, we need to use pickle:

```bash
./dlatkInterface.py -d akreuel -t kraut_messages -c message_id --group_freq_thresh 1 -f 'feat$cat_LIWC2015$kraut_messages$message_id$1gra' 'feat$1to3gram$kraut_messages$message_id$16to16' --outcome_table kraut_labels --outcomes emodispos emodisneg --train_regression --model ridge1000 --save_model --picklefile ~/sd.LIWC.1to3grams.16to16ridge1000.gft1.pickle
```

This should result in the following message (this is also the path where your model is saved!):

```bash
[TRAINING COMPLETE]

[Saving /home/akreuel/sd.LIWC.1to3grams.16to16ridge1000.gft1.pickle]
```

## Applying the Model to get Across Dataset Results
