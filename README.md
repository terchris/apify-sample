# The simplest apify

This is a simplefied version of the simplest apify example found here https://sdk.apify.com/docs/examples/cheerio-crawler

I will here point out the following challenges when working locally 

1) When debugging in vs code you must manually delete files otherwise you get unpredictable errors


2) 






1) When debugging in vs code you must manually delete files otherwise you get unpredictable errors
When debugging you will get errors like:
WARN  The following Key-value store directory contains a previous state:
      /workspace/apify-sample/apify_storage/key_value_stores/default
      If you did not intend to persist the state - please clear the respective directory and re-start the actor.

The reason for this is that Apify uses the folder apify_storage to store its internal workings. 
The first time the program is run the folder "apify_storage" is created.
Inside this directory you will see the following structure:

´´´
apify_storage
|- datasets
|- key_value_stores
|-- default
´´´
Inside the default folder you find the following files
SDK_CRAWLER_STATISTICS_0.json
SDK_SESSION_POOL_STATE.json
SDK_start-urls-REQUEST_LIST_REQUESTS.bin

The files are never deleted. You must manually delete them every time you debug. If you do not delete the files you will get the state from the first time you run your program.




