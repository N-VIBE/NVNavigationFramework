# N-Vibe Navigation SDK for iOS

NVNavigationKit is a kit to reproduce turn-by-turn used by N-Vibe application.

## Get started

To use NVNavigationKit, some requirements are needed.

## Dependency

NVNavigationKit is available from Cocoapods. Add it to your `podfile`:

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

Don't forget to install your pod by running:

```ruby
pod install --repo-update
```

## Token
  
You need a token with the key `NVibeAPIAccessToken` to receive route from the framework. You can add it in your `Info.plist`.

```
<key>NVibeAPIAccessToken</key>
<string>YourToken</string>
```

You need a token with the key `NVibeSDKAccessToken` to use navigation service from the framework. You can add it in your `Info.plist`.

```
<key>NVibeSDKAccessToken</key>
<string>YourToken</string>
```

## Http request permission

To use NVNavigationKit, you need also a temporary permission to allow http request, this will change later. You can add it in your `Info.plist`.

```
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

## Location tracking

Prior to use navigation service, we need location access enabled when in use from the user.

To achieve this, you need to add the key `NSLocationWhenInUseUsageDescription` in your `Info.plist` and request it from `CLLocationManager`.

```
<key>NSLocationWhenInUseUsageDescription</key>
<string>Your location is used to calculate turn-by-turn</string>
```

Since iOS 14, you can set accuracy location level, ensure that `fullAccuracy` is set.
