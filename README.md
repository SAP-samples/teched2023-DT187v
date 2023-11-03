[![REUSE status](https://api.reuse.software/badge/github.com/SAP-samples/teched2023-DT187v)](https://api.reuse.software/info/github.com/SAP-samples/teched2023-DT187v)

# DT187v - Build Analytical Applications with ABAP Cloud

## Description

This repository contains the material for the SAP TechEd 2023 session called DT187v - Build Analytical Applications with ABAP Cloud.
Visit the [Jump-Start Session: Build Analytical Applications with ABAP Cloud](https://go3.events.sap.com/sapteched/virtual/2023/reg/flow/sap/sapteched23/catalog/page/catalog/session/1693019886304001sNN4) at Friday, November 3, 2023, 7pm CET, to get an introduction.
The [slides of the Jump-Start Session](/exercises/ex3/images/231018_Embedded-Analytics-for-TechEd_Ext.pdf) are available in this GitHub repository as well.


## Overview

This hands-on session introduces attendees to building an analytical data model with ABAP Cloud that can be consumed by analytical applications like e.g., SAP Analytics Cloud. You will learn how to build analytical applications with the ABAP Cloud programming language using ABAP core data services, analytical providers and business services in the ABAP development tools.

Specifically, you will build an analytical application, that uses SAP's Flight Demo Model. You will design the data model and the application in order to analyze the occupation of flights from different perspectives.

While building the data model, you will be introduced to analytical terms like measures, dimensions, cubes and queries as well as to the general idea of analytical data modelling with the star schema.<br>
Previewing the analytical application and exploring its features will deepen this understanding.

## Requirements

Experience in ABAP, ABAP CDS and working with the Eclipse based ABAP Development Tools (ADT) is recommended, but not strictly necessary. If you are in a hurry or find it hard to follow the instructions, intermediate and final solutions of the exercises are available ready to be copied & pasted (with low naming adaption effort).

The hard [prerequisites](https://github.com/SAP-samples/teched2023-DT187v/blob/main/exercises/ex0/README.md) to follow the exercises in this repository are that you:
1) get a new trial account for the BTP ABAP environment (please make sure to get a new one if your account is older than 2023-10-15)
2) have the latest version of the ABAP development tools installed
3) have downloaded and installed the templates for analytical CDS views

## Exercises

- [Prerequisites](https://github.com/SAP-samples/teched2023-DT187v/blob/main/exercises/ex0/README.md)
    - [Get a SAP BTP Trial Account](https://github.com/SAP-samples/teched2023-DT187v/blob/main/exercises/ex0/README.md#sap-btp-trial-account)
    - [Get the BTP ABAP Environment Trial](https://github.com/SAP-samples/teched2023-DT187v/blob/main/exercises/ex0/README.md#abap-environment-in-sap-btp-trial)
    - [Install Analytical CDS Templates](https://github.com/SAP-samples/teched2023-DT187v/blob/main/exercises/ex0/README.md#download-and-install-analytical-abap-cds-templates)

- [Exercise 1 - Introduction to Analytics in ABAP Cloud](https://github.com/SAP-samples/teched2023-DT187v/blob/main/exercises/ex1/README.md)
    - [Exercise 1.1 - Read: Scope of the Tutorial](https://github.com/SAP-samples/teched2023-DT187v/blob/main/exercises/ex1/README.md#exercise-11---read-scope-of-the-tutorial)
    - [Exercise 1.2 - Read: Embedded Analytics in ABAP Cloud](https://github.com/SAP-samples/teched2023-DT187v/blob/main/exercises/ex1/README.md#exercise-12---read-embedded-analytics-in-abap-cloud)
    - [Exercise 1.3 - Hands-On: Use the FLIGHT Data Model for Analytics](https://github.com/SAP-samples/teched2023-DT187v/blob/main/exercises/ex1/README.md#exercise-13---hands-on-use-the-flight-data-model-for-analytics)
    - [Exercise 1 - Summary](https://github.com/SAP-samples/teched2023-DT187v/blob/main/exercises/ex1/README.md#summary)

- [Exercise 2 - Build a Cube with Dimensions](https://github.com/SAP-samples/teched2023-DT187v/blob/main/exercises/ex2/README.md)
    - [Exercise 2.1 - Build the basic Cube](https://github.com/SAP-samples/teched2023-DT187v/blob/main/exercises/ex2/README.md#exercise-21---build-the-basic-cube)
    - [Exercise 2.2 - Prepare the Measures](https://github.com/SAP-samples/teched2023-DT187v/blob/main/exercises/ex2/README.md#exercise-22---prepare-the-measures)
    - [Exercise 2.3 - Prepare the Dimensions](https://github.com/SAP-samples/teched2023-DT187v/blob/main/exercises/ex2/README.md#exercise-23---prepare-the-dimensions)
    - [Exercise 2.4 - Use the Dimensions](https://github.com/SAP-samples/teched2023-DT187v/blob/main/exercises/ex2/README.md#exercise-24---use-the-dimensions)
    - [Exercise 2.5 - Optional - Add and use additional Dimensions](https://github.com/SAP-samples/teched2023-DT187v/blob/main/exercises/ex2/README.md#exercise-25---optional-add-and-use-additional-dimensions)
    - [Exercise 2 - Summary](https://github.com/SAP-samples/teched2023-DT187v/blob/main/exercises/ex2/README.md#summary)

- [Exercise 3 - Build the analytical Query](https://github.com/SAP-samples/teched2023-DT187v/blob/main/exercises/ex3/README.MD)
    - [Exercise 3.1 - Build the initial query](https://github.com/SAP-samples/teched2023-DT187v/blob/main/exercises/ex3/README.MD#exercise-31---build-the-initial-query)
    - [Exercise 3.2 - Prepare the Measures](https://github.com/SAP-samples/teched2023-DT187v/blob/main/exercises/ex3/README.MD#exercise-32---prepare-the-measures)
    - [Exercise 3.3 - Working Multi-Dimensional](https://github.com/SAP-samples/teched2023-DT187v/blob/main/exercises/ex3/README.MD#exercise-33---working-with-the-multi-dimensional-analysis)
    - [Exercise 3 - Summary](https://github.com/SAP-samples/teched2023-DT187v/blob/main/exercises/ex3/README.MD#summary)


**IMPORTANT**

## Contributing
Please read the [CONTRIBUTING.md](./CONTRIBUTING.md) to understand the contribution guidelines.

## Code of Conduct
Please read the [SAP Open Source Code of Conduct](https://github.com/SAP-samples/.github/blob/main/CODE_OF_CONDUCT.md).

## How to obtain support
Support for the content in this repository is available during the respective [Jump-Start Session: Build Analytical Applications with ABAP Cloud](https://go3.events.sap.com/sapteched/virtual/2023/reg/flow/sap/sapteched23/catalog/page/catalog/session/1693019886304001sNN4) at Friday, November 3, 2023, 7pm CET, for which this content has been designed.<br>
Otherwise, you may request support via the [Issues](../../issues) tab.<br>
A [SAP Help page for Embedded Analytics in ABAP Cloud](https://help.sap.com/docs/abap-cloud/abap-analytics/abap-analytics) is available as well. 

## License
Copyright (c) 2023 SAP SE or an SAP affiliate company. All rights reserved. This project is licensed under the Apache Software License, version 2.0 except as noted otherwise in the [LICENSE](LICENSES/Apache-2.0.txt) file.
