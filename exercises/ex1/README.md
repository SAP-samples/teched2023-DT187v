# Exercise 1 - Introduction to Analytics in ABAP Cloud

In this exercise, you will understand what embedded analytics in ABAP Cloud is all about.
Also, you will get to know the FLIGHT data model you will be working with.

## Prerequisites

Make sure you have completed the prerequisites from [Exercise 0 - Get an ABAP Environment Trial](../ex0/README.md).

Even if you already have an ABAP Environment Trial, make sure to get a new one.
The trial systgems have been updated for the TechEd 2023 sessions!

## Embedded Analytics in ABAP Cloud

Let's understand how Analytics is embedded in ABAP Cloud. 
<br>![](/exercises/ex1/images/01-EmbeddedAnalyticsInABAPCloud.png)
1. SAP HANAs column store is vital for analytical data processing
2. Analytical data modelling is done ABAP Cloud style via ABAP CDS entities
3. Service Exposure via the multi-dimensional InA protocol is included in ABAP Cloud
4. The developer tooling needed for Analytics is included in the ABAP development tools
5. Analytical Fiori UIs that are part of ABAP Cloud are in preparation
6. SAP Analytics Cloud "SAC" can be used as client with multi-dimensional capabilities

## Scope of the Tutorial

What you will do in the course of this Hands-On session is focussed on the lower level of this diagram:
<br>![](/exercises/ex1/images/02-HandsOnScope.png)
You will mainly build an analytical data model on top of the transactional model that consists of a multi-dimensional cube view, according analytical dimension views as well as an analytical query that reads from the cube.
The Service Exposure part can also be done in the trial systems, but unfortunately, we do not have multi-dimensional clients yet available as part of the trial experience.
That you anyhow get an impression of how a multi-dimensional client works, we enabled a multi-dimensional preview on the query in the trial system and will link you to additional material.
With the preview, you will be able to visualise the model that you built in the same way as it would be possible with an analytical end uer application.

## Analytical Basiscs




## The FLIGHT Data Model

After completing these steps you will have an understanding about how you can use the FLIGHT model as the basis for analytical data model according to the STAR SCHEMA.

1.	Open ABAP Development Tools and open your project.
2.	Use the "Open Development Object" button to open the "/DMO/I_Flight" CDS View.
<br>![](/exercises/ex1/images/03-ADTDemoFlight.png)
3.	Use the 
<br>![](/exercises/ex0/images/00_00_0010.png)

4.	Insert this code.
``` abap
 DATA(params) = request->get_form_fields(  ).
 READ TABLE params REFERENCE INTO DATA(param) WITH KEY name = 'cmd'.
  IF sy-subrc <> 0.
    response->set_status( i_code = 400
                     i_reason = 'Bad request').
    RETURN.
  ENDIF.
```

## Summary

Now that you have analysed the FLIGHT data model, you know which elements are needed to start building your first analytical entity: The cube.
Continue to - [Exercise 2 - Build a Cube](../ex2/README.md)
