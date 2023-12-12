# NVibeMultimodal

## NVibeMultimodalRoute

| NVibeMultimodalRoute        | Type                       |
|:---------------------------:|:--------------------------:|
| startAddress                | NVibeLocation              |
| endAddress                  | NVibeLocation              |
| distance                    | Double                     |
| duration                    | Double                     |
| steps                       | [NVibeMultimodalStep]      |

## NVibeMultimodalStep

| NVibeMultimodalStep         | Type                                |
|:---------------------------:|:-----------------------------------:|
| startAddress                | NVibeLocation                       |
| endAddress                  | NVibeLocation                       |
| distance                    | Double                              |
| duration                    | Double                              |
| information                 | NVibeMultimodalStepInformation      |

## NVibeMultimodalStepInformation

`NVibeMultimodalStepInformation` can be `NVibeWalkingInformation` or `NVibeTransitInformation` regarding the mode.

| NVibeWalkingInformation     | Type                                             |
|:---------------------------:|:------------------------------------------------:|
| mode                        | NVibeMultimodalStepInformation.Mode = WALKING    |
| startAddress                | NVibeLocation                                    |
| endAddress                  | NVibeLocation                                    |
| walkingMode                 | NVibeWalkingInformation.Mode = DEFAULT or INDOOR |

| NVibeTransitInformation     | Type                                                          |
|:---------------------------:|:-------------------------------------------------------------:|
| mode                        | NVibeMultimodalStepInformation.Mode = TRANSIT                 |
| transitMode                 | NVibeTransitInformation.Mode = TRAM or TRAIN or SUBWAY or BUS |
| headsign                    | String                                                        |
| backgroundColor             | String                                                        |
| textColor                   | String                                                        |
| type                        | String                                                        |
| line                        | String                                                        |
| stops                       | [NVibeLocation]                                               |
| duration                    | String                                                        |
| departureTime               | String                                                        |
| id                          | String                                                        |

`NVibeTransitInformation` has several method to obtain more information.

```swift
    let information: NVibeTransitInformation!
    information.stopNumber()      //return the total number of station
    information.departureStop()   //return the first stop
    information.arrivalStop()     //return the last stop
    information.getDistance()     //return the distance from the first stop to the last stop with direct line
    information.getDuration()     //return the duration of the transit step
```
