# **K-Index Calculator**
==================

A module for calculation of the geomagnetic K-Index from horizontal geomagnetic components.

This module uses the Finnish Meteorological Institute (FMI) method, details of which can be found [here](http://swans.meteo.be/sites/default/files/documentation/TN-RMI-2010-01_K-LOGIC.pdf):

In a nutshell, this module takes horizontal geomagnetic components like this:
![alt text][Horiz]
[Horiz]: https://cloud.githubusercontent.com/assets/20742138/17296858/1a7f54c2-57fb-11e6-956b-c98714c9a6aa.png "Geomagnetic Field on St. Patrick's Day 2015"

... and calculates the [K-Index](https://en.wikipedia.org/wiki/K-index):
![alt text][KFin]
[KFin]: https://cloud.githubusercontent.com/assets/20742138/17296866/20c566e6-57fb-11e6-9d18-c33fa18b53c2.png "K-Index on St. Patrick's Day 2015"

## **Quick Code Example**
Assuming you have 4 days of geomagnetic data in the following format:

Times = array of datetime objects in second bins.

Bx, By, Bz = arrays of geomagnetic data in second bins.

First, convert the Times array into second floats:

`>>> Times_float = Time2Float(Times)`

Now convert all of the data to minute bins:

`>>> minute_time, minute_bx, minute_by, minute_bz = MinuteBin(Times_float, Bx, By, Bz)`

To get the K-Index for these 4 days:

`>>> k_index, k_time, order = KIndexSuperCalc(minute_time, minute_bx, minute_by, n)`

where n is the maximum threshold for a K9 event (dependent on latitude).

**NOTE**: The first few values in the array k_index are likely to be inaccurate.
This is because of the way the K-Index is calculated. It is safest to dismiss the first 8
calculated K-Index values.

To plot the K-Index:

`>>> KIndexPlotter(k_index[8:], k_time[8:], m)`

where m is the figure number.



##**Installation**
To install, type

`pip install k_index_calculator`

##**Dependencies**
datetime

numpy

scipy

matplotlib


##**Author**
Written by Sean Blake in Trinity College Dublin, 2014-2016

Email: blakese@tcd.ie

GITHUB: https://github.com/TerminusEst

Uses the MIT license.


##**FMI Method**
Assuming you have 4 days of geomagnetic data in the following format:

minute_time = array of datetime objects in minute bins.

minute_bx, minute_by, minute_bz = arrays of geomagnetic data in minute bins.

**Step 1**

For each 3-hour block in the horizontal geomagnetic components, the max variation is found. This is then compared to the following table to get an initial K-index:

| K-Index Value | nt Variation Range |
|:-------------:|:-------------:|
| 0             | (0-5)*(n/500)    |
| 2             | (10-20)*(n/500)|
| 3             | (20-40)*(n/500)|
| 4             | (40-70)*(n/500)|
| 5             | (70-120)*(n/500)|
| 6             | (120-200)*(n/500)|
| 7             | (200-330)*(n/500)|
| 8             | (330-500)*(n/500)|
| 9             | (500+)*(n/500)|

where **n** is the maximum threshold for a K9 event (this is dependent on geomagnetic latitude). This is calculated using the **KIndex** function.

**Step 2**

An estimation for the solar quiet or solar regular curve is calculated. This is calculated using the **FMISmooth** function, which is dependent on the preliminary K-Index.

**Step 3**
This solar quiet estimation is smoothed (**SrSmooth**), and subtracted from both horizontal components (**Subtracted**).
![alt text][SrCurve]
[SrCurve]: https://cloud.githubusercontent.com/assets/20742138/17298215/efce2c3e-5800-11e6-85ce-29aba5c144af.png "Bx Component and Sr Curve"

**Step 3**
Steps 1-2 are repeated twice more with the reduced data to get a second and then final K-Index.

All of these steps can be combined in one call of **KIndexSuperCalc**.
##Functions List


####Main Functions
KIndexSuperCalc
- Calculates the K-index using the FMI method.

KIndexPlotter
- Plots K-Index values in a nice Bar Plot


####Secondary Functions

Time2Float
- Converts datetime object or array of datetime objects to floats.

Float2Time
- Converts float or array of floats to datetime objects.

MinuteBin
- Bin second data into minutes.

KIndex
- Calculate K-Index according to the FMI Method.

FMISmooth
- Calculates the Solar Regular (Sr) curve according to the FMI Method.

SrSmooth
- Smooths the solar regular curves.

Subtracted
- Subtracts the smoothed solar regular curves from the minute binned data.

KIndexBarColor
-Colours K-index bar plot so it looks nice.
