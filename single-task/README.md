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

|Features|DLATK Command                                                                                                                 |Link (where applicable)|
|----------|-----------------------------------------------------------------------------------------------------------------------------|-----------------------|
|nGrams    |```  python dlatkInterface.py -d <database> -t <message_table> -c <group_column> --add_ngrams -n 1 2 3 --combine_feat_tables 1to3gram``` | [Link](http://dlatk.wwbp.org/fwinterface/fwflag_add_ngrams.html)|
|LDA Topics|```  dlatkInterface.py -d <database> -t <message_table> -c <group_column> --add_lex_table -l met_a30_2000_cp --weighted_lexicon``` | [Link (Chapter "Generate Lexicon (topic) Features")](http://dlatk.wwbp.org/tutorials/tut_dla.html)|
|LIWC      |```  dlatkInterface.py -d <database> -t <message_table> -c <group_column> --add_lex_table -l LIWC2015``` | [Link (Chapter "Generate Lexicon (topic) Features")](http://dlatk.wwbp.org/tutorials/tut_dla.html)|
|RoBERTa   |See explanation below; ```dlatkInterface.py -d dla_tutorial -t msgs_xxx -c user_id --add_bert --bert_model roberta-uncased```| [Link](http://dlatk.wwbp.org/tutorials/tut_bert.html)|

### RoBERTa Features

