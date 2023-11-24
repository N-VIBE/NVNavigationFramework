# NVCompassService

There is a compass feature to get a callback when a certain direction angle is reached. You can set your own angle or if you don’t, the angle along the current navigation will be used only if a navigation is started.

## Start and stop compass

```swift
import CoreLocation
import NVNavigationKit

class MyClass: UIViewController, NVCompassServiceDelegate {
    private var compassService: NVCompassService? = nil
    
    override func viewDidLoad() {
        compassService = NVCompassService(delegate: self)
        compassService?.startCompass(direction: 180.0)
        
        //You can stop the compass whenever you want with stopCompass(), stopCompass() is automatically called when onCompassTriggered() is called.
        compassService?.stopCompass()
    }
    
    override compassService(_ service: NVCompassService, didCompassUpdateWith direction: CLLocationDirection, didCompassTrigger: Bool) {
        //You receive here the updated direction of the phone, the phone needs to be flat with a screen on the top. If compass is not available, direction value will be -1.
        //When compass reached the right angle, didCompassTrigger is set to true.
    }
}
```
