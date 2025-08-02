# CST8917 - Lab 4: Real-Time Trip Event Processing with Azure Logic App

## Overview

This lab implements a real-time trip monitoring system using Azure Event Hub, Logic App, Azure Function, and Microsoft Teams. When trip event data is sent to Event Hub, the Logic App is triggered, processes the data through an Azure Function, and posts categorized alerts to a Teams chat using Adaptive Cards.

## Architecture and Logic App Steps

1. The Logic App uses an Event Hub trigger with batch mode enabled.
2. It forwards the batch data to an Azure Function named `analyze_trip`.
3. The Function analyzes each trip and returns classification details including:
   - `isInteresting`: Boolean flag indicating noteworthy trips
   - `insights`: Optional array of warning tags like `SuspiciousVendorActivity`
4. The Logic App iterates through the Function results using `For each`.
5. Based on the result:
   - If `isInteresting` is `false`, it posts a “no issue” card.
   - If `isInteresting` is `true` and insights include `SuspiciousVendorActivity`, it posts a “suspicious” card.
   - Otherwise, it posts a generic “interesting” card.

## Azure Function Description

The Function `analyze_trip` is triggered by HTTP POST and receives a batch of trip events. It applies simple logic to classify each trip and returns enriched results used by Logic App for conditional processing.

## Example Input

```json
{
  "vendorID": "V123",
  "tripDistance": 5.0,
  "passengerCount": 2,
  "paymentType": "Cash"
}
```

## Example Output from Function

```json
[
  {
    "vendorID": "V123",
    "tripDistance": 5.0,
    "passengerCount": 2,
    "paymentType": "Cash",
    "isInteresting": false,
    "summary": "No issues detected"
  }
]
```
## Demo Video

Watch the demonstration of the system in action:  
[**[YouTube Link Here]**](https://youtu.be/Dm_wxGOaUHc)
