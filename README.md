I am taking one example to write a JsWrapper for the cordova-plugin in Angular-2x, 4x, 5x and 6x
Cordova plugin name:- 'cordova-plugin-device'
How to add plugin:- 'cordova plugin add cordova-plugin-device' (https://cordova.apache.org/docs/en/latest/reference/cordova-plugin-device/)
Name of Wrapper File Name:- cordovaPluginWrapper.js

I am writting one annonomus function name is cordovaPluginWrapper(Any name what you prefer)
```javascript
var cordovaPluginWrapper = (function() {

    return {
        // getDeviceType- Name of the method which will be called from .ts file
        //deviceTypeCallback- is the call back, which will return data from the Plugin(either sucess or error).
        getDeviceType: function(deviceTypeCallback) {
            deviceTypeCallback(device); //device is the object which has all value related to device(Android and ios).
        }
    }
})(cordovaPluginWrapper || {});
```

- Add cordovaPluginWrapper.js file to index.html
```javascript
<script src="cordova.js"></script> //Simply add this line for compile time check. In run time file will be generated autometically.
<script src="cordovaPluginWrapper.js"></script>
```
- In Angular project declare cordovaPluginWrapper as global variable.
- In typings.d.ts:-
```javascript
declare var cordovaPluginWrapper;
```

- In .ts file call getDeviceType function of cordovaPluginWrapper.
- Call getDeviceType method after the device ready
- either constructor() or ngOnInit() add the following code snippet.
- store this to some variable because if we call deviceReady then controller is going in to
```javascript

let that; (Add this code to top of the class, after all import files)
 constructor() {
 
that = this;
document.addEventListener('deviceready', this.deviceReady, true);
}
deviceReady() {
    cordovaJsWrapper.getDeviceType(that.getDeviceTypeCallback); //getDeviceTypeCallback: callback from the jswrapper.
}
getDeviceTypeCallback(data) { // data:- which is passed from Jswrapper
  that._zone.run(() => {
      console.log(data);
    });
}
```


