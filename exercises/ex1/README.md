# Exercise 1 - Introduction to Analytics in ABAP Cloud

In this exercise, you will understand what embedded analytics in ABAP Cloud is all about.
Also, you will get to know the FLIGHT data model you will be working with.

## Prerequisites

Make sure you have completed the prerequisites from [Exercise 0 - Get an ABAP Environment Trial](../ex0/README.md).

Even if you already have an ABAP Environment Trial, **please get a new instance** as the trial systems have been updated for the TechEd 2023 sessions!

## Exercise 1.1 - Read: Embedded Analytics in ABAP Cloud

TLDR: [Skip the reading and start the hands-on](#exercise-13---hands-on-use-the-flight-data-model-for-analytics)

Let's understand how Analytics is embedded in ABAP Cloud. 
<br>![](/exercises/ex1/images/01-EmbeddedAnalyticsInABAPCloud.png)
1. SAP HANAs column store is vital for analytical data processing
2. Analytical data modelling is done ABAP Cloud style via ABAP CDS entities
3. Service Exposure via the multi-dimensional InA protocol is included in ABAP Cloud
4. The developer tooling needed for Analytics is included in the ABAP development tools
5. Analytical Fiori UIs that are part of ABAP Cloud are in preparation
6. SAP Analytics Cloud "SAC" can be used as client with multi-dimensional capabilities

## Exercise 1.2 - Read: Scope of the Tutorial

TL;DR [Skip the reading and start the hands-on](#exercise-13---hands-on-use-the-flight-data-model-for-analytics)

What you will do in the course of this Hands-On session is focussed on the lower level of this diagram:

![](/exercises/ex1/images/02-HandsOnScope.png)

**ABAP CDS:** In this hands-on, you will mainly build an analytical data model on top of the transactional model that consists of a multi-dimensional cube view, according analytical dimension views as well as an analytical query that reads from the cube.

**INA SERVICE:** The Service Exposure part can theoretically also be done in the trial systems, but as we do not (yet) have multi-dimensional clients 
available as part of the trial experience, this part will not be covered in the hands-on.<br>
You may want to check the Tutorial [Develop and Consume Queries on SAP Analytics Cloud](https://developers.sap.com/tutorials/abap-environment-analytics.html) (starts with step 10) or the Devtoberfest 2023 Session [Introduction to Embedded Analytics in ABAP Cloud](https://www.youtube.com/watch?v=2dIqQNnYKjY&list=PLBoQ2iTAoalS9Urg3jcyVjGtxb15Gudfq) (starts at 28:30) on how to expose the query.

**CLIENTS:** As there are no multi-dimensional analytical end user clients available in trial, we enabled the multi-dimensional Fiori preview on the query in the trial system. We will use this preview to visualise the model. It works in the same way as an analytical end user application.<br>
To see, how the SAC client would look like, you can also check the [Tutorial](https://developers.sap.com/tutorials/abap-environment-analytics.html) (starts with step 16) or the [Devtoberfest Session](https://www.youtube.com/watch?v=2dIqQNnYKjY&list=PLBoQ2iTAoalS9Urg3jcyVjGtxb15Gudfq) (around minute 38).


## Exercise 1.3 - Hands-On: Use the FLIGHT Data Model for Analytics

In the hands-on, we will use the SAP Flight Model. The mission will be to analyse the occupation rate of flights.
After completing these steps you will have an understanding about how you can use the FLIGHT model as the basis for an analytical data model.

**Step 1)**	Open ABAP Development Tools and open your trial project.

**Step 2)**	Use the "Open Development Object" button to open the /DMO/I_Flight CDS View. <details><summary>Hint: How it looks like in ADT</summary><p>![ABAP Development Tools](/exercises/ex1/images/03-ADTDemoFlight.png)</p></details>

**Step 3)** Analytics is about measuring, so we first need to decide what to measure. This figure is called a ***measure*** in analytical terms. It has to be numerical. Typical measures are amounts or quantities, but can also be counts of something. Have a look at the "/DMO/I_Flight" CDS View, find suitable measures & note them down.
<details><summary>Solution</summary><p>

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
<details><summary>Solution</summary><p>

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
