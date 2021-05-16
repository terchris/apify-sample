# Problems when debugging apify in vs code

This is a simplefied version of the simplest apify example found here https://sdk.apify.com/docs/examples/cheerio-crawler

I will here point out the following challenges when working and debugging locally 

* When debugging in vs code you must manually delete files otherwise you get unpredictable errors
  * Suggested solution and workaround
* APIFY_LOCAL_STORAGE_DIR in .env file is ignored
  * Suggested solution and workaround


You can test it in gitpod.  Here is a full dev environment with vs code running in the browser.
[![Gitpod Ready-to-Code](https://img.shields.io/badge/Gitpod-ready--to--code-blue?logo=gitpod)](https://gitpod.io/#https://github.com/terchris/apify-sample)

Just click the link and then click the debug button to start debugging.

## When debugging in vs code you must manually delete files otherwise you get unpredictable errors
When debugging you will get errors like:
WARN  The following Key-value store directory contains a previous state:
      /workspace/apify-sample/apify_storage/key_value_stores/default
      If you did not intend to persist the state - please clear the respective directory and re-start the actor.

The reason for this is that Apify uses the folder apify_storage to store its internal workings. 
The first time the program is run the folder "apify_storage" is created.
Inside this directory you will see the following structure:

```
apify_storage
|- datasets
|- key_value_stores
|-- default
```
Inside the default folder you find the following files
* SDK_CRAWLER_STATISTICS_0.json
* SDK_SESSION_POOL_STATE.json
* SDK_start-urls-REQUEST_LIST_REQUESTS.bin

The files are never deleted. You must manually delete them every time you debug. If you do not delete the files you will get the state from the first time you run your program.

### Suggested solution and workaround
The functionality for cleaning out state is already implemented using the "-p" for purge parameter 
```
apify run -p
```
Adding it to the utils would be a solution eg:
```
await Apify.utils.purge();
```

The purge should be able to take a parameter so that when setting the APIFY_LOCAL_STORAGE_DIR from code works one could delete it using its name eg:
```
await Apify.utils.purge("my-local-storage-dir");
```

#### workaround
Hardcode deletion of folder "apify_storage" using Node.js File System Module

## APIFY_LOCAL_STORAGE_DIR in .env file is ignored
According to the documentation you can change the name of the folder "apify_storage" to something else by adding the following line to the .env file
```
APIFY_LOCAL_STORAGE_DIR="./my-local-storage-dir"
```
This example project has this setting, but it is ignored and the following warning message is displayed:
WARN  Neither APIFY_LOCAL_STORAGE_DIR nor APIFY_TOKEN environment variable is set, defaulting to APIFY_LOCAL_STORAGE_DIR="/workspace/apify-sample/apify_storage"


### Suggested solution and workaround
I suggest that it must be possible to set the name of the folder "apify_storage" in the code.
The log level can be set in .env variable and it can be overridden in code like this
```
log.setLevel(log.LEVELS.DEBUG); 
```
I suggest that the utils gets a function to set the APIFY_LOCAL_STORAGE_DIR Something like:
```
await Apify.utils.setLocalStorageDir("my-local-storage-dir");
```


