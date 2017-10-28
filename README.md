How much time does it actually cost a driver to pit during a race?
 
Not the 20 to 30 seconds it takes for a driver to enter and exit the pitlane.  But the seconds that are lost during the **in-lap** and **out-lap**.  This number (18 seconds? 20 seconds?) is often brought up when discussing the race strategy for # of pit stops.  But I haven't seen a list of costs that covers all the circuits on the schedule.  So, why don't we run some calculations?

This isn't a super-rigorous look at this topic; getting the numbers perfect down to tenths isn't going to happen.  More precision would require taking a number of other things into consideration.  Tire degredation, fuel load, track conditions, car characteristics, the type of tires on the car before and after the pit, pit-crew performance, etc...  All play a part in contributing to the time lost due to a pit.

This little experiment is just running a simple query on the [ergast DB](http://ergast.com/mrd), removing the easy outliers, and then throwing all the data points into some charts.  The numbers >should< be good enough to make general observations about each track.

The analysis starts with the 2011 season.  So, changes like Silverstone's makeover for 2011 or Circuit Gilles Villeneuve's in 2002 are avoided.  However, the reduction in pitlane speed (starting at Hungary in 2013) would affect the time costs.  So, the numbers are split into two parts... before and after this change.

![Total_time](/images/aggregate_pitstop_2011_to_2013_and_2013_to_2017_dry_vs_wet.png)

The cost of each pit stop is being calculated the same way you'd roughly eyeball them.

Assuming the pit stop occurred during lap 15, the times for the following laps would be used:

* Lap 14 (**Lap-Prior**) - Used as the last clean lap before pitting
* Lap 15 (**In-Lap**) - Lap when the car enters the pitlane
* Lap 16 (**Out-Lap**) - Lap when the car exits the pitlane
* Lap 17 (**Lap-Next**) - Used as the first clean lap after exiting the pit

The following calculations would then be run:

```
In-Lap cost  = In-Lap time  - Lap-Prior time
Out-Lap cost = Out-Lap time - Lap-Next time

Total Time Lost = In-Lap cost + Out-Lap cost
```

Pit events with a Total Time cost below 10 seconds or above 35 seconds are dropped.  These are scenarios where something unusual occurred.  Low values can point to things like a VSC or SC being out (think the early laps of Shanghai 2017 when all the cars were being led through the pitlane).  High values could indicate a damaged car trying to make its way back to the pit.  Out-lap penalties that are 'negative' were also thrown out.  

Tighter rules on what laps were kept or dropped could've been used.  But, the quartiles identified by the Box&Whiskers charts seem to do a decent job at throwing away outliers and identifying the cost of normal pit stops.  Most boxes cover a width of 2 to 3 seconds (So +/- 1 second in variance).  This seems like a reasonable range, especially given the fact that the performance of the pit crew's themselves will vary by a second or two also.   Occasional large whiskers seem to be linked to races that had bad/changing weather conditions.
 
Let's start with the costs earlier this decade, when the pitlane speed limit was 100 KPH.

## 2011 to 2013 Germany - The 100 KPH era

Most tracks during those years used the 100 kph limit (with only 3 exceptions).  Australia, Singapore and Monaco all used (and still use) a 60 kph limit during races.  So, it shouldn't be a surprise that the first two have the highest costs for pitting, with Australia at ~26 seconds and Singapore between 28 to 29 seconds. 

![2011 to 2013](/images/aggregate_pitstop_2011_to_2013_Germany_100kph.png)

Silverstone, Canada, Hockenheim, Spa and Istanbul were the tracks with the smallest penalties (~16-17 seconds), which is to be expected given their configurations.  Spa's pitlane starts around the bus stop and includes the hairpin at Turn 1.  Pitting at the Circuit Gilles Villeneuve means skipping turns 13, 14, 1, and 2.  All other tracks floated around the low 20's.

Looking specifically at the In-Lap costs, Silverstone's infamous 'shortcut' does show up in the numbers.  **In-laps** were usually a few seconds faster than an actual lap.  **In-Laps** at CotA were also routinely a little faster than actual laps.

Note that due to the mid-season change in rules, some tracks only held 2 races using the 100 kph limit during this period.  These tracks also often had one dry and one wet race.  So, variance for those tracks is understandably greater.

Moving on to the races that ran under the 80 kph speed limit.

## 2013 Hungary to 2017 Malaysia - The 80 KPH era

![2013 to 2017](/images/aggregate_pitstop_2013_hungary_and_above_80kph.png)

Due to the 20 kph reduction in pitlane speeds (100 kph to 80 kph), pit stop time costs were expected to go up.

The [official Mercedes-AMG page](https://www.mercedesamgf1.com/en/mercedes-amg-f1/silverstone-gb/) for Silverstone lists the exact length for the speed-limited section of the pit lane.

```
Length of speed-limited section: 376.4 meters

100 kph = 100,000 meters/hour = 1,666 meters/minute = 27.8 meters/second
80 kph = 80,000 meters/hour = 1,333 meters/minute = 22.2 meters/second

376.4 meters ÷ 27.8 meters/sec = 13.6 seconds @100 kph
376.4 meters ÷ 22.2 meters/sec = 16.9 seconds @80 kph
```

So, the 20 kph reduction was expected to a little over 3 seconds to the cost of pitting at Silverstone.

Compare that to what the box/whiskers charts are reporting:

* For the 100 kph era - **15.1** to **18.4** seconds, with a median time of **16.4** seconds
* For the 80 kph era  - **18.2** to **20.7** seconds, with a median time of **19.5** seconds

3.1 seconds vs 3.3 seconds.  Looks close enough.

The times for the tracks that were always limited to 60 kph (Singapore, Australia and Monaco) were fairly close in both sets.

Circuit | 2011 to 2013 | 2014 to 2017
:--- | :---: | :---:
Singapore |28.9|27.8
Australia |26.3|25.0
Monaco |22.7|21.7

<br>
Most pit costs go up the 2 to 3 seconds expected.  Sepang's seems a bit low though: **23.0** seconds vs **23.8** seconds.  But Sepang's numbers in the first set had a fairly wide range.  So that may explain that observation.  Spa's also a bit low: **17.2** vs **18.6**.  But all that could be just statistical noise.   Or possibly the changing characteristics of the cars, coupled with the specific make-ups of the tracks.

Silverstone's **in-laps** are still faster than normal laps, with the difference slightly mutely by a second or so.  The 20 kph reduction in speed also appears to have slowed down CotA's **in-laps**.  They're no longer routinely faster than regular laps.

A few observations to end this:

* Based on the In-Lap vs Out-Lap numbers for each track, you can tell which tracks have their finish lines before or after the pitlane.
  * If the In-Lap is **LOWER**, the finish line is before the garages
  * If the In-Lap is **HIGHER**, the finish line is after the garages
* The lap (In-Lap or Out-Lap) where the car actually stops in the pit appears to have the larger variance.  I'm *guessing* this is due to the variance in pit crew performance.  The times between the slowest and faster pit crews seems to hover around 2 seconds.
