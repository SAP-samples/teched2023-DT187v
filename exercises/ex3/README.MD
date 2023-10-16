# Exercise 3 - Build an analytical Query

Now that we have built the analytical CUBE that defines the capabilities of our analytical model, lets formulate a ***query*** to that model.<br>
The query defines more or less the initial layout of the table (the table is kind of a pivot table). 

The analytical query view is a `transient view entity' with the `provider contract analytical_query`.

## Exercise 3.1: Build the initial Query

**Step 1)** Create a Query View in your package.
- Right-click the package and choose<br>
- New -> Other Repository Object -> Core Data Services -> Data Definition<br>
- Click "Next"<br>
- Enter the name "ZDT187v_[YourInitials]_Flight_Query"<br>
- Give it the description "Flight Query"<br>
- As Referenced Object, use "ZDT187v_[YourInitials]_Flight_Cube"<br>
- Click "Next"<br>
- Use your request "ZDT187v [YourInitials] Flight Hands-On"<br>
- Click "Next"<br>
- Use the template "Define a View Entity for a Query" (Prerequisite is that you downloaded and installed the templates)
- Click "Finish"

<details><summary>Hint: Your code should now look like this</summary><p>

```abap

@AccessControl.authorizationCheck: #NOT_ALLOWED
@EndUserText.label: 'Flights Query'

define transient view entity ZDT187v_[YourInitials]_Flight_Query
provider contract analytical_query
as projection on ZDT187v_[YourInitials]_Flight_Cube {
 key AirlineID,
 key ConnectionID,
 key FlightDate,
 PlaneType,
 AirportFromId,
 AirportToId,
 FlightType,
 YearMonth,
 YearQuarter,
 CalendarYear,
 CurrencyCode,
 Price,
 MaximumSeats,
 OccupiedSeats,
 TotalFlights,
 DistanceUnit,
 Distance,
 /* Associations */
 _Airline,
 _Connection,
 _Currency,
 _FlightDate
}

```
</p></details>

**Step 2)** Fix the errors by removing the 'key' keyword. A query does not need them. Unfortunately this cannot be noted in templates.<br>

**Step 3)** Define the layout by assigning the axis as well as the existence of totals for the dimension.<br>
For the axis, the annotation `` is used.<br>
For the totals, we use ``.<br>
You can freely decide if you write the annotations either in a flat or on object style. In a standard layout, dimensions are on the row axis and the measures are on the column axis.<br>
If you do not assign a dimension to an axis, the measures will be aggregated according to the layout.<br>
Example: If you have the price in the columns and only add the airline to the rows (either by removing other dimensions or by assigning them to the `#FREE` axis), then the maximum price of all flights that are conducted by the airline will be the result of the aggregation (assuming the default aggregation of the price is set to `#MAX`).

<details><summary>Hint: See how it looks like</summary><p>

```abap

/* Object Style */
@AnalyticsDetails.query: {
    axis: #ROWS,
    totals: #HIDE
}
AirlineID

/* Flat Style */
@AnalyticsDetails.query.axis: #ROWS
@AnalyticsDetails.query.totals: #HIDE
AirlineID

```
</p></details>


## Exercise 2.2 - Prepare the Measures

## Summary

Now that you have analysed the FLIGHT data model, we know which elements are needed to start building our first analytical entity: The cube.
Continue to - [Exercise 2 - Build a Cube](../ex2/README.md)

