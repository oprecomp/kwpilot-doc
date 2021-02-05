..  Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

..    http://www.apache.org/licenses/LICENSE-2.0

..  Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.

.. _overview:

Overview
########

This page describes the system architecture of the OPRECOMP kW platform.

The goal is to develop one computation node of an HPC system, based on a high-performance processing node (IBM POWER 8/9) and an array of interconnected controllable precision processors.
The OPRECOMP kW demonstrator platform provides an evaluation framework to investigate scalability and functionality of the software stack. This platform is one node of an HPC system with transprecision capabilities.

The kwPilot demonstrator is developed in two stages. In the first phase, the system is based on existing POWER8 - CAPI (Coherent Accelerator Processor Interface) attached FPGA prototyping system developed by IBM. The POWER8 processor is used for precise calculations, and an interconnected array of PULP based processing units with controllable precision is implemented on the FPGA.

In the second phase, we have upgraded the node to a POWER9, with CAPI 2.0 connection to an advanced version of PULP, on a large-scale FPGA. This system is used to validate the approach.

The actual power and performance numbers will be extracted through measurements on a dedicated ASIC that will be manufactured containing an array of the PULP based transprecision computation cores. Finally, the results on the kW system will be evaluated and they will be further projected up to a potential multi-node MW architecture.

The first version of our kw demonstrator, namely, the kW-Pilot 1.0 platform, has been developed within the first phase specifications, i.e. POWER8 - CAPI1.0 attached FPGA prototyping platform.

The final version of our kw demonstrator, namely, the kW-Pilot 2.0 platform, has been developed within the final phase specifications, i.e. POWER9 - CAPI2.0 attached FPGA prototyping platform.

The development of the OPRECOMP kw-pilot is systematically developed in three pillars, namely the kw-pilot Hardware Development Kit (kwHDK), the kw-pilot Software Development Kit (kwSDK) and the kw-pilot Cloud Development Kit (kwCDK)

* The kw-pilot Hardware Development Kit (HDK) considers transprecision devices attached to a POWER8/9 node via CAPI1.0/CAPI2.0 or OpenCAPI3.0. We have experimented with inserting transprecision computing capabilities in different granularity levels. Specifically the transprecision devices can be either PULP cores or standalone acceleratos. In the former case the transprecision computing capabilities have been introduced at the instruction level, whereas at the latter case, we have designed customized hardware accelerators suppporting different precision across the computation progress.
* The kw-pilot Software Development Kit (SDK) describes the programming environment that enables the mapping of applications to this kw-pilot platform.
* The kw-pilot Cloud Development Kit (CDK) integrates the kW-Pilot platform to a cloud infrastructure of IBM Zurich Lab.


The kwHDK, the kwSDK and the kwCDK are supported by different design tools, libraries and APIs as depicted in the following figure.

.. image:: ../images/kw_tools_table.png

Fine-Grain Transprecision Computing
***********************************
In the fine-grain transprecision aspect of our kW-Pilot platform, we define a high-performance processing node (IBM POWER 8/9) and an array of interconnected controllable precision PULP processors (https://pulp-platform.org/).
The development of the OPRECOMP kw-pilot is systematically developed in three pillars, namely the kw-pilot Hardware Development Kit (kwHDK), the kw-pilot Software Development Kit (kwSDK) and the kw-pilot Cloud Development Kit (kwCDK), as depicted in the following figure.

.. image:: ../images/fig1.png


* The kw-pilot Hardware Development Kit (HDK.
* The kw-pilot Software Development Kit (SDK) describes the programming environment that enables the mapping of applications to this kw-pilot platform.
* The kw-pilot Cloud Development Kit (CDK) integrates the kW-Pilot platform to a cloud infrastructure of IBM Zurich Lab.

.. mdinclude:: ./hdk.md

.. mdinclude:: ./sdk.md




Coarse-Grain Transprecision Computing
*************************************

In the coarse-grain transprecision aspect of our kW-Pilot platform, we define a high-performance processing node (IBM POWER 8/9) and an array of interconnected controllable precision accelerators.
We have extended the open-source OC-Accel framework of OpenPOWER consortium with transprecision capabilities.
The OpenCAPI Transprecision Acceleration Framework, abbreviated as OC-Accel-TP, is a framework that helps you implement your FPGA acceleration solutions with OpenCAPI technology.
The framework is a fork of OC-Accel Framework, that also includes some examples with transprecision capabilities. 

https://github.com/oprecomp/oc-accel-tp 

In this case, we define:

* The kw-pilot Hardware Development Kit (HDK
 * Core design: https://github.com/oprecomp/oc-accel-tp/tree/master/hardware
 * Transprecision extensions: https://github.com/oprecomp/oc-accel-tp/tree/master/transprecision
* The kw-pilot Software Development Kit (SDK) describes the programming environment that enables the mapping of applications to this kw-pilot platform.
 * https://github.com/oprecomp/oc-accel-tp/tree/master/software
 * Transprecision extensions: https://github.com/oprecomp/oc-accel-tp/tree/master/transprecision
 * Example: https://github.com/oprecomp/oc-accel-tp/tree/master/actions/hls_tp_mult/sw
* The kw-pilot Cloud Development Kit (CDK) integrates the kW-Pilot platform to a cloud infrastructure of IBM Zurich Lab.
