## How To Use
1. Sign up for Google Maps Distance Matrix API, Google Maps Geocoding API, and Google Maps JavaScript API 
2. Insert the embed script somewhere within the document `<head>` before the opening `<body>` tag
3. Insert the Org ID in the "data-org-id" attribute in the script tag
4. Include the `<div>` on the page for the widget you'd like to display

**For questions about implementation or to report bugs contact support@jellyfishhealth.com**

### Embed Script
```html
<script data-org-id="{YOUR ORG ID}" id='jfh-widgets' type='text/javascript'>
  (function() {
    function async_load(){
      var e = document.getElementById('jfh-widgets');
      var j = document.createElement('script');
      j.type = 'text/javascript';
      j.async = true;
      j.src = 'https://s3.amazonaws.com/jfh-widgets/1.1.0/jfh-widgets.js';
      e.parentNode.insertBefore(j, e);
    }
    (window.attachEvent ? window.attachEvent('onload', async_load) : window.addEventListener('load', async_load, false));
  })();
</script>
```

### Embed Attributes
- **data-org-id (Required)**
  - Description: Determines the Org that the data should be pulled from
  - Type: **String**

- **data-google-api-key (Required for map and distance calculation)**
  - Description: Adds the Google Maps API key
  - Type: **String**
  - Required APIs: Google Maps Distance Matrix API, Google Maps Geocoding API, Google Maps JavaScript API

- **data-embed-css**
  - Description: Enables/Disables the css from being embedded in the page
  - Type: **boolean**
  - Default: **true**

- **data-callback-function**
  - Description: Enables a callback function to run after the embed script runs
  - Type: **String**
  - Default: **None**

- **data-wait-min-text**
  - Description: Sets the text to show when the if the minimum threshold time is not met
  - Type: **String**
  - Default: **None**

- **data-wait-min-threshold**
  - Description: Sets the minimum time **in minutes** to show the the threshold text
  - Type: **int**
  - Default: **None**

### URL Attributes for Testing with different environments
To test with different environments you can set the service urls to the environment you want to test against.

- **data-org-service-url**
  - Description: Sets the org service url
  - Type: **String**
  - Default: **Production Service URL**

- **data-getinline-url**
  - Description: Sets the get in line button url
  - Type: **String**
  - Default: **Production URL**
  - Note: Can be used to point to self scheduling

- **data-access-service-url**
  - Description: Sets the access service url
  - Type: **String**
  - Default: **Production Service URL**

- **data-stylesheet-url**
  - Description: Sets the stylesheet url
  - Type: **String**
  - Default: **Production Stylesheet URL**

## Map with Org Units
```html
<div id="jfh-org-unit-map"></div>
```
### Widget Attributes
- **data-map-zoom**
  - Description: Sets the zoom level of the map
  - Type: **int**
  - Default: **8**

- **data-update-location**
  - Description: Enables/Disables the widget from updating on location change
  - Type: **boolean**
  - Default: **false**

- **data-init-lat**
  - Description: Sets the initial latitude value
  - Type: **int**
  - Default: **None**

- **data-init-lng**
  - Description: Sets the initial longitude value
  - Type: **int**
  - Default: **None**

- **data-button-text**
  - Description: Allows setting custom text for the Get In Line button
  - Type: **string**
  - Default: **Get In Line Now**

## Org Unit Widget
```html
<div class="jfh-org-unit-widget" data-org-unit-id="{ORG UNIT ID}"></div>
```
### Widget Attributes
- **data-org-unit-id (Required if data-update-location is false)**
  - Type: **string**

- **data-update-location (Required if data-org-unit-id is not specified)**
  - Description: Enables/Disables the widget from updating on location change
  - Type: **boolean**
  - Default: **false**

- **data-init-lat**
  - Description: Sets the initial latitude value
  - Type: **int**
  - Default: **None**

- **data-init-lng**
  - Description: Sets the initial longitude value
  - Type: **int**

- **data-show-name**
  - Description: Hides/Shows the org unit name
  - Type: **boolean**
  - Default: **true**

- **data-show-phone**
  - Description: Hides/Shows the org unit phone number
  - Type: **boolean**
  - Default: **true**

- **data-show-address**
  - Description: Hides/Shows the org unit address
  - Type: **boolean**
  - Default: **true**

- **data-show-wait**
  - Description: Hides/Shows the org unit estimated wait time
  - Type: **boolean**
  - Default: **true**

- **data-show-hours**
  - Description: Hides/Shows the org unit hours
  - Type: **boolean**
  - Default: **true**

- **data-show-button**
  - Description: Hides/Shows the org unit "get in line now" button
  - Type: **boolean**
  - Default: **true**

- **data-button-text**
  - Description: Allows setting custom text for the Get In Line button
  - Type: **string**
  - Default: **Get In Line Now**

## Updating widget locations
To update the location of a widget follow these steps
1. Set the "data-update-location" attribute to "true"
2. call the `JFHWidgets.updateWidgets(lat, lng)` function

**Note:** To update the widgets on page load when you have the user's location, you will need to run `JFHWidgets.updateWidgets` using the callback function data attribute

**Example**
```html
<script async='true'>
function geolocate() {
  // Try HTML5 geolocation.
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(function(position) {
      var userPos = {
        lat: position.coords.latitude,
        lng: position.coords.longitude
      };
      JFHWidgets.updateWidgets(userPos.lat, userPos.lng)
    }, function() {
      handleLocationError(true);
    });
  } else {
    // Browser doesn't support Geolocation
    handleLocationError(false);
  }
  function handleLocationError(browserHasGeolocation) {
    let error = (browserHasGeolocation ?
                          'Error: The Geolocation service failed.' :
                          'Error: Your browser doesn\'t support geolocation.');
    console.log(error)
  }
}
</script>
```
