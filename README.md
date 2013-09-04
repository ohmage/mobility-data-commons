# Mobility Data Commons

This is a repository for publicly available anonymized and pseudo-anonymized mobility data from the ohmage Mobility 
Android app ([source](https://github.com/ohmage/mobility-phone), [app](https://play.google.com/store/apps/details?id=org.ohmage.mobility)). 
The Mobility app collects a data point every minute or every five minutes, depending on user preference. The data is available 
as JSON or CSV. For CSV, the data includes a header row and is followed by rows containing the actual data. There is an example [here](https://github.com/ohmage/mobility-data-commons/blob/master/example/single-record-mobility.csv). 
For JSON, each point is represented by a JSON object that looks like this.

    {
       "id":72247796,
       "uid":"11c8d0da-abe0-432a-b6db-7f37a7928aae",
       "time":1377206820239,
       "time_offset":-14400000,
       "time_adjusted":1377192420239,
       "timezone":"America/New_York",
       "location_timestamp":"2013-08-22T17:27:00.239-04:00",
       "latitude":42.4377468,
       "longtiude":-76.4641825,
       "location_accuracy":36.00299835205078,
       "location:provider":"network",
       "wifi_data":{
          "scan":[
             {
                "ssid":"00:15:6d:9e:d7:d8",
                "strength":-71
             }
          ],
          "time":1377206753497,
          "timezone":"America/New_York"
       },
       "speed":0,
       "accel_data":[
          {
             "z":-3.6320040225982666,
             "y":9.008782386779785,
             "x":0.558447539806366
          }       
       ],
       "mode":"still"
    }
    
## Element Dictionary

<table>
<tr><td>id</td><td>A unique integer identifier for this object given its presence in a list of other mobility data points.</td></tr>
<tr><td>uuid</td><td>As above, but it is a UUID.</td></tr>
<tr><td>time</td><td>The standard Unix UTC milliseconds from January 1, 1970 up to the time this point was created.</td></tr>
<tr><td>time_offset</td><td>The number of milliseconds to convert time to the timezone where the point was collected.</td></tr>
<tr><td>timezone</td><td>The human-readable "zoneinfo" timezone.</td></tr>
<tr><td>location_timestamp</td><td>The timestamp at which the location was determined, which may be different than the time the point was created.</td></tr>
<tr><td>latitude and longitude</td><td>The location of the user at the time of location_timestamp.</td></tr>
<tr><td>location_accuracy</td><td>From the Android developer docs: 

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
    
<tr><td>location_provider</td><td>The system entity that supplied the location. The possible values are 
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

======

For filtered mobility data, the location data (including timezone) is not present and the Wi-Fi access points are converted into UUIDs.

    {
       "id":72247796,
       "uid":"11c8d0da-abe0-432a-b6db-7f37a7928aae",
       "time":1377206820239,
       "wifi_data":{
          "scan":[
             {
                "ssid":"11c8d0da-abe0-432a-b6db-7f37a7928aad",
                "strength":-71
             }
          ],
          "time":1377206753497,
          "timezone":"America/New_York"
       },
       "speed":0,
       "accel_data":[
          {
             "z":-3.6320040225982666,
             "y":9.008782386779785,
             "x":0.558447539806366
          }       
       ],
       "mode":"still"
    }

