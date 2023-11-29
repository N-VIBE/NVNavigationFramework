## NVMultimodalService

## Start and stop multimodal route

First, you need to create a `NVMultimodalService` object, then you can use `startMultimodalNavigation()` and `stopMultimodalNavigation()`. `NVMultimodalService` will help you to follow the different step containing in the multimodal route.

```swift
import CoreLocation
import NVNavigationKit

class MyClass: UIViewController, NVMultimodalServiceDelegate {
    private var multimodalService: NVMultimodalService? = nil
    
    override func viewDidLoad() {
        multimodalService = NVMultimodalService(delegate: self)
        
        let multimodalRoute: NVibeMultimodalRoute //You can get route from NVDirection
        multimodalService?.startMultimodalNavigation(departure: multimodalRoute.startAddress, arrival: multimodalRoute.endAddress, route: multimodalRoute)
    }
    
    deinit {
        multimodalService?.stopMultimodalNavigation()
    }

    override func multimodalService(_ service: NVMultimodalService, didFailToPassTokenWith error: TokenError) {
        //You will receive error related to SDK Token here
    }

    override func multimodalService(_ service: NVMultimodalService, from route: NVibeMultimodalRoute, shouldGoToNext multimodalStep: NVibeMultimodalStep, with remainingMultimodalStep: Int) -> Bool {
        //You will receive a callback each time you finished a multimodal step. You can set a boolean to tell if you want to start immediatly the next step. It's set to true by default.
        return true
    }

    override func multimodalService(_ service: NVMultimodalService, from route: NVibeMultimodalRoute, didChange multimodalStep: NVibeMultimodalStep, with remainingMultimodalStep: Int) {
        //When you set the previous callback to true, you will get this one just after to tell you that the step has been correctly changed and you will get information for the next multimodal step. It will not start TurnByTurn, it's on your side to set `NVNavigationService` or `NVTransitService` to start the TurnByTurn corresponding to the multimodal step. When the next TurnByTurn that you started will be complete, it will call again the above callback to start the next multimodal step. If you don't succeed to complete the next TurnByTurn (bad accuracy at arrival), you can use `multimodalService?.skipMultimodalStepNavigation()` to trigger the above callback and stops the current TurnByTurn.
    }
}
```
