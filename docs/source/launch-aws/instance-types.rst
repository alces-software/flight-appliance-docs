:orphan:
 .. _instance-types:

AWS EC2 Instance Types for Flight Compute
#########################################

The Alces Flight Compute CloudFormation templates include a number of popular AWS EC2 instance types for different use cases. The following table details each instance type provided for Flight Compute dedicated compute hosts: 

.. list-table::
   :header-rows: 1
   :widths: 2 2 2 2 2

   *  -  Description 
      -  Cores [1]_
      -  RAM (GB)
      -  GB/core 
      -  AWS instance type
   *  -  compute.small 
      -  2 
      -  3.75
      -  2
      -  ``c4.large``
   *  -  compute.medium 
      -  8 
      -  15
      -  2
      -  ``c4.2xlarge``
   *  -  compute.large 
      -  16 
      -  30
      -  2
      -  ``c4.4xlarge``
   *  -  compute.dedicated 
      -  36
      -  60
      -  1.66
      -  ``c4.8xlarge``
   *  -  balanced.small 
      -  4 
      -  16
      -  4
      -  ``m4.xlarge``
   *  -  balanced.medium 
      -  8 
      -  32
      -  4
      -  ``m4.2xlarge``
   *  -  balanced.large 
      -  16 
      -  64
      -  4
      -  ``m4.4xlarge``
   *  -  balanced.dedicated 
      -  40 
      -  160
      -  4
      -  ``m4.10xlarge``
   *  -  memory.small 
      -  4
      -  30.5
      -  8
      -  ``r3.xlarge``
   *  -  memory.medium 
      -  8
      -  61
      -  8
      -  ``r3.2xlarge``
   *  -  memory.large 
      -  16
      -  122
      -  8
      -  ``r3.4xlarge``
   *  -  memory.dedicated 
      -  32
      -  244
      -  8
      -  ``r3.8xlarge``
   *  -  gpu.medium 
      -  8 
      -  15
      -  2
      -  ``g2.2xlarge``
   *  -  gpu.dedicated 
      -  32 
      -  60
      -  2
      -  ``g2.8xlarge``

********************************
Compute optimised instance types
********************************

The following table details each of the compute optimised instance types available for use with the Alces Flight Compute CloudFormation templates. The compute optimised instance types are useful for most compute jobs and general workload processing. Each compute instance provided offers 2 GB memory per CPU core.

.. list-table::
   :stub-columns: 1
   :widths: 20 20 20 20 20

   *  -  CloudFormation identifier
      -  ``compute.small-c4.large``
      -  ``compute.medium-c4.2xlarge``
      -  ``compute.large-c4.4xlarge``
      -  ``compute.dedicated-c4.8xlarge``
   *  -  Cores per instance [1]_ 
      -  2
      -  8
      -  16
      -  36
   *  -  Memory per instance (GB)
      -  3.75
      -  15
      -  30
      -  60
   *  -  GB/core 
      -  1.87
      -  1.87 
      -  1.87
      -  1.66
   *  -  Share of 10GbE interface [2]_
      -  Low (10-15%)
      -  Medium (55-60%)
      -  High (75-80%)
      -  Full

***********************
Balanced instance types
***********************

The following table details each of the balanced instance types available for use with the Alces Flight Compute CloudFormation templates. The balanced instance types typically include a higher amount of memory per core than the compute optimised and are useful for most compute jobs and general workload processing. Each instance type provided offers 4GB memory per CPU core.

.. list-table::
   :stub-columns: 1
   :widths: 20 20 20 20 20

   *  -  CloudFormation identifier
      -  ``balanced.small-m4.xlarge``
      -  ``balanced.medium-m4.2xlarge``
      -  ``balanced.large-m4.4xlarge``
      -  ``balanced.dedicated-m4.10xlarge``
   *  -  Cores per instance [1]_ 
      -  4
      -  8
      -  16
      -  40
   *  -  Memory per instance (GB)
      -  16
      -  32
      -  64
      -  160
   *  -  GB/core 
      -  4
      -  4
      -  4
      -  4
   *  -  Share of 10GbE interface [2]_
      -  Low (25-30%)
      -  Medium (55-60%)
      -  High (75-80%)
      -  Full

*******************************
Memory optimised instance types
*******************************

The following table details each of the memory optimised instance types available for use with the Alces Flight Compute CloudFormation templates. The memory optimised instance types include a much higher GB memory per core ratio than any other available EC2 instance type - making them particularly useful for memory-intensive workloads. Each instance type provided offers 7.6GB memory per CPU core. 

.. list-table::
   :stub-columns: 1
   :widths: 20 20 20 20 20

   *  -  CloudFormation identifier
      -  ``memory.small-r3.xlarge``
      -  ``memory.medium-r3.2xlarge``
      -  ``memory.large-r3.4xlarge``
      -  ``memory.dedicated-r3.8xlarge``
   *  -  Cores per instance [1]_ 
      -  4
      -  8
      -  16
      -  32
   *  -  Memory per instance (GB)
      -  30.5
      -  61
      -  122
      -  244
   *  -  GB/core 
      -  7.62
      -  7.62
      -  7.62
      -  7.62
   *  -  Share of 10GbE interface [2]_
      -  Low (25-30%)
      -  Medium (55-60%)
      -  High (75-80%)
      -  Full

**************************
GPU enabled instance types
**************************

The following table details each of the GPU enabled instance types available for use with the Alces Flight Compute CloudFormation templates. The GPU enabled instance types include a number of dedicated GPU cards with the instance available for use with your compute jobs. Each NVIDIA K520 GPU card included with an instance includes 1,536 CUDA cores and 4GB video memory.

.. list-table::
   :stub-columns: 1
   :widths: 20 20 20

   *  -  CloudFormation identifier
      -  ``gpu.medium-g2.2xlarge``
      -  ``gpu.dedicated-g2.8xlarge``
   *  -  Cores per instance [1]_ 
      -  8
      -  32
   *  -  Memory per instance (GB)
      -  15
      -  60
   *  -  Share of 10GbE interface [2]_
      -  High (75-80%)
      -  Full
   *  -  GPU cards included
      -  1 X NVIDIA GRID K520 GPU
      -  4 x NVIDIA GRID K520 GPUs

For more information on the available AWS EC2 instance types - please visit the AWS documentation

    - https://aws.amazon.com/ec2/instance-types/

.. [1] Includes Hyper-Threaded cores
.. [2] When launched into a ``cluster`` Placement Group

