# NVibeMultimodal

## NVibeMultimodalRoute

| NVibeMultimodalRoute        | Type                       | Description |
|:---------------------------:|:--------------------------:|:--------------------------:|
| startAddress                | NVibeLocation              | Start location of the full route |
| endAddress                  | NVibeLocation              | End location of the full route |
| distance                    | Double                     | Distance of the full route |
| duration                    | Double                     | Duration of the full route |
| steps                       | [NVibeMultimodalStep]      | Steps describing the multimodal route |

## NVibeMultimodalStep

| NVibeMultimodalStep         | Type                                | Description |
|:---------------------------:|:-----------------------------------:|:--------------------------:|
| startAddress                | NVibeLocation                       | Start location of the specific step |
| endAddress                  | NVibeLocation                       | End location of the specific step |
| distance                    | Double                              | Distance of the specific step |
| duration                    | Double                              | Duration of the specific step |
| information                 | NVibeMultimodalStepInformation      | More information about the step |

## NVibeMultimodalStepInformation

`NVibeMultimodalStepInformation` can be `NVibeWalkingInformation` or `NVibeTransitInformation` regarding the mode. So, you can check the mode and then cast to the specific type.

```swift
    if information.mode == .WALKING {
        let walkingInformation = information as! NVibeWalkingInformation
    } else {
        let transitInformation = information as! NVibeTransitInformation
    }
```

| NVibeWalkingInformation     | Type                                             | Description |
|:---------------------------:|:------------------------------------------------:|:--------------------------:|
| mode                        | NVibeMultimodalStepInformation.Mode = WALKING    | Always WALKING |
| startAddress                | NVibeLocation                                    | Start location of the walking step |
| endAddress                  | NVibeLocation                                    | End location of the walking step |
| walkingMode                 | NVibeWalkingInformation.Mode = DEFAULT or INDOOR | Describe if walking step is outdoor or indoor |

| NVibeTransitInformation     | Type                                                          | Description |
|:---------------------------:|:-------------------------------------------------------------:|:--------------------------:|
| mode                        | NVibeMultimodalStepInformation.Mode = TRANSIT                 | Always TRANSIT |
| transitMode                 | NVibeTransitInformation.Mode = TRAM or TRAIN or SUBWAY or BUS | Describe the type of transit |
| headsign                    | String                                                        | Headsign of the transit |
| backgroundColor             | String                                                        | Background color of the transit icon |
| textColor                   | String                                                        | Text color of the transit icon |
| type                        | String                                                        | Same as transitMode but human readable and usable with line (e.g., Métro 6) |
| line                        | String                                                        | Line of the transit |
| stops                       | [NVibeLocation]                                               | List of stops |
| duration                    | String                                                        | Duration of the transit |
| departureTime               | String                                                        | Departure time of the transit (e.g., 20240109T102900) |
| id                          | String                                                        | Specific id for departure stop |

`NVibeTransitInformation` has several method to obtain more information.

```swift
    let information: NVibeTransitInformation!
    information.stopNumber()      //return the total number of station
    information.departureStop()   //return the first stop
    information.arrivalStop()     //return the last stop
    information.getDistance()     //return the distance from the first stop to the last stop with direct line
    information.getDuration()     //return the duration of the transit step
```
