.. _run-a-fastq-galaxy-job:

How to run a FastQC read analysis job using your Galaxy environment. 

The following guide will detail how to upload publicly available genome data to your Galaxy environment - and then run a FastQ analsysis on that data using the available Pulsar compute hosts.

Prerequisites
=============

- Alces Galaxy environment deployed
- Access to Galaxy environment obtained, logged in with provided acccount

Uploading data
==============
To upload the ``hg19`` data to your Galaxy environment - first log in to your environment, then navigate to the ``Upload File - From your computer`` tab from the tool box on the left. 

1.  Select the ``Paste/Fetch Data`` button
2.  Enter one URL per line, use the following URLs
  -  https://s3-eu-west-1.amazonaws.com/alces-public/galaxy-demo/hg19/hg19_ERR030856-ERR030856-SUBMITTED.fastq.fastq
  -  https://s3-eu-west-1.amazonaws.com/alces-public/galaxy-demo/hg19/hg19_ERR030856-ERR030856-FASTQ.fastq.fastqsanger
3.  Click the ``Start`` button to begin uploading to your local Galaxy environment

Install the FastQ application
=============================
From the Galaxy web console, navigate to the ``Admin`` tab - then follow the below instructions: 

1.  Select the ``Search tool shed`` button
2.  Select ``Galaxy Main Tool Shed``
3.  Search ``fastqc`` in the search box
4.  From the list of available options, select ``FastQC``
5.  Select ``Preview and Install``
6.  Select ``Install to Galaxy``
7.  Leave all settings default unless you wish to customise your toolbar
8.  Click ``Install`` to start installing the ``FastQC`` tool to your local toolshed

Running the analysis
====================
From the Galaxy web console, navigate to the ``Analyze Data`` tab - then follow the below instructions:

1.  From the available tools on the left - select the ``FastQC - Read Quality Reports`` application
2.  Select the input files to use
  -  ``Short read data``: Select any one of the previously uploaded datasets
  -  ``Contaminant list``: Leave blank
  -  ``Submodule file``: Leave blank
3. Click ``Execute``
4. Your job will now begin running on one of the available Pulsar compute hosts. From the history viewer on the right hand side, you will be able to see your output data once the job has sucessfully run. 
5. FastQC Read Reports output a HTML page for you to view the available output data. 

What's next?
============
For more advanced Galaxy usage - read the Galaxy project wiki: 

-  https://wiki.galaxyproject.org/
