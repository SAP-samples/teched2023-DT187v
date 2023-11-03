# Exercise 3 - Build an analytical Query

Now that we have built the analytical ***cube*** that defines the capabilities of our analytical model, lets formulate a ***query*** to that model.<br>
The query defines more or less the initial layout of the table (the table is kind of a pivot table) that will be displayed in your analytical application. 

The analytical query view is a `transient view entity` with the `provider contract analytical_query`.

## Exercise 3.1 - Build the initial Query

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

**Step 2)** Fix the errors by removing the 'key' keyword. A query does not need them. Unfortunately this cannot be noted in templates.<br>

<details><summary>Hint: Your code should now look like this</summary><p>

```abap

@AccessControl.authorizationCheck: #NOT_ALLOWED
@EndUserText.label: 'Flight Query'

define transient view entity ZDT187v_[YourInitials]_Flight_Query
provider contract analytical_query
as projection on ZDT187v_[YourInitials]_Flight_Cube {
 AirlineID,
 ConnectionID,
 FlightDate,
 PlaneType,
 AirportFromId,
 AirportToId,
 YearQuarter,
 CalendarYear,
 Price,
 CurrencyCode,
 MaximumSeats,
 OccupiedSeats,
 TotalFlights,
 DistanceUnit,
 Distance,
 /* Associations */
 _Airline,
 _AirportFrom,
 _AirportTo,
 _Connection,
 _FlightDate
}

```
</p></details>

**Step 3)** Define the layout by assigning the axis as well as the existence of totals for the dimension.<br>
For the axis, the annotation `@AnalyticsDetails.query.axis` is used.<br>
For the totals, we use `@AnalyticsDetails.query.totals`.<br>
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

**Step 2)** Add a new (calculated) measure to the query by calculating the occupation rate as a percentage using the MaximumSeats and OccupiedSeats fields.<br>
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
          @Semantics.amount.currencyCode: 'CurrencyCode'
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

**Step 6)** Format & activate your analytical query view.

<details><summary>Hint: Your query should now look like this</summary><p>

```abap

@AccessControl.authorizationCheck: #NOT_ALLOWED
@EndUserText.label: 'Flight Query'

define transient view entity ZDT187v_[YourInitials]_Flight_Query
  provider contract analytical_query
  as projection on ZDT187v_[YourInitials]_Flight_Cube
{
          /* Dimensions */

          @AnalyticsDetails.query.axis: #ROWS
          @AnalyticsDetails.query.totals: #HIDE
          AirlineID,

          @AnalyticsDetails.query.axis: #ROWS
          @AnalyticsDetails.query.totals: #HIDE
          ConnectionID,

          @AnalyticsDetails.query.axis: #ROWS
          @AnalyticsDetails.query.totals: #HIDE
          FlightDate,

          @AnalyticsDetails.query.axis: #FREE
          @AnalyticsDetails.query.totals: #HIDE
          PlaneType,

          @AnalyticsDetails.query.axis: #ROWS
          @AnalyticsDetails.query.totals: #SHOW
          AirportFromId,

          @AnalyticsDetails.query.axis: #FREE
          @AnalyticsDetails.query.totals: #HIDE
          AirportToId,

          @AnalyticsDetails.query.axis: #FREE
          @AnalyticsDetails.query.totals: #HIDE
          YearQuarter,

          @AnalyticsDetails.query.axis: #FREE
          @AnalyticsDetails.query.totals: #SHOW
          CalendarYear,

          /* Measures */

          @AnalyticsDetails.query.axis: #COLUMNS
          @Semantics.amount.currencyCode: 'GlobalCurrency'
          @Aggregation.default: #FORMULA
          currency_conversion (
            amount             => curr_to_decfloat_amount( Price ),
            source_currency    => CurrencyCode,
            target_currency    => cast ( 'EUR'      as abap.cuky( 5 ) ) ,
            exchange_rate_date => cast ( '20230101' as abap.dats),
            exchange_rate_type => 'M'
          )                                                          as Price,
  virtual GlobalCurrency : abap.cuky( 5 ),
          CurrencyCode,

          @AnalyticsDetails.query.axis: #COLUMNS
          MaximumSeats,

          @AnalyticsDetails.query.axis: #COLUMNS
          OccupiedSeats,

          @AnalyticsDetails.query.axis: #COLUMNS
          @EndUserText.label: 'Occupation Rate'
          @Aggregation.default: #FORMULA
          @Semantics.quantity.unitOfMeasure: 'unitPercent'
          $projection.occupiedseats / $projection.maximumseats * 100 as OccupationRate,
          cast( '%' as abap.unit )                                   as unitPercent,

          @AnalyticsDetails.query.axis: #COLUMNS
          TotalFlights,

          @AnalyticsDetails.query.axis: #COLUMNS
          Distance,
          DistanceUnit,

          /* Associations */
          _Airline,
          _AirportFrom,
          _AirportTo,
          _Connection,
          _FlightDate
}

```
</p></details>

**Step 7)** Preview the query by right-clicking the query in the explorer and selecting 'Preview'.<br>
Be patient. It's just a free trial system ;-)

## Exercise 3.3 - Working with the Multi-Dimensional Analysis

**Step 1)** Preview the query with the multi-dimensional analysis.<br>
To make use of the new multi-dimensional analysis based preview that we introduced to the trial systems, proceed as follows:
- open the regular preview (opens in the browser)
- now modify the URL in the following way:<br>
  - change the semantic oaction from 'AnalyticQuery-preview' to 'AnalyticQuery-previewInDeveloperMode'<br>
  - remove the '2C' after query=
  - completely remove the &cdsName=... part at the end
- the result should look like this<br>
  https://[...].hana.ondemand.com/sap/bc/ui2/flp?[...]#AnalyticQuery-preview?query=ZDT187V_[YourInitials]_FLIGHT_QUERY
- now press enter to load the new URL
- you may have to press F5 to reload in case of an error
- press 'go' to run the query
- check if the result of the layout that you defined in the query meets your expectations

<br>![](/exercises/ex3/images/01-MultiDimensionalPreview.png)

**Step 2)** Try changing the layout in the app directly:<br>
- Open the layout side pannel (second icon from top in the right side bar)
- Remove all attributes except the Airline from the rows
- Remove all measures except for the occupation rate from the measures structure
- Move the Departure Airport to the Columns
- etc.

**Step 3)** Try changing the display properties:<br>
- Right click the Departure Airport in the UI
- Select Dimension -> Properties
- Switch the Desciption on
- Display Totals
- Change the sorting
- etc.

**Step 4)** Add display attributes:<br>
- In the sidebar, expand the Flight Date and
- select Calendar Quarter

**Step 5)** Observe the currency conversion behaviour:<br>
- Notice the blue warning on the top left of the table
- Click it and see that conversion rates are missing
- Expand the measure structure and de-select all measures except for the price
- Empty the rows an only take the Flight Date into the rows
- See that some prices cannot be aggregated as they contain different currencies and no conversion is available<br>
  These issues are indicated with an '*'. Some dates have flights only in one currency. They will be displayed.
- Now exchange the Flight Date with the airline and see that the aggregation happens without conversion in the local currency.

**Step 6)** Play ;-)

**Step 7)** Also try modifying the layout in the query view and observe the results in the UI (re-publish, re-load)


## Summary

You now got to know common analytical modelling practices and learned the most important analytical terms.
You have an impression on what 'multi-dimensional' analytics means and how analytical UIs behave.

### What's next?

You are ready to learn more about embedded analytics for example in:
- The [SAP Help on ABAP Cloud Analytics](https://help.sap.com/docs/abap-cloud/abap-analytics/abap-analytics)
- The [Embedded Analytics Overview - Blog Post](https://blogs.sap.com/2022/11/30/embedded-analytics-with-abap-cloud-a-brief-overview-part-1/) by [Fabian Fellhauer](https://people.sap.com/fabianfellhauer)

Further links can be found in the [Jump-Start Session slides](/exercises/ex3/images/231018_Embedded-Analytics-for-TechEd_Ext.pdf).

Feel free to use this tutorial as a basis and explore further modeling practices within the trial system. What about e.g. implementing a hierarchy?

You may also want to check the transactional side of the story? Then head to the session [Use ABAP Cloud for developer extensibility](https://github.com/SAP-samples/teched2023-DT182v) from [Andrè Fischer](https://people.sap.com/andre.fischer).

The [slides of the Jump-Start Session](/exercises/ex3/images/231018_Embedded-Analytics-for-TechEd_Ext.pdf) are available in this GitHub repository as well.

