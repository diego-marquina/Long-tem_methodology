# Documentation on how the long-term fundamentals model currently works
## Objective
The fundamentals team is responsible for producing the European long-term market forecast. The end goal is to produce hourly forecasts for most European markets 15 years out, or as far out as needed in order to cover the time horizons relevant to our stakeholder’s projects.
The workflow, described in more detail in the following sections, is summarised in the following chart:
## Current status
The following table shows the current coverage of our forecasts – the numbers in each cell represent the time horizon for each forecast.
|**Region**|**Day-ahead hourly forecast**|
|:------|:-----:|
|Austria|2022-26|
|Belgium|2022-26|
|Czechia|2022-26|
|Denmark|2022-26|
|France|2022-26|
|Germany|2022-26|
|Great Britain|2022-26|
|Netherlands|2022-26|
|Portugal|2022-26|
|Spain|2022-26|
|Switzerland|2022-26|
|Italy|In progress|

## Inputs
The following is a list of inputs that the team is responsible for estimating or sourcing from third parties and that feed into the dispatch model:

|**Input**|**Source**|**Analysis group**|
|:------|:-----|:-----|
|**Demand (fixed & flexible)**|Tesla, EQ|RazorShell.Optim.inputs.Base!0_WY-EQ!40|
|<ul><li>Residential & commercial</li></ul>|||
|<ul><li>Industrial</li></ul>|||
|<ul><li>Transport</li></ul>|||
|<ul><li>Demand-side response</li></ul>|||
|**Supply (unit-level)**|||
|<ul><li>Price responsive (eg CCGTs, coal, peakers, batteries, etc.)</li></ul>|||
|<ul><ul><li>Capacity</li></ul></ul>|EPSI for existing assets, then Shell's assumptions for the longer-term||
|<ul><ul><li>Availability</li></ul></ul>|REMIT for existing assets, then Shell's assumptions for the longer-term|<ul><li>W:\Power\RazorShell\Availability for short-term French nukes</li><li>[Long-term assumptions in curve 10676](https://marketdatadashboard.azurewebsites.net/curveAdmin/curveBuilderManager/10676)</li></ul>|
|<ul><ul><li>Efficiency</li></ul></ul>|EPSI for existing assets, then Shell's assumptions for the longer-term||
|<ul><ul><li>Constraints(eg start-up costs, ramp rates, min up/down times, max cycles)</li></ul></ul>|EPSI & Shell|RazorShell.Optim.inputs.Base!0.FuelPrice for start-up costs|
|<ul><ul><li>CHP/must run</li></ul></ul>|EPSI & Shell||
|<ul><li>Interconnectors</li></ul>|||
|<ul><ul><li>Capacity</li></ul></ul>|Mix of third parties and in-house assumptions||
|<ul><ul><li>Availability</li></ul></ul>|Mix of third parties and in-house assumptions|REMIT for existing assets, then Shell's assumptions for the longer-term|
|**Non-price responsive**|||
|<ul><li>Renewables</li></ul>|||
|<ul><ul><li>Capacity</li></ul></ul>|Mix of third parties and in-house assumptions|RazorShell.Capacity.inputs.Base_WY-EQ|
|<ul><ul><li>Load factors</li></ul></ul>|Mix of third parties and in-house assumptions|RazorShell.LoadFactors.inputs.Base_WY-EQ|
|<ul><ul><li>Price response</li></ul></ul>|In-house assumptions||
|**Commodities & other**|||
|<ul><li>Fuel prices</li></ul>|Markets, Aligne, KYOS|RazorShell.Optim.inputs.Base!0.FuelPrice|
|<ul><li>Carbon prices</li></ul>|Markets, Aligne, KYOS|RazorShell.Optim.inputs.Base!0.FuelPrice|
|<ul><li>FX rates</li></ul>|Markets, Aligne, KYOS|RazorShell.Optim.inputs.Base!0.FuelPrice|

## Modelling
To forecast the European power system, we use a model that optimises for the least cost dispatch of generating resources to meet demand in every hour. The optimisation window we work with is usually two weeks long.
For the long-term we have two different models:
- Deterministic – uses seasonal normal demand, wind, solar and hydro
- Weather years – uses demand and renewable generation based on 40+ weather years

We currently run only a central/reference scenario, but will add a high and low price scenario once the central/reference scenario is set up and running.

## Outputs
Outputs can be found in the [Market Data Dashboard - Power Model](https://marketdatadashboard.azurewebsites.net/powerModels/runner)
The model produces the following set of hourly outputs (or in the case of the weather-years model output distributions):
- Day-ahead power prices
- Emissions
- Generation
- Demand (flexible)
- Dump energy
- Pump energy
- Loading energy (ie battery charging)
- Unserved energy
- Interconnector flows
- Renewables response to price

The data will be available at the technology level.

Relevant stakeholders can view and download the data, be it directly in the  [Market Data Dashboard](https://marketdatadashboard.azurewebsites.net/powerModels/runner) or using the [API](https://systradingmarketdataapi.azurewebsites.net/index.html) or the [razorshell python API](https://github.com/sede-x/se-ee-mkta-razorshell-library).
