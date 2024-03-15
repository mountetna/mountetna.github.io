---
layout: default
---
# Linking

## Introduction

At long last, after carefully defining our data models, naming grammar, and ingesting our data, we are ready to link our data into a hierarchy! Fortunately, having done all the heavy lifting, the remainder comes relatively painlessly.

## Adding a loader

Linking data into the library is done using the Polyphemus application, which allows us to configure data loaders that can link data from various sources. To create a loader, click the "Add Loader" button at the bottom of your project's Polyphemus page and select the relevant loader type.

Each loader has an associated configuration and secrets. Once these are setup, the loader can be configured to run (either once or regularly on an interval). Once the loader finishes running(usually in a few minutes), it will generate a log entry, which will either show successful completion of the task or relevant errors.

### Metis

The Metis loader is the main way to attach unorganized data from buckets in Metis into the project data model hierarchy. The loader can attach data to one or more models (click the "Add Model" button). To add data to a model, add a new script for that model and select a script type: file, file_collection or data_frame.

In each case we will pick a root folder to find files to scan along with a glob to specify the matching files. There is a button in the Metis loader UI allowing you to test the match and list the matching files.

In the case of the file and file_collection scripts, we select a file or file_collection attribute from the model. When the script runs, the matching files will be associated with the corresponding record and attribute. The Metis loader will glean the record name from the file path itself. *This will not work* unless there is a matching rule defined in Gnomon for the model; without this rule the loader will be unable to determine where to link the file.

The third case, the data_frame script, is slightly different. Here the filename or path is irrelevant; all matching files are scanned and parsed as a data frame (coming from a CSV, TSV or Excel file). In this case, each row in the data frame is a record, and must contain an identifier. Column names in this data frame can be mapped in the data_frame script to attribute names in the model. In addition "empty" values (e.g. "NA") can be defined.

#### Touching files

It is important to understand *when* the Metis loader will attempt to scan files. Because we don't want the loader to keep trying to load the same files over and over again, the loader will only examine files in the tree that have been updated since the last time it ran. Therefore, if you upload some data, run the loader, discover an error, and run it a second time, the files that caused the error will be ignored in the second run because they have already been scanned. To get the Metis loader to pay attention to your file again, you must update its modification time ("touching" the file but not changing it). The Metis loader UI allows you to touch files via the Match dialog. Simply match your files and touch them before setting the loader to run.

### Redcap

Many projects store clinical data in Redcap. With a Redcap API token, a Polyphemus Redcap loader can be configured to link data directly from Redcap into the data library. The Redcap loader allows data from Redcap fields to be pulled into models/attributes, and allows some conditional logic such that data can be transformed before entry.
