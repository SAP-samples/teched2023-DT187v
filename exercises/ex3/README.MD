# Exercise 3 - Build an analytical Query

Now that we have built the analytical CUBE that defines the capabilities of our analytical model, lets formulate a ***query*** to that model.<br>
The query defines more or less the initial layout of the table (the table is kind of a pivot table). 

The analytical query view is a `transient view entity` with the `provider contract analytical_query`.

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
In a standard layout, dimensions are on the row axis and the measures are on the column axis.<br>
If you assign a dimension to a different axis or remove it, the measures will be aggregated according to the layout.<br>
Example: If you have the price in the columns and only add the airline to the rows (either by removing other dimensions or by assigning them to the `#FREE` axis), then the maximum price of all flights that are conducted by the airline will be the result of the aggregation (assuming the default aggregation of the price is set to `#MAX`).

<details><summary>Hint: See how it looks like</summary><p>

```abap

/* You can freely decide if you write the annotations either in a flat or on object style */

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


## Exercise 3.2 - Prepare the Measures

Generally, all measures will always be (in a group) on the same axis. If you do not assign an axis at all, they will be on the column axis by default.
If you assign different axes to the measures, the first one will win. For readability reasons, we will add the axis annotations to all measures here.

**Step 1)** Add the `@AnalyticsDetails.query.axis: #COLUMNS` annotation to all measures.

**Step 2)** Calculate the occupation rate as a percentage and add it as new (calculated) measure to the query.<br>
Remember: Measures that need to be calculated after the aggregation have to be part of the query.

<details><summary>Hint: See how it looks like</summary><p>

```abap

          /* label for a better user experience */
          @EndUserText.label: 'Occupation Rate'
          /* as the aggrregation is set already for the fields that are used in the calculation, we leave it up to the engine to decide for our formula */
          @Aggregation.default: #FORMULA
          /* the calculation formula */
          $projection.occupiedseats / $projection.maximumseats * 100 as OccupationRate,

```
</p></details>

**Step 3)** Optional: To indicate, that the occupation rate is a percentage, add an according unit to it.<br>
This will need introducing the unit as a field & then referencing it from the calculated field.

<details><summary>Hint: See how it looks like</summary><p>

```abap

          /* introduce the unit */
          cast( '%' as abap.unit )                                   as unitPercent,
          /...
          /* reference the new unit */
          @Semantics.quantity.unitOfMeasure: 'unitPercent'
          $projection.occupiedseats / $projection.maximumseats * 100 as OccupationRate,

```
</p></details>

**Step 4)** Introduce a currency conversion for the price.<br>
As the price is listed in different currencies, it has to be converted so that it can be aggregated.

<details><summary>Hint: Currency Conversion</summary><p>

```abap

          @AnalyticsDetails.query.axis: #COLUMNS
          @Semantics.amount.currencyCode: 'EUR'
          @Aggregation.default: #FORMULA
          currency_conversion (
            amount             => curr_to_decfloat_amount( Price ),
            source_currency    => CurrencyCode,
            target_currency    => cast ( 'EUR'      as abap.cuky( 5 ) ) ,
            exchange_rate_date => cast ( '20230101' as abap.dats),
            exchange_rate_type => 'M'
          )                                                          as Price,

```
</p></details>

**Step 5)** Introduce a virtual unit for the price.<br>
In case the price cannot be converted e.g. due to missing conversion rates, this has to be considered by displaying the original currency and amount instead of the target currency.

<details><summary>Hint: Virtual unit</summary><p>

```abap

virtual GlobalCurrency : abap.cuky( 5 ),
/...
          @Semantics.amount.currencyCode: 'GlobalCurrency'
          /...
          currency_conversion (
             /...
          )                                                          as Price,

```
</p></details>

<details><summary>Hint: Your query should now look like this</summary><p>

```abap



```
</p></details>

**Step 6)** Preview the query by right-clicking the query in the explorer and selecting 'Preview'.

## Exercise 3.3 - Working with the Multi-Dimensional Analysis

**Step 1)** Preview the query with the multi-dimensional analysis.<br>
To make use of the new multi-dimensional analysis based preview that we introduced to the trial systems, proceed as follows:
- open the regular preview (opens in the browser)
- now modify the URL and change the semantic oaction from 'AnalyticQuery-preview' to 'AnalyticQuery-previewInDeveloperMode'
- set the value of 'query=2C...' to 'query=...' 
- now press enter to load the new URL



## Summary

Now that you have analysed the FLIGHT data model, we know which elements are needed to start building our first analytical entity: The cube.
Continue to - [Exercise 2 - Build a Cube](../ex2/README.md)

