# scify-file-reader
The scify-file-reader package provides a convenient class for handling multiple files with the same structure in a directory. It offers functionality to read and process data from various file types, including CSV, XLSX, Parquet, and JSON.

## Installation

You can install scify-file-reader using pip:

```shell
pip install scify-file-reader
```

## Usage

To use scify-file-reader, follow these steps:

1. Import the `FileReader` class:

```python
from scify_file_reader import FileReader
```

2. Create an instance of the FileReader class, providing the content you want to read. The content can be a string representing a `file path`, a `Path` object, or a `zipfile.ZipFile` object:
```python 
content = 'path/to/directory'
reader = FileReader(content)
```

3. Read the files using the read_files method:
```python
data = reader.read_files()
```

The `read_files` method returns a dictionary where the keys are the filenames (without the extension) and the values are pandas DataFrames containing the file data.

**For more details on the available methods and parameters, refer to the package documentation.**


## Examples:
Here's an example that demonstrates how to use scify-file-reader:

### Normal Output
```python
from scify_file_reader import FileReader

PATH = '/path/to/directory'

"""
# Supomos que temos estes arquivos dentro do nosso diretório
print(os.listdir(PATH))
# OUT: ['file_1.csv'', 'log_2.csv', 'test_3.csv',
        'file_%Y%m%d%H%M%S.csv', 'log_%Y%m%d%H%M%S.csv', 'test_%Y%m%d%H%M%S.csv', 
        'file_%Y%m%d_%H%M%S.csv', 'log_%Y%m%d_%H%M%S.csv', 'test_%Y%m%d_%H%M%S.csv', 
"""

# Example: Reading files from a directory
reader = FileReader('/path/to/directory')
data = reader.read_files() # read_files accept kwargs from pandas read_ methods

"""
OUTPUT: print(data)
{
    'file_1.csv': <pd.DataFrame>,
    'log_2.csv': <pd.DataFrame>,
    'test_3.csv': <pd.DataFrame>,
    'file_%Y%m%d%H%M%S.csv': <pd.DataFrame>,
    'log_%Y%m%d%H%M%S.csv': <pd.DataFrame>,
    'test_%Y%m%d%H%M%S.csv': <pd.DataFrame>,
    'file_%Y%m%d_%H%M%S.csv': <pd.DataFrame>,
    'log_%Y%m%d_%H%M%S.csv': <pd.DataFrame>,
    'test_%Y%m%d_%H%M%S.csv': <pd.DataFrame>
}
"""

```

### Concatenating patterns:
Use this method when you want to concatenate multiple files as one's based on the pattern.

**E.g.** In the last example we have 9 files on the same directory, with common patterns, such as 'file', 'log' and 'test'. Why do not join them? Let's see the example

```python
from scify_file_reader import FileReader

PATH = '/path/to/directory'

"""
# Supomos que temos estes arquivos dentro do nosso diretório
print(os.listdir(PATH))
# OUT: ['file_1.csv'', 'log_2.csv', 'test_3.csv',
        'file_%Y%m%d%H%M%S.csv', 'log_%Y%m%d%H%M%S.csv', 'test_%Y%m%d%H%M%S.csv', 
        'file_%Y%m%d_%H%M%S.csv', 'log_%Y%m%d_%H%M%S.csv', 'test_%Y%m%d_%H%M%S.csv', 
"""

# Example: Reading files from a directory
reader = FileReader('/path/to/directory')
data = reader.read_files(join_prefixes=True) #

"""
OUTPUT: print(data)
{
    'file': <pd.DataFrame>,
    'log': <pd.DataFrame>,
    'test': <pd.DataFrame>,
}
"""
```

### Using a specific regular expression

What happens in the example above is that all common files prefixes was joined together such as `file_1.csv`, `file_%Y%m%d%H%M%S.csv`, `file_%Y%m%d_%H%M%S.csv` are now `file` key on the output.  

If you want to use a specific regular expression for your files, you can do the following:

```python
from scify_file_reader import FileReader

PATH = '/path/to/directory'

# Example: Reading files from a directory
reader = FileReader('/path/to/directory')

regex = '<some_regex>'
reader.set_prefix_file_pattern_regex(regex)

data = reader.read_files(join_prefixes=True) 
```

By default the regular expression is `^([A-Z]+)_\d+`.

### Speficic prefixes instead of regular expressions

If you want to use specific prefixes instead of regular expressions you can use the `join_custom_prefixes` argument that requers a tuple.

```python
from scify_file_reader import FileReader

PATH = '/path/to/directory'

"""
# Supomos que temos estes arquivos dentro do nosso diretório
print(os.listdir(PATH))
# OUT: ['file_1.csv'', 'log_2.csv', 'test_3.csv',
        'file_%Y%m%d%H%M%S.csv', 'log_%Y%m%d%H%M%S.csv', 'test_%Y%m%d%H%M%S.csv', 
        'file_%Y%m%d_%H%M%S.csv', 'log_%Y%m%d_%H%M%S.csv', 'test_%Y%m%d_%H%M%S.csv', 
"""


# Example: Reading files from a directory
reader = FileReader('/path/to/directory')

specific_prefixes = ('file', 'log', 'test')

data = reader.read_files(join_prefixes=True) 

"""
OUTPUT: print(data)
{
    'file': <pd.DataFrame>,
    'log': <pd.DataFrame>,
    'test': <pd.DataFrame>,
}
"""
```

## Contributing
Contributions are welcome! If you have any suggestions, bug reports, or feature requests, please open an issue or submit a pull request on the [scify-file-reader](https://github.com/Jeferson-Peter/scify-file-reader) repository.
