# geo-me

# Description
The Geo-me service performs self-geolocation of a platform (the target) using a set of observations from the platform to a predefined set of landmarks (the asset(s)). The service provides a re-sectioning function commonly used in overland navigation scenarios in the absence of global positioning system (GPS) systems. This service is capable of fusing any combination of angles to or ranges from known landmarks (the assets) and can handle erroneous measurements. Output of the service is a geolocation estimation latitude and longitude, a circular error of probability (CEP) provided as elliptical (elp) parameters (length, width and rotation). The service also provides a formatted kml file containing the computing scenario and result (this kml may be plotted on external geospatial interface systems) such as Google Earth.

# Inputs
Inputs are provided in the following json format:
```
{
    "observation": [
        {
            "assetId": "<string>",
            "meas": <double - range in [metres] or angle in [radians] according to the type>,          
            "id": <int>,
            "type": "<'range' or 'aoa'>",
            "lat": <double - latitude in decimal degrees -90 to 90>,
            "lon": <double - latitude in decimal degrees -180 to 180>
        },
        ...
    ],
    "target": {
        "name": "<string>",
        "id": "<string>"
    },
    "provide_kml_out": <boolean, optional, default: false>
}
```
A valid input should be in the JSON format, contain a target name and Id and at least two observations. A result will only be computed for observations which are generally agreeable in their location estimation. In other words, poor observations will likely result in an inaccurate or invalid result.

An optional simulation tool is provided for ease of validation - the tool generates a set of observations in the required format for any arbitrary geometry of platforms/targets and landmarks/assets. See: https://github.com/tgotech/geo_me_simulator

SingularityNET Geo-me User Interface can receive a link to a valid json url, or receive a custom formatted json. 
  
# Outputs
Outputs are returned in the following json format:
```
{
	"lat": <double - latitude [decimal degrees]>,
	"lon": <double - longitude [decimal degrees]>,
	"elp_long": <double - [metres], ellipse long axis>,
	"elp_short": <double - [metres], ellipse short axis>,
	"elp_rot": <double - [radians]>, ellipse rotation,
	"residual": <double>
	"status_svc": "<string - 'ok' or 'hung'>",
	"statusMessage_svc": "<string, returned only if status is not 'ok'>",
	"kml_out": <string>
}
```
'status_svc' may produce a 'hung' status where the provided observations do not enable the system to converge on a estimate, the observations are likely to contradict each other and the result is likely to be innacurate or invalid.
'kml_out' is provided as an example to quickly inspect and interpret the results as a map layer in an external geospatial interface system. The format and style of the map layer are non-configurable, however a custom layer can be produced by the client using the input data (observations & descriptions) and output data (location estimation & CEP parameters).

SingularityNET Geo-me User Interface produces a text based json output containing the geolocation result, and if the request included kml generation, a link to download or the kml for viewing in external geospatial interface systems.

# Support
email: support@tgo.tech
