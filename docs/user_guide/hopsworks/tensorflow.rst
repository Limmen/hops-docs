TensorFlow on Hops
==================

Hops provides support for running TensorFlow on a Hops cluster. The development environment for doing so is in Jupyter notebooks.
The datasets that you are working with should be uploaded to your HopsWorks project, or you can add a dataset from a different project to be accessible by your project. HopsFS is the filesystem of Hops, it is essentially a fork of HDFS and is compliant with any API that can read data from an HDFS path, such as TensorFlow. In your TensorFlow code you can simply replace local file paths to the corresponding path in HDFS.

https://www.tensorflow.org/deploy/hadoop


Working with TensorFlow in Jupyter
----------------------------------
We currently support three different modes of running TensorFlow, that are meant for different purposes and use-cases.
To execute the Python code in YARN containers on the Hops cluster, we make use of Spark to distribute and execute the code, so effectively we run TensorFlow on Spark.
When starting Jupyter in HopsWorks, certain configuration properties need to be filled in and understood, therefore it is recommended to look at each guide to understand the implications of these values. Each of the links below will go through the configuration properties that can be set for each mode.


Hops python library
-------------------
`hops` python library was developed to make it simple to run TensorFlow on Hops and scale-out training and increase productivity.
It contains several submodules that can be used for interacting with TensorBoard, GPUs, HopsFS and parallelizing training.


       ../tensorflow/hops.rst

Mode 1. Parallel TensorFlow experiments
-----------------------------------------

The use-case of this mode is to run multiple parallel experiments where you have a set of predefined hyperparameters and a list of values per hyperparameter that should be used to run training with.  a set of hyperparameters and find the best set of hyperparameters for the model. define a TensorFlow function containing the code to run, with hyperparameters as the arguments for this function.
This would effectively create multiple jobs, where each job is the same TensorFlow code running with a distinct set of hyperparameter values.
By increasing the number of Spark executors for Jupyter, it would be possible to run multiple jobs in parallel and increase throughput.
Furthermore, the training for all the jobs can be visualized in the same TensorBoard in HopsWorks. Each TensorBoard logdir is then placed in your HopsWorks project,
versioned with the particular hyperparameters set for that job.


       ../tensorflow/tflauncher.rst

Mode 2. Distributed TensorFlow with TensorFlowOnSpark
---------------------------------------------

Originally developed by Yahoo, TensorFlowOnSpark is essentially a wrapper for Distributed TensorFlow (https://www.tensorflow.org/deploy/distributed) and in that sense, TensorFlowOnSpark supports all features which Distributed TensorFlow provides, such as asynchronous or synchronous training.
Hops have since done some improvements since we have the ability to schedule GPUs, it is for example possible to define how many GPUs each worker should be allocated.

The `TFCluster` API remains the same, so any existing examples will run on TensorFlowOnSpark on Hops. The framework is then fully compatible with the python libraries provided by Hops such as ``tensorboard`` and ``hdfs``.


       ../tensorflow/tensorflow_on_spark.rst

Mode 3. Optimal scalability with Horovod
------------------------------------

Horovod is a distributed training framework for TensorFlow. The goal of Horovod is to make distributed Deep Learning fast and easy to use. Compared to TensorFlowOnSpark (Distributed TensorFlow), the programming model is significantly simpler, and it requires only a couple of changes to your existing code to convert a non-distributed training code to distributed and scale over 100s of GPUs.

       ../tensorflow/horovod.rst
