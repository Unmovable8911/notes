# The Notes of Head First HTML5 Programming

## Properties and methods of some objects
| **object** | **Properties** | **Methods** |
| --- | --- | --- |
| *window* | location: holds the url of the page | alert |
|   | **status**: the string displayed in the status bar at the bottom of the screen | **prompt**: behaves like alert, except it gets informantion from the user |
|   | **onload** | **open**: open a browser window |
|   | **document** | **close**: close the window |
|   |   | **setTimeOut**: invokes a handler after a specified interval |
|   |   | **setInterval**: invokes a handler on a specified time interval, over and over |
| *document* | **domain**: the domain name of the page | **getElementById** |
|   | **title**: the title of the page, the very element in the `<head>` element | **getElementsByTagName**, **getElementsByClass** |
|   | **URL** | **createElement** |
| *element* | **innerHTML** |
|   | **childElementCount**: number of children the element has | **appendChild** |
|   | **firstChild** | **insertBefore**: insert element(s) as child(ren) of *element* |
|   |   | **getAttribute**, **setAttribute** |

## JavaScript objects
*JavaScript objects* works like an *instance of class in python*, but a JvaScript
object can be defined in a way that looks like a python *dict*.
### Here's an example of a javascript object
```
fido = {
    name: 'fido',
    weight: 21,
    age: 2,
    looseWeight: function() {
        this.weight -= 5;
    }
}
fido.looseWeight();
fido['name'] // You can also refer to a property of an object by sqare brackes notations
```
### And here's an example of class in python
```
class Dog:
    def __init__(self, name, weight, age):
        self.name = name
        self.weight = weight
        self.age = age
    def loose_weight(self):
        self.weight -= 5

fido = Dog('fido', 21, 2)
fido.lose_weight()
```
### JavaScript object *constructor*
```
function Dog(name, weight, age) {
    this.name = name;
    this.weight = weight;
    this.age = age;
    this.looseWeight = function() {
        this.weight -= 5;
    }
}
fido = new Dog('fido', 21, 2);
fido.looseWeight()
```

## The `navigator.geolocation` object
`navigator.geolocation` is an object that contains the **entire** geolocation API.  
Here's a most used method of `geolocation`  
`getCurrentPosition(successHandler, errorHandler, options)`  
`successHandler` is a function to call when the browser successed in getting the location of the user, and it is passed a **posion** object.  
The `errorHandler` is called when the browser failed to obtain the location of the user, and it is passed an **error** object.
The `options` takes an object to control the behavior of `getCurrentPosition`, here's an expample of options:
```
var options = {
    enableHighAccuracy: false,
    timeout: Infinity, // controls how long the browser gets to determine its location
    maximumAge: 0 // how long the browser waits to refresh the location, default value is 0
}
```
### Methods of `navigator.geolocation`
1. `getCurrentPosition`
2. `watchPosition`
3. `clearWatch`
### THe position and error object
#### Children of `position` object
##### i. `coords`
  1. latitude
  2. longitude
  3. accuracy
##### The following attributes may not be supported on some devices
  1. altitude
  2. altitudeAccuracy
  3. heading
  4. speed
##### ii. `timestamp`
#### The `error` object
An example of using the `error` object
```
var errorTypes = {
    0: 'Unknow Error',
    1: 'Permission denied by user',
    2: 'Position is not available',
    3: 'Request timeout'
}
var error Message = errorType[error.code];
```
## Request and Response
```
window.onload = function() {
    var rq = new XMLHttpRequest(); // XMLHttpRequest object is the standard javascript object sending or receiving data to or from server
    var url = 'https://whatever-this-is.org/';
    rq.onload = function() {
        if (rq.status == 200) { // successfully receive data from server
            alert('Something');
        }
    }
    rq.open('GET', url); // set up the XMLHttpRequest object
    rq.send(null); // send giving data (in this case, null) to url
}
```