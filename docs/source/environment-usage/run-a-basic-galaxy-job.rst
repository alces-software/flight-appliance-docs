.. _run-a-basic-galaxy-job:

How to run a basic job using your Galaxy environment. 

The following guide will detail how to upload sample data to your Galaxy environment, and then run a basic job using the Pulsar compute service on either a single-node Galaxy environment, or a multiple-node Galaxy environment. 

Prerequisites
=============

- Alces Galaxy environment deployed
- Access to Galaxy environment obtained, logged in with provided acccount

Uploading data
==============
First - some test data will need to be added to the Galaxy environment in order to run our example job, from the Galaxy web console: 

1. Using the ``Tools`` pane on the left-hand side - navigate to the ``Get Data -> Upload File`` page 
2. Press the ``Paste/Fetch data`` button, and input the following sample data: 

.. code-block:: yaml

    myfile:
      description: Some content

3. Click the ``Start`` button to begin the upload process. 
4. Galaxy will provide feedback on the successful upload of your example data

Running an example task
=======================
From the Galaxy web console: 

1. Using the ``Tools`` pane on the left-hand side - navigate to the ``Filter and Sort -> Select lines that match an expression`` page
2. Using the Galaxy ``Select`` tool, select your previously pasted entry from the input files list in the ``Select lines from`` dropdown
3. Select ``matching`` in the ``that`` field
4. Enter ``description`` in the ``the pattern`` field
5. Click ``Execute`` to submit your task to the Galaxy compute host(s) pool
6. The job will be sent to the task queue, view its status and output by clicking on the job name in the ``History`` tab on the right-hand side. 
7. The output should display the following line from our previously uploaded data: 

.. code-block:: yaml

      description: Some content

What's next?
============
For more advanced Galaxy usage - read the Galaxy project wiki: 

-  https://wiki.galaxyproject.org/
