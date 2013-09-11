# Mobility Data Commons

This is a repository for publicly available anonymized and pseudo-anonymized mobility data from the ohmage Mobility 
Android app ([source](https://github.com/ohmage/mobility-phone), [app](https://play.google.com/store/apps/details?id=org.ohmage.mobility)). 
The Mobility app collects a data point every minute or every five minutes, depending on user preference. 

The data is available as CSV exports. There is a pseudo-anonymized example [here](https://github.com/ohmage/mobility-data-commons/blob/master/example/single-record-mobility.csv) and 
a anonymized example [here](https://github.com/ohmage/mobility-data-commons/blob/master/example/single-record-mobility-filtered.csv).

The data can be found in the [data directory](https://github.com/ohmage/mobility-data-commons/tree/master/data) of this repo. Each directory 
represents one user's data. The data has been cleaned and is valid CSV and JSON, but irregularities may exist. Each directory contains
 gzipped tar archives that contain chunks of data for one to six month periods. Very gappy three month chunks have been removed. Each 
 archive is roughly 50 MB or less.

If you have questions, feel free to put in an issue. Otherwise, clone this repo and analyze away!

## Element Dictionary

<table>
<tr><td>id</td><td>A unique integer identifier for this object given its presence in a list of other mobility data points.</td></tr>
<tr><td>uuid</td><td>As above, but it is a UUID.</td></tr>
<tr><td>time</td><td>The standard Unix UTC milliseconds from January 1, 1970 up to the time this point was created.</td></tr>
<tr><td>time_offset</td><td>The number of milliseconds to convert time to the timezone where the point was collected.</td></tr>
<tr><td>timezone</td><td>The human-readable "zoneinfo" timezone. May be null.</td></tr>
<tr><td>location_timestamp</td><td>The timestamp at which the location was determined, which may be different than the time the point was created. May be null.</td></tr>
<tr><td>latitude and longitude</td><td>The location of the user at the time of location_timestamp. May be null.</td></tr>
<tr><td>location_accuracy</td><td>This value may be null, but if it isn't, the Android developer docs state the following. 

    <p>Get the estimated accuracy of this location, in meters.</p>
    
    <p>We define accuracy as the radius of 68% confidence. In other
    words, if you draw a circle centered at this location's
    latitude and longitude, and with a radius equal to the accuracy,
    then there is a 68% probability that the true location is inside
    the circle.</p>
    
    <p>In statistical terms, it is assumed that location errors
    are random with a normal distribution, so the 68% confidence circle
    represents one standard deviation. Note that in practice, location
    errors do not always follow such a simple distribution.</p>
    
    <p>This accuracy estimation is only concerned with horizontal
    accuracy, and does not indicate the accuracy of bearing,
    velocity or altitude if those are included in this Location.</p>

    <p>If this location does not have an accuracy, then 0.0 is returned.</p></td></tr>
    
<tr><td>location_provider</td><td>May be null. The system entity that supplied the location. The possible values are 
<ul>
<li>WifiGPSLocation:GPS - the GPS receiver.</li>
<li>WiFiGPSLocation:Cached - a cached GPS lock from the GPS receiver.</li>
<li>WiFiGPSLocation:Fake - a fake provider for testing. <tt>speed</tt> will always be 0.0 in this case.</li>
<li>WiFiGPSLocation:Network - the Wi-Fi Network scan provided the location.</li>
<li>WiFiGPSLocation:Network - the Wi-Fi Network scan provided the location, but the value returned is from a cache of Wi-Fi scans and possibly incorrect.</li>
</ul>
<p>You can check out the source code for WiFiGPSLocation <a href="https://github.com/ohmage/wi-fi-gps-location">here</a>. Note
that other values for location_provider are possible for this field, but the list above contains the values that are officially
supported.</p>
</td></tr>

<tr><td>wifi_data</td><td>A list of Wi-Fi access points and the time at which the access points were collected. A typical list size is between 5 and 10 points though it can be larger.</td></tr>
<tr><td>scan</td><td>A list of ssid-strength pairs. Strength is a negative integer that represents stronger signal strength as it approaches zero.</td></tr>
<tr><td>speed</td><td>The speed (not velocity) of the mobile device compared to the previous GPS reading. Can be zero if obtaining a GPS lock was unsuccessful.</td></tr>
<tr><td>accel_data</td><td>A list of accelerometer readings. The list is typically between 30 and 50 triaxial points.</td></tr>
<tr><td>mode</td><td>The classified ambulatory mode based on our classification algorithm applied to this point. The value
will be one of still, walk, run, drive, or error. The error mode occurs if the classifier cannot determine the mode based on the
data for the point.</td></tr>
</table> 
