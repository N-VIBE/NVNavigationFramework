# NVNavigationService

## Start and stop TurnByTurn

First, you need to create a `NVNavigationService` object, then you can use `startNavigation()` and `stopNavigation()`. To get callback, you need to set the proper delegate.

```swift
import CoreLocation
import NVNavigationKit

class MyClass: UIViewController, NVNavigationServiceDelegate {
    private var navigationService: NVNavigationService? = nil
    
    override func viewDidLoad() {
        navigationService = NVNavigationService(delegate: self)
        
        let route: NVibeRoute //You can get route from NVDirection
        navigationService?.startNavigation(route: route)
    }
    
    deinit {
        navigationService?.stopNavigation()
    }
}
```

There are 4 callbacks almost essential and 5 others more optional.

```swift
//================================= Essential ===============================

override func navigationService(_ service: NVNavigationService, didStartWith step: NVibeStep, atRemaining routeDistance: CLLocationDistance, atRemaining stepDistance: CLLocationDistance, withRemaining routeDuration: Double, with image: ImageType) {
    //This callback announces when turn-by-turn navigation is started.
}

override func navigationService(_ service: NVNavigationService, didUpdate progress: NVibeRouteProgress, with rawLocation: CLLocation, snappedTo location: CLLocation) {
    //This callback announces when turn-by-turn navigation is updated and you can retrieve the general progress of the navigation (remaining distance, current step, etc.).
}

override func navigationService(_ service: NVNavigationService, didStopWith instruction: String, with image: ImageType) {
    //This callback announces when turn-by-turn navigation is finished so when the user reaches his destination.
}

override func navigationService(_ service: NVNavigationService, shouldRerouteFrom location: CLLocation) -> Bool {
    //You can set here if you want enable rerouting or not
    return true
}

override navigationService(_ service: NVNavigationService, didRerouteAlong route: NVibeRoute, to direction: CLLocationDirection, with step: NVibeStep, atRemaining routeDistance: CLLocationDistance, atRemaining stepDistance: CLLocationDistance, withRemaining routeDuration: Double, with image: ImageType) {
    //This callback announces when turn-by-turn navigation is rerouted and you will receive the new route. You cannot reroute if you don't have a valid API token.
}

//================================= Optional ================================

override navigationService(_ service: NVNavigationService, didUpdateUIWith step: NVibeStep, atRemaining routeDistance: CLLocationDistance, atRemaining stepDistance: CLLocationDistance, with image: ImageType) {
    //This callback announces when the turn-by-turn navigation UI needs to be updated, it's the same data as in onWalkingNavigationUpdated but here, you have access directly to the more important data.
}

override func navigationService(_ service: NVNavigationService, didPauseNavigationOnBadAccuracy: Bool) {
    //This callback announces if the navigation is paused due to bad location accuracy
}

override navigationService(_ service: NVNavigationService, didFailToPassTokenWith error: TokenError) {
    //This callback announces an error occurred with NVibeNavigationAccessToken.
}

//All next callback include vibration details if you want to include vibrating wristbands from N-Vibe.

override navigationService(_ service: NVNavigationService, didUpdateRightDirectionWith vibration: NVibeVibration) {
    //This callback is triggered every 25 meters traveled. If you want to change the distance, you can set the value distanceBetweenRightDirection from the NVNavigationService object. This value is only applied before starting navigation so if you changed it during navigation, you will need to restart navigation.
}

override navigationService(_ service: NVNavigationService, willChange step: NVibeStep, with vibration: NVibeVibration, atRemaining stepDistance: CLLocationDistance) {
    //This callback is triggered 25 meters before the next instruction. If you want to change the distance, you can set the value distanceBeforePrepareChangeStep from the NVNavigationService object. This value is only applied before starting navigation so if you changed it during navigation, you will need to restart navigation.
}

override navigationService(_ service: NVNavigationService, didChange step: NVibeStep, with vibration: [NVibeVibration]) {
    //This callback is triggered when the next instruction needs to be done.
}

override navigationService(_ service: NVNavigationService, didForceChangeSideFor step: NVibeStep, atRemaining stepDistance: CLLocationDistance, with vibration: NVibeVibration) {
    //This callback is triggered when you need to change the side of the street to prepare the next instruction even if there is no crossing. It’s triggered only when you have no proper solution for the calculated route.
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

## Customize TurnByTurn

When you start a navigation, you can choose to pause the navigation when the accuracy of your GPS exceeds a certain value. By default, it’s set to true so when bad accuracy will be detected, you will receive a specific callback.

```swift
import CoreLocation
import NVNavigationKit

class MyClass: UIViewController, NVNavigationDelegate {
    private var navigationService: NVNavigationService? = nil
    
    override func viewDidLoad() {
        navigationService = NVNavigationService(delegate: self)
        navigationService?.pauseNavigationWhenBadAccuracy = true
        navigationService?.maximumAccuracyAllowed = 45.0
        
        //This value is only applied before starting navigation so if you changed it during navigation, you will need to restart navigation.
    }
    
    override func navigationService(_ service: NVNavigationService, didPauseNavigationOnBadAccuracy: Bool) {
        //When accuracy changes according to the specific value set, you can check here if the navigation is paused or not.
    }
}
```

You can set the distance between right direction update and the distance before the next instruction will be notified.

```swift
import CoreLocation
import NVNavigationKit

class MyClass: UIViewController, NVNavigationDelegate {
    private var navigationService: NVNavigationService? = nil
    
    override func viewDidLoad() {
        navigationService = NVNavigationService(delegate: self)
        navigationService?.pauseNavigationWhenBadAccuracy = true
        navigationService?.distanceBetweenRightDirection: Double = 20.0
        navigationService?.distanceBeforePrepareChangeStep: Double = 25.0
        
        //This value is only applied before starting navigation so if you changed it during navigation, you will need to restart navigation.
    }
    
    override navigationService(_ service: NVNavigationService, didUpdateRightDirectionWith vibration: NVibeVibration) {
        //This callback is triggered every 20 meters.
    }
    
    override navigationService(_ service: NVNavigationService, willChange step: NVibeStep, with vibration: NVibeVibration, atRemaining stepDistance: CLLocationDistance) {
        //This callback is triggered 25 meters before the next instruction.
    }
}
```

## Get location during TurnByTurn

You can get your current location coordinate during navigation according to the side street where you are supposed to be. It’s useful when you follow the navigation correctly but because of your GPS accuracy, you are detected on the other side of the street.

```swift
    navigationService?.getCurrentLocationDuringNavigation() { (result, error) in
        guard result != nil, error == nil else {
            //You will receive an error if the current location coordinate recuperation failed. More detail about the error will be set later.
            return
        }
        //You receive here the location coordinate, you can implement your own reverse geocoding after.
        result.coordinate //CLLocationCoordinate2D
    }
```

