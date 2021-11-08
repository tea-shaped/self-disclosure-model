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

You should now see a table with a list of your imported data as MySQL tables. In case you need to do some additional data cleaning, you can check out [here](http://dlatk.wwbp.org/tutorials/tut_data_cleaning.html) what DLATK can do for you.

## Generating Feature Tables

With DLATK, you can generate features tables for a variety of features easily. Here's an overview of the features we used with the corresponding commands in DLATK. Where applicable, we also linked the DLATK documentation for the respective features. If you want a more in-depth tutorial on feature tables in DLATK, check out [this link](http://dlatk.wwbp.org/tutorials/tut_feat_tables.html).

|Features  |DLATK Command                                                                                                                 |Link (where applicable)|
|----------|-----------------------------------------------------------------------------------------------------------------------------|-----------------------|
|nGrams    |```bash  
python dlatkInterface.py -d <database> -t <message_table> -c <group_column> --add_ngrams -n 1 2 3 --combine_feat_tables 1to3gram``` | [Link](http://dlatk.wwbp.org/fwinterface/fwflag_add_ngrams.html)|
|LDA Topics|```bash  
pyhton dlatkInterface.py -d <database> -t <message_table> -c <group_column> --add_lex_table -l met_a30_2000_cp --weighted_lexicon``` | [Link (Chapter "Generate Lexicon (topic) Features")](http://dlatk.wwbp.org/tutorials/tut_dla.html)|
|EmoLex    | ```bash  
pyhton dlatkInterface.py -d <database> -t <message_table> -c <group_column> --add_lex_table -l NRC_emot_v0_92``` | [Link (Chapter "Generate Lexicon (topic) Features")](http://dlatk.wwbp.org/tutorials/tut_dla.html)|
|LIWC      |```bash 
python dlatkInterface.py -d <database> -t <message_table> -c <group_column> --add_lex_table -l LIWC2015``` | [Link (Chapter "Generate Lexicon (topic) Features")](http://dlatk.wwbp.org/tutorials/tut_dla.html)|
|RoBERTa   |See explanation below; ```bash
python dlatkInterface.py -d <database> -t <message_table> -c <group_column> --add_bert --bert_model roberta-uncased```| [Link](http://dlatk.wwbp.org/tutorials/tut_bert.html)|

### RoBERTa Features

The general feature extraction process in DLATK is almost the same as for the other features mentioned above. However, there are two additional steps you need to do.

Firstly, you need to ensure that punkt is installed for NLTK:
```bash 
python -m nltk.downloader punkt
```

Secondly, you need to add a MySQL table containing your tokenized sentences with the following command:
```bash 
python dlatkInterface.py -d <database> -t <message_table> -c <group_column> --add_sent_tokenized
```

This will create a table called '<message\_table>\_stoks'. Finally, you can use the command mentioned in the table above to create your feature table for further analysis:
```bash 
python dlatkInterface.py -d <database> -t <message_table> -c <group_column> --add_bert --bert_model roberta-uncased
```

## Applying a Pickle Model

After you've generated your feature tables, you can finally use our single-task models (YAY!)! To do so, substitute the relevant parameters in the following command (explanations adapted from the DLATK documentation).

```bash 
python dlatkInterface.py -d <database> -t <message_table> -c <group_column> --group_freq_thresh <group_frequency_threshhold> -f '<feature_table>' --outcome_table <outcome_table> --outcomes <output_column> --predict_regression_to_feat <table_name_prediction_results>  --load --picklefile \~/<single-task-model_matching_feature_table>
```

|Parameter                                    | Explanation                                                                             |
|---------------------------------------------|-----------------------------------------------------------------------------------------|
|[<data_base>](http://dlatk.wwbp.org/fwinterface/fwflag_d.html)                                 | The database where your MySQL tables are stored                                         |
|[<message_table>](http://dlatk.wwbp.org/fwinterface/fwflag_t.html)                            | The message table where your messages are stored                                        |
|[<group_column>](http://dlatk.wwbp.org/fwinterface/fwflag_c.html)                             | The name of the column that contains your keys                                          |
|[<group_frequency_threshhold>](http://dlatk.wwbp.org/fwinterface/fwflag_group_freq_thresh.html)               | Counts the number of words in each group specified by -c (the group field). If this count is less than the given group frequency threshold then this group is thrown out. The group is otherwise kept.            |
|[<feature_table>](http://dlatk.wwbp.org/fwinterface/fwflag_f.html)                            | The name of the feature table in MySQL.                                                 |
|[<outcome_table>](http://dlatk.wwbp.org/fwinterface/fwflag_outcome_table.html)                            | Table containing the values for the outcomes.                                           |
|[<output_column>](http://dlatk.wwbp.org/fwinterface/fwflag_outcomes.html)                            | Name of the column where the labels are in your labels table                                         |
|[<table_name_prediction_results>](http://dlatk.wwbp.org/fwinterface/fwflag_predict_regression_to_feats.html)            | Puts predicted results in the MySQL table specified here                                         |
|<single-task-model_matching_feature_table> | The .pickle model you want to use. It has to match the feature table you're using in <feature_table>.                                     |



For more information, check out the DLATK page [here](http://dlatk.wwbp.org/tutorials/tut_pickle_apply.html?highlight=pickle). 
