## NVTransitService

## Start and stop Transit TurnByTurn

First, you need to create a `NVTransitService` object, then you can use `startTransitNavigation()` and `stopTransitNavigation()`. `startTransitNavigation()` takes `NVibeTransitInformation` object which you can get in `NVibeMultimodalRoute` object. To get callback, you need to set the proper delegate.

```swift
import CoreLocation
import NVNavigationKit

class MyClass: UIViewController, NVTransitServiceDelegate {
    private var transitService: NVTransitService? = nil
    
    override func viewDidLoad() {
        transitService = NVTransitService(delegate: self)
        
        let multimodalRoute: NVibeMultimodalRoute //You can get route from NVDirection
        if multimodalRoute.steps.first.information.mode == .TRANSIT {
            transitService?.startTransitNavigation(departure: multimodalRoute.steps.first.startAddress, arrival: multimodalRoute.steps.first.endAddress, transit: (multimodalRoute.steps.first.information as! NVibeTransitInformation))
        }
    }
    
    deinit {
        navigationService?.stopNavigation()
    }

    override func transitService(_ service: NVTransitService, didFailToPassTokenWith error: TokenError) {
        //You will receive error related to SDK Token here
    }

    override func transitService(_ service: NVTransitService, didUpdateTransitTo stop: NVibeLocation, with remainingStop: Int) {
        //Call when the next transit stop is near
    }
}
```
