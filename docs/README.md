# N-Vibe Navigation SDK for iOS

NVNavigationKit is a full kit to reproduce turn-by-turn used in the N-Vibe application.

## Get started

To use NVNavigationKit, some requirements are needed.

   - ## Installation

     - ## Dependency

       NVNavigationKit is only available via Cocoapods for now. You need to enable `BUILD_LIBRARY_FOR_DISTRIBUTION` to work.

       ```ruby
       platform :ios, '13.0'

       target 'TargetNameForYourApp' do
         use_frameworks!
         
         pod 'NVNavigationKit', '~> 0.17.0'
       end
       
       post_install do |installer|
         installer.pods_project.targets.each do |target|
           target.build_configurations.each do |config|
             config.build_settings['BUILD_LIBRARY_FOR_DISTRIBUTION'] = 'YES'
           end
         end
       end
       ```
     - ## Token
       
       There are two tokens to add in Info.plist to use all features.

       You need one token with the key NVibeAPIAccessToken to get the route from the N-Vibe calculator.

       ```
       <key>NVibeAPIAccessToken</key>
       <string>YourToken</string>
       ```
       
       You need another token with the key NVibeSDKAccessToken to use turn-by-turn navigation.

       ```
       <key>NVibeSDKAccessToken</key>
       <string>YourToken</string>
       ```

       If you want just the route, you can use only NVibeAPIAccessToken but if you want turn-by-turn, NVibeAPIAccessToken is also needed to enable rerouting.

     - ## Http request permission
    
       To use NVNavigationKit, you need also a temporary permission to allow http request, this will change later.

       To add this permission, you need to add this key in your Info.plist.

       ```
       <key>NSAppTransportSecurity</key>
       <dict>
         <key>NSAllowsArbitraryLoads</key>
         <true/>
       </dict>
       ```
  - ## Location tracking

    Prior to use turn-by-turn, we need location access enabled when in use from the user.

    To achieve this, you need to add the key NSLocationWhenInUseUsageDescription in your Info.plist and request it from CLLocationManager.

    ```
    <key>NSLocationWhenInUseUsageDescription</key>
    <string>Your location is used to calculate turn-by-turn</string>
    ```

    Since iOS 14, you can set accuracy location level, ensure that fullAccuracy is set.
