# Logging messsages

## Defining location of log file

```python
import logging
logging.basicConfig(filename='app.log', filemode='w', format='%(name)s - %(levelname)s - %(message)s')
```


## Specify log type

There are several types of logs:

* DEBUG
* INFO
* WARNING
* ERROR
* CRITICAL

```python
import logging

logging.debug('This is a debug message')
logging.info('This is an info message')
logging.warning('This is a warning message')
logging.error('This is an error message')
logging.critical('This is a critical message')
```


# Zarr array file


