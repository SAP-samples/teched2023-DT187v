# Exercise 1 - Introduction to Analytics in ABAP Cloud

In this exercise, you will understand what embedded analytics in ABAP Cloud is all about.
Also, you will get to know the FLIGHT data model you will be working with.

## Prerequisites

Make sure you have completed the prerequisites from [Exercise 0 - Get an ABAP Environment Trial](../ex0/README.md).

Even if you already have an ABAP Environment Trial, **please get a new instance** as the trial systems have been updated for the TechEd 2023 sessions!

## Read: Embedded Analytics in ABAP Cloud

TLDR: [Skip the reading and start the hands-on](#the-flight-data-model)

Let's understand how Analytics is embedded in ABAP Cloud. 
<br>![](/exercises/ex1/images/01-EmbeddedAnalyticsInABAPCloud.png)
1. SAP HANAs column store is vital for analytical data processing
2. Analytical data modelling is done ABAP Cloud style via ABAP CDS entities
3. Service Exposure via the multi-dimensional InA protocol is included in ABAP Cloud
4. The developer tooling needed for Analytics is included in the ABAP development tools
5. Analytical Fiori UIs that are part of ABAP Cloud are in preparation
6. SAP Analytics Cloud "SAC" can be used as client with multi-dimensional capabilities

## Read: Scope of the Tutorial

TLDR: [Skip the reading and start the hands-on](#the-flight-data-model)

What you will do in the course of this Hands-On session is focussed on the lower level of this diagram:

<br>![](/exercises/ex1/images/02-HandsOnScope.png)

**ABAP CDS:** You will mainly build an analytical data model on top of the transactional model that consists of a multi-dimensional cube view, according analytical dimension views as well as an analytical query that reads from the cube.

**INA SERVICE:** The Service Exposure part can also be done in the trial systems, but we do not have multi-dimensional clients yet available as part of the trial experience, therefore this part of the tutorial is optional.

**CLIENTS:** To give an impression of how a multi-dimensional client works, we enabled the multi-dimensional preview on the query in the trial system for you. We will use this preview to visualise the model that you built in the same way as it would be possible with an analytical end uer application. We will also link to addition material on how to consume the query in SAP Analytics Cloud.


## Hands-On: Use the FLIGHT Data Model for Analytics

In the hands-on, we will use the SAP Flight Model. The mission will be to analyse the occupation rate of flights.
After completing these steps you will have an understanding about how you can use the FLIGHT model as the basis for an analytical data model.

**Step 1)**	Open ABAP Development Tools and open your trial project.

**Step 2)**	Use the "Open Development Object" button to open the /DMO/I_Flight CDS View. <details><summary>Hint</summary><p>![ABAP Development Tools](/exercises/ex1/images/03-ADTDemoFlight.png)</p></details>

**Step 3)** Analytics is about measuring, so we first need to decide what to measure. This figure is called a ***measure*** in analytical terms. It has to be numerical. Typical measures are amounts or quantities, but can also be counts of something. Have a look at the "/DMO/I_Flight" CDS View, find suitable measures & note them down.
<details><summary>Hint</summary><p>

 ```abap
 @AccessControl.authorizationCheck: #NOT_REQUIRED
 @EndUserText.label: 'Flight View - CDS Data Model'
 //...

 define view entity /DMO/I_Flight
  as select from /dmo/flight as Flight

 //...
 {
      //...

      /* A price is a typical measure */
      @Semantics.amount.currencyCode: 'CurrencyCode'
      Flight.price          as Price,

      //...

      /* Maximum Seats and Occupied Seats are numerical and will help calculating the occupation rate */
      Flight.seats_max      as MaximumSeats,
      Flight.seats_occupied as OccupiedSeats,

      //...
 }
```
</p></details>

**Step 4)** We need to put measures into a perspective. This perspective is called ***dimension*** in analytical terms.  Have a look at the "/DMO/I_Flight" CDS View, find suitable dimensions & note them down.
<details><summary>Hint</summary><p>

 ```abap
 @AccessControl.authorizationCheck: #NOT_REQUIRED
 @EndUserText.label: 'Flight View - CDS Data Model'
 //...

 define view entity /DMO/I_Flight
  as select from /dmo/flight as Flight

 //...
 {
  //...

  /* Key Fields are always dimensions */
  key Flight.carrier_id     as AirlineID,

  //...
  key Flight.connection_id  as ConnectionID,

  //...
  key Flight.flight_date    as FlightDate,

  //...

  /* Basically all fields that are no measures can be dimensions */
  Flight.plane_type_id  as PlaneType,

  //...
 }
```
</p></details>

## Summary

Now that you have analysed the FLIGHT data model, we know which elements are needed to start building our first analytical entity: The cube.
Continue to - [Exercise 2 - Build a Cube](../ex2/README.md)
