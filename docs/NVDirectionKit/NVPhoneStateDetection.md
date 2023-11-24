# NVPhoneStateDetection

## Start and stop phone state detection

To use phone state detection, you need to use the method `startPhoneStateDetection()` and `stopPhoneStateDetection()` along with notification center if you want callback.

```swift
import NVDirectionKit
import CoreLocation

class MyClass: UIViewController {

    @objc func onReceiveNotification(_ notification: NSNotification) {
        switch notification.name {
        case .NOTIFICATION_PHONE_STATE_CHANGED:
            guard let phoneState = notification.userInfo?["state"] as? PhoneState else {
                return
            }
            phoneState.state //State
            phoneState.orientation //CLDeviceOrientation
        default:
            return
        }
    }
    
    override func viewDidLoad() {
        NotificationCenter.default.addObserver(self, selector: #selector(onReceiveNotification(_:)), name: .NOTIFICATION_PHONE_STATE_CHANGED, object: nil)
                
        NVDirection.shared.startPhoneStateDetection() {
            //The phone state detection is started here
        }
    }
    
    deinit {
        NotificationCenter.default.removeObserver(self)
        NVDirection.shared.stopPhoneStateDetection()
    }
}
```
