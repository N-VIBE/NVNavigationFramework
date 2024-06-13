# NVDirection

## Get route

To calculate route, you need to use the method `getNavigationRoute()` with a start and end location.

```swift
import NVDirectionKit
import CoreLocation

class MyClass: UIViewController {

    override func viewDidLoad() {
        let origin = CLLocationCoordinate2D(latitude: 48.85934519646515, longitude: 2.3790224217764826)
        let destination = CLLocationCoordinate2D(latitude: 48.85869580049223, longitude: 2.373062553499055)
        
        NVDirection.shared.getNavigationRoute(from: NVibeLocation(address: "7 Avenue Parmentier", position: origin), to: NVibeLocation(address: "Pépinière 27", position: destination))
    }
}
```

Class NVibeLocation has 3 parameters and two of them are optional.

1. address : represent the name of the location. If not set, it’s replaced by an empty string.

2. position : represent the coordinate where the location is.

3. access : represent the coordinate where the location entry is on the road. By default, this coordinate is calculated by the projection of the position coordinate to the nearest road but sometimes, the nearest road is not where the entry is so you can set this value to force the calculated route to this coordinate.

When you request a route, you can also add a departure side street to tell if you want that the calculated route starts from the left or the right side. If no side street is set, the starting side will be set to the opposite of the first instruction (turn left will be set with a right starting side).

```swift
import NVDirectionKit
import CoreLocation

class MyClass: UIViewController {

    override func viewDidLoad() {
        let origin = CLLocationCoordinate2D(latitude: 48.85934519646515, longitude: 2.3790224217764826)
        let destination = CLLocationCoordinate2D(latitude: 48.85869580049223, longitude: 2.373062553499055)
        
        NVDirection.shared.getNavigationRoute(from: NVibeLocation(address: "7 Avenue Parmentier", position: origin), to: NVibeLocation(address: "Pépinière 27", position: destination), sideStreet: .LEFT)
    }
}
```

When you calculate a route, you can change the language of the instruction. Available languages are English (en), French (fr), Dutch (nl), Spanish (es), German (de).

```swift
import NVDirectionKit
import CoreLocation

class MyClass: UIViewController {

    override func viewDidLoad() {
        let origin = CLLocationCoordinate2D(latitude: 48.85934519646515, longitude: 2.3790224217764826)
        let destination = CLLocationCoordinate2D(latitude: 48.85869580049223, longitude: 2.373062553499055)
        
        NVDirection.shared.getNavigationRoute(from: NVibeLocation(address: "7 Avenue Parmentier", position: origin), to: NVibeLocation(address: "Pépinière 27", position: destination), language: "en")
    }
}
```

You have access to the result directly from the completion handler. `solutionFound` value indicates if the result contains good crossing pathing.

```swift
import NVDirectionKit
import CoreLocation

class MyClass: UIViewController {

    override func viewDidLoad() {
        let origin = CLLocationCoordinate2D(latitude: 48.85934519646515, longitude: 2.3790224217764826)
        let destination = CLLocationCoordinate2D(latitude: 48.85869580049223, longitude: 2.373062553499055)
        
        NVDirection.shared.getNavigationRoute(from: NVibeLocation(address: "7 Avenue Parmentier", position: origin), to: NVibeLocation(address: "Pépinière 27", position: destination)) { (result, error) in
            guard result != nil, error == nil else {
                //You will receive an error if the route calculation fails. More detail about the error in the error object.
                return
            }
            //You receive the calculated route here and additional data.
            result.route //NVibeRoute
            result.startingDirection //CLLocationDirection
            result.solutionFound //Bool
        }
    }
}
```

If you want only the starting direction of a route, you can use `getStartingDirection()` instead. It will count as a route calculation for the API but without crossing so it will be faster. If you have already your route, just get the starting direction from the object data.

```swift
import NVDirectionKit
import CoreLocation

class MyClass: UIViewController {

    override func viewDidLoad() {
        let origin = CLLocationCoordinate2D(latitude: 48.85934519646515, longitude: 2.3790224217764826)
        let destination = CLLocationCoordinate2D(latitude: 48.85869580049223, longitude: 2.373062553499055)
        
        NVDirection.shared.getStartingDirection(from: NVibeLocation(address: "7 Avenue Parmentier", position: origin), to: NVibeLocation(address: "Pépinière 27", position: destination)) { (direction, error) in
            guard result != nil, error == nil else {
                //You will receive an error if the route calculation fails. More detail about the error in the error object.
                return
            }
            //You receive the direction here.
            direction //CLLocationDirection
        }
    }
}
```

## Get custom route

You can request a custom route the same way as normal route. You just need to add a list of `CLLocationCoordinate2D`. The list need at least two coordinates.

```swift
import NVDirectionKit
import CoreLocation

class MyClass: UIViewController {

    override func viewDidLoad() {
        let origin = CLLocationCoordinate2D(latitude: 48.85934519646515, longitude: 2.3790224217764826)
        let destination = CLLocationCoordinate2D(latitude: 48.85869580049223, longitude: 2.373062553499055)
        let path: [CLLocationCoordinate2D] = [origin, CLLocationCoordinate2D(latitude: 48.86029101560107, longitude: 2.378561057106947), destination]
        
        NVDirection.shared.getCustomNavigationRoute(from: NVibeLocation(address: "7 Avenue Parmentier", position: origin), to: NVibeLocation(address: "Pépinière 27", position: destination), with: path, language: "en") { (result, error) in
            guard result != nil, error == nil else {
                //You will receive an error if the route calculation fails. More detail about the error in the error object.
                return
            }
            //You receive the calculated route here and additional data.
            result.route //NVibeRoute
            result.startingDirection //CLLocationDirection
        }
    }
}
```

## Get multimodal route

To calculate route, you need to use the method `getMultimodalRoute()` with a start and end location and set if you want full walking route with it (it's set to true by default).

```swift
import NVDirectionKit
import CoreLocation

class MyClass: UIViewController {

    override func viewDidLoad() {
        let origin = CLLocationCoordinate2D(latitude: 48.85934519646515, longitude: 2.3790224217764826)
        let destination = CLLocationCoordinate2D(latitude: 48.85869580049223, longitude: 2.373062553499055)
        
        NVDirection.shared.getMultimodalRoute(from: NVibeLocation(address: "7 Avenue Parmentier", position: origin), to: NVibeLocation(address: "Pépinière 27", position: destination), includeFullWalking: true) { (multimodalRoutes, error) in
            guard result != nil, error == nil else {
                //You will receive an error if the route calculation fails. More detail about the error in the error object.
                return
            }
            //You receive the list of multimodal route here.
            multimodalRoutes //NVibeMultimodalRoute
        }
    }
}
```
