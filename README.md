# Product Category
Product category prediction model trained based on [Amazon product data](http://jmcauley.ucsd.edu/data/amazon/).

## Installation
```Bash
pip install product_category
```

## Predict with Pre-trained Model
###
```
python product_category/predict.py -h
usage: predict.py [-h] -t TEXT [--trained_model_path TRAINED_MODEL_PATH]
                  [--i2cat_path I2CAT_PATH] [--tokenizer_name TOKENIZER_NAME]
                  [--topn TOPN]

optional arguments:
  -h, --help            show this help message and exit
  -t TEXT, --text TEXT  Product info text to predict.
  --trained_model_path TRAINED_MODEL_PATH
                        Model used to predict.
  --i2cat_path I2CAT_PATH
                        File name for the ordered list of categories. Each
                        line for one category.
  --tokenizer_name TOKENIZER_NAME
                        Tokenizer name.
  --topn TOPN           Number of top predicted categories to display.
  ```
```
python predict.py -t "Lykmera Famous TikTok Leggings, High Waist Yoga Pants for Women, Booty Bubble Butt Lifting Workout Running Tights"

Sports & Outdoors: 0.997
Sports & Fitness: 0.994
Exercise & Fitness: 0.980
Clothing: 0.961
Yoga: 0.905
```

## Training Data Format
Training data file should be csv with 3 columns: `category` (categories separated by '|'), `title` (str), `is_validation` (0 or 1). Similar to `data/example_data.csv`. 
```
category,title,is_validation
Sports & Outdoors|Outdoor Recreation|Cycling|Clothing|Men|Shorts,Louis Garneau Men's Neo Power Motion Bike Shorts,1
"Clothing, Shoes & Jewelry|Novelty & More|Clothing|Novelty",Nirvana Men's Short Sleeve Many Smiles T-Shirt Shirt,0
Grocery & Gourmet Food|Snack Foods|Chips & Crisps|Tortilla,Doritos Tapatio Salsa Picante Hot Sauce Flavor Chips 7 5/8 oz Bag (Pack of 1),0
"Clothing, Shoes & Jewelry|Women|Shoes|Boots|Synthetic|Synthetic sole|Vegan Friendly",SODA Womens Dome-H Boot,1
Sports & Outdoors|Outdoor Recreation|Camping & Hiking,Folding Pot Stabilizer,0
```

## Training
Run `python train.py -h` to see full 
```
python train.py -h
usage: train.py [-h] [--model_name_or_path MODEL_NAME_OR_PATH]
                [--transfer_learn] [--trained_model_path TRAINED_MODEL_PATH]
                [--data_file_path DATA_FILE_PATH] [--freeze_bert]
                [--max_seq_length MAX_SEQ_LENGTH]
                [--min_products_for_category MIN_PRODUCTS_FOR_CATEGORY]
                [--train_batch_size TRAIN_BATCH_SIZE]
                [--val_batch_size VAL_BATCH_SIZE]
                [--dataloader_num_workers DATALOADER_NUM_WORKERS]
                [--pin_memory] [--logger [LOGGER]]
                [--learning_rate LEARNING_RATE] [--adam_beta1 ADAM_BETA1]
                [--adam_beta2 ADAM_BETA2] [--adam_epsilon ADAM_EPSILON]

optional arguments:
  -h, --help            show this help message and exit
  --model_name_or_path MODEL_NAME_OR_PATH
                        Path to pretrained model or model identifier from
                        huggingface.co/models.
  --transfer_learn      Wether to use transfer learning based on a pretrained
                        model.
  --trained_model_path TRAINED_MODEL_PATH
                        Model used to predict.
  --data_file_path DATA_FILE_PATH
                        Path to training data file. Data file should be csv
                        with 3 columns: category (categories separated by
                        '|'),title (str),is_validation (0 or 1). e.g.: Sports
                        & Outdoors|Outdoor
                        Recreation|Cycling|Clothing|Men|Shorts,Louis Garneau
                        Men's Neo Power Motion Bike Shorts,1
  --freeze_bert         Whether to freeze the pretrained model.
  --max_seq_length MAX_SEQ_LENGTH
                        The maximum total input sequence length after
                        tokenization. Sequences longer than this will be
                        truncated, sequences shorter will be padded.
  --min_products_for_category MIN_PRODUCTS_FOR_CATEGORY
                        Minimum number of products for a category to be
                        considered in the model.
  --train_batch_size TRAIN_BATCH_SIZE
                        How many samples per batch to load for train
                        dataloader.
  --val_batch_size VAL_BATCH_SIZE
                        How many samples per batch to load for validation
                        dataloader.
  --dataloader_num_workers DATALOADER_NUM_WORKERS
                        How many subprocesses to use for data loading. 0 means
                        that the data will be loaded in the main process.
  --pin_memory          Wether to use pin_memory in pytorch dataloader. If
                        True, the data loader will copy Tensors into CUDA
                        pinned memory before returning them.
  --learning_rate LEARNING_RATE
                        Learning Rate
  --adam_beta1 ADAM_BETA1
                        The beta1 parameter in Adam, which is the exponential
                        decay rate for the 1st momentum estimates.
  --adam_beta2 ADAM_BETA2
                        The beta2 parameter in Adam, which is the exponential
                        decay rate for the 2nd momentum estimates.
  --adam_epsilon ADAM_EPSILON
                        The epsilon parameter in Adam, which is a small
                        constant for numerical stability.
```

### Training from Scratch

For example
```
python train.py --data_file_path ../data/sample_data.csv

```
### Transfer Learning from Pre-trained Model
For example
```
python train.py --transfer_learn --data_file_path ../data/sample_data.csv
```


