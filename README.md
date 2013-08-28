# Mobility Data Commons

This is a repository for publicly available anonymized and pseudo-anonymized mobility data from the ohmage Mobility 
Android app. The Mobility app collects a data point every minute or every five minutes, depending on user preference. Each
point is represented by a JSON object that looks like this.

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
<tr><td>location_accuracy</td><td>The distance in meters from the actual latitude-longitude coordinates with a ~90% confidence.</td></tr>
<tr><td>location_provider</td><td>The system entity that supplied the location. The value will be either "network" or "GPS".</td></tr>
<tr><td>wifi_data</td><td>A list of Wi-Fi access points and the time at which the access points were collected.</td></tr>
<tr><td>scan</td><td>A list of ssid-strength pairs. Strength is a negative integer that represents stronger signal strength as it approaches zero.</td></tr>
<tr><td>speed</td><td>The speed (not velocity) of the mobile device compared to the previous point.</td></tr>
<tr><td>accel_data</td><td>A list of accelerometer readings.</td></tr>
<tr><td>mode</td><td>The classified ambulatory mode based on our classification algorithm applied to this point.</td></tr>
</table>

======

For filtered mobility data, the location data is not present and the Wi-Fi access points are converted into UUIDs.

    {
       "id":72247796,
       "uid":"11c8d0da-abe0-432a-b6db-7f37a7928aae",
       "time":1377206820239,
       "time_offset":-14400000,
       "time_adjusted":1377192420239,
       "timezone":"America/New_York",
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

