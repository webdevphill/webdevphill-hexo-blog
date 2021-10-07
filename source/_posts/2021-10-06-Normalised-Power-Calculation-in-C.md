---
title: Normalised Power Calculation in C#
comments: true
date: 2021-10-06 15:16:24
tags:
- zwift
- fit file
- csharp c#
- normalized power
categories: Projects
---

## Normalised Power Calculation in C#

The beginning of my journey to fix FIT files that are missing Training Stress Score (TSS). 

*Just here for the code? Skip to the end...*

## Back Story

According to [DC Rainmakers](https://www.dcrainmaker.com) [article](https://www.dcrainmaker.com/2021/06/garmin-training-status-now-includes-zwift-trainerroad-the-sufferfest-and-tacx-app-workouts.html), FIT files automatically uploaded from [Zwift](https://zwift.com) to [Garmin Connect](https://connect.garmin.com) should get synced with your Garmin device and add to your Training Load.

However, this is not the case with my device (Garmin Fenix 5s) for whatever reason. 
On further investigation of the FIT files exported from Zwift, there was no Normalized Power, Intensity Factor or Training Stress Score; all things needed to calculate Training Load.

The simple fix for this is to record your indoor training activity on your Garmin device whilst Zwifting, however, it doesn't take into account things like elevation changes during your ride.
And being somewhat of a completist, the Zwift workouts that I have already uploaded to Garmin Connect have not added to my total training load and must be fixed!!

## Solution

Obviously the only solution is to implement my own solution very specific solution, because the short time I spent searching for an easy way to fix FIT files returned zero results.

Luckily the [Garmin FIT SDK](https://developer.garmin.com/fit/overview/) is available for developers and I have some C# experience (there are also examples for C, C++ and Java).
Which means I can cobble something together and stick it on the internets for those like myself looking for a way to update their FIT files with Training Load data.

So next step was to work out how to calculate Training Stress Score (TSS), the best article on the topic I found was from [slowtwitch.com](https://www.slowtwitch.com/Training/General_Physiology/Measuring_Power_and_Using_the_Data_302.html).
Turned out I needed to calculate Intensity Factor(IF) to calculate TSS and to calculate IF I needed to calculate Normalized Power(NP).

### Normalised Power
* T = time spent at that power level (minutes)
* P = power output (watts)

*NP = ( ( (T x P ^ 4) + ... + (T x P ^ 4) ) / 60 ) ^ (1.0 / 4)*

### Intensity Factor
* NP = Normalised Power (watts)
* FTP = Functional Threshold Power (watts)
* IF = Intensity Factor (%)

*IF = NP/FTP*

### Training Stress Score
* D = duration (hours)
* IF = Intensity Factor (% in decimal)
* TSS = Training Stress Score

*TSS = IF^2 x D x 100*

## Code
The code examples below both use the following example data:
*10 minutes at 145 watts*  
*20 minutes at 265 watts*  
*30 minutes at 175 watts*
*Normalized Power = 216 watts*

Inline calculation:
```csharp
double NormalisedPower = Math.Pow( 
    (
        (10 * Math.Pow(145, 4)) + 
        (20 * Math.Pow(265, 4)) + 
        (30 * Math.Pow(175, 4))
    ) / 60, 1.0 / 4);
```

Below is an example of the above as a function to process many input values.  
It requires a custom type for readability, however, you could just as easily use a tuple.
```csharp
// Custom Type
 public struct NormalisedPowerPair
{
    public double Time { get; }
    public double Power { get; }

    public NormalisedPowerPair(double time, double power) => (Time, Power) = (time, power);
}
```
```csharp
// Function to parse many values
static double NormalisedPower(NormalisedPowerPair[] normalisedPowerPairs)
{
    double normalisedPower = 0;
    double accumulatedValue = 0;

    foreach (NormalisedPowerPair item in normalisedPowerPairs)
    {
        accumulatedValue += item.Time * Math.Pow(item.Power, 4);
    }

    normalisedPower = Math.Pow(accumulatedValue / 60, 1.0 / 4);
    
    return normalisedPower;
}

```
```csharp
// Example use
NormalisedPowerPair[] npPairs = {
            new NormalisedPowerPair(10, 145),
            new NormalisedPowerPair(20, 265),
            new NormalisedPowerPair(30, 175)
        };

Console.WriteLine("Normalised Power = " + NormalisedPower(npPairs) + " watts");
```





