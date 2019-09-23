Basic usage of tf.


1. FLAGS

flags.py
```python

# First: state which type you define by DEFINE_string/ DEFINE_bool / DEFINE_integer / DEFINE_float
# second: add three params 1. the name of this variable 2. the default value 3. the help info

import tensorflow as tf

flags = tf.flags
FLAGS = flags.FLAGS

# Define string
flags.DEFINE_string("input_file", None,
                    "Input raw text file (or comma-separated list of files).")
# Define boolean
flags.DEFINE_bool(
    "do_lower_case", True,
    "Whether to lower case the input text. Should be True for uncased "
    "models and False for cased models.")

# Define integer
flags.DEFINE_integer("max_seq_length", 128, "Maximum sequence length.")

# Define float
flags.DEFINE_float("masked_lm_prob", 0.15, "Masked LM probability.")
```
Usage of this file:
```cmd
python flags.py \
  --input_file=./sample_text.txt \
  --do_lower_case=True \
  --max_seq_length=128 \
  --masked_lm_prob=0.15 \
```
