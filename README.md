[![Build Status](https://travis-ci.org/mcross1882/geocoder.svg?branch=master)](https://travis-ci.org/mcross1882/geocoder)

Geocoder
========

A Google Maps geocoding library for Scala. The goal of the library is provide lightweight, 
easy to use geocoding functions that are thoroughly tested.

### Goals

- No 3rd-party dependencies (`scala-xml` is required for projects building on `2.11.x`)
- Fully unit tested
- API Compliant
- Easy to use and integrate

### Building

To build simply run the following `sbt` commands.

```
$ sbt clean compile test doc assembly
```

*API Documentation will be generated in `target/scala-2.{version}/api/`*

### Usage

To create a `Geocoder` simply call the `Geocoder.create` function. Refer to the `test/` directory
for more in depth examples, below is a high level overview on how to use the library.

```scala
// Bring the Geocoder into scope
import com.github.mcross1882.geocoder.{Geocoder, ResponseParser}

// The factory object can be used to lazily create Geocoders
val geo = Geocoder.create

// If you need to use an API key because of limiting
val geoWithKey = Geocoder.create(MY_KEY)

// Geocoders can be created with parameters that will be passed at every lookup
val geoWithParams = Geocoder.create(Parameters(region = Some("us")))

// And lastly if you need to manually create the Geocoder
// that's supported as well.
val customGeo = new Geocoder(API_URL, Some(API_KEY), None new ResponseParser)
```

To perform latitude/longitude lookups simply provide a formatted address.

```scala
// Lookup a location with a formatted address string
// Returns a Seq[Result]
val results = geo.lookup("2821 W 7th St, Fort Worth, TX")

// Access the MapComponents geometry data to get the location
val location = results.head.components.head.geometry.location

println(s"Latitude: ${location.latitude}, Longitude: ${location.longitude}")
```

Performing reverse lookups is just as easy.

```scala
// Lookup an address by latitude/longitude
// Reverse lookups also produce Seq[Result] objects
val results = geo.lookup(32.857, -96.748)

// Lookups can also be done with Component objects
// See com.github.mcross1882.geocoder.Component for more examples
val results = geo.lookup(Seq(CountryComponent("fr")))

// Place IDs are also supported
val results = geo.lookup("ChIJk4x9peBzToYRBYLucCG-eGY")
```

Asynchronous calls are also supported.

```scala
val geo = Geocoder.createAsync // Can also be created with a key

// Returns a Future[Seq[Result]]
val query = geo.lookup("2821 W 7th St, Fort Worth, TX")

// Process the Seq[Result]
query onSuccess { case results =>
    for (result <- results) {
        // do something...
    }
}

// Handle any exceptions or failures
query onFailure {
    case e: Exception => println(e.getMessage)
}
```

### Other Related Projects

These projects acheive the same purpose as this library and provide alternative options if this library is unsuitable.

- [NewMotion/scala-geocode](https://github.com/NewMotion/scala-geocode)
- [divarvel/geocoding-api](https://github.com/divarvel/geocoding-api)

