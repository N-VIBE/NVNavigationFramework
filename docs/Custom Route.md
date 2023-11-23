# Custom Route

You can build custom route from a list of coordinates and start turn by turn with it.

## Build custom route

First, you need a list of `CLLocationCoordinate2D`. Then, you can pass this list to the specific NVibeRoute's constructor to generate route based on this list. The list need at least two coordinates.

```swift
import CoreLocation
import NVDirectionKit

class MyClass: UIViewController {
    override func viewDidLoad() {
        let path: [CLLocationCoordinate2D] = []
        let customRoute = NVibeRoute(from: path)
    }
}
```

## Start TurnByTurn with custom route

To start TurnByTurn with custom route, it's the same as normal route. Custom route are not part of our map data so if a reroute occured, it will switch back to our route so if you want to stay with your custom route, you can set the reroute mode to `.FOLLOW_SAME_ROUTE`. After rerouting with this mode, the new route will always add instruction to come back to the custom route path.

```swift
import CoreLocation
import NVDirectionKit
import NVNavigationKit

class MyClass: UIViewController, NVNavigationServiceDelegate {
    private var navigationService: NVNavigationService? = nil
    
    override func viewDidLoad() {
        navigationService = NVNavigationService(delegate: self)
        
        let path: [CLLocationCoordinate2D] = []
        let customRoute = NVibeRoute(from: path)
        
        navigationService?.startNavigation(route: customRoute, rerouteMode: .FOLLOW_SAME_ROUTE)
    }
    
    deinit {
        navigationService?.stopNavigation()
    }
    
    ...
}
```
