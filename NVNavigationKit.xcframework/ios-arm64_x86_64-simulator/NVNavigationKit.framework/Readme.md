#  BUILD DEVICE

xcodebuild archive \
-workspace NVNavigationKit.xcworkspace \
-scheme NVNavigationKit \
-configuration Release \
-sdk iphoneos \
-archivePath archives/NVNavigationKit-iphoneos.xcarchive \
BUILD_LIBRARY_FOR_DISTRIBUTION=YES \
SKIP_INSTALL=NO

#  BUILD SIMULATOR

xcodebuild archive \
-workspace NVNavigationKit.xcworkspace \
-scheme NVNavigationKit \
-configuration Release \
-sdk iphonesimulator \
-archivePath archives/NVNavigationKit-iphonesimulator.xcarchive \
BUILD_LIBRARY_FOR_DISTRIBUTION=YES \
SKIP_INSTALL=NO

#  BUILD XCFRAMEWORK

xcodebuild -create-xcframework \
-framework 'archives/NVNavigationKit-iphonesimulator.xcarchive/Products/Library/Frameworks/NVNavigationKit.framework' \
-framework 'archives/NVNavigationKit-iphoneos.xcarchive/Products/Library/Frameworks/NVNavigationKit.framework' \
-output 'archives/NVNavigationKit.xcframework'

#  PUBLISH COCOAPOD

pod trunk push NVNavigationKit.podspec --skip-import-validation
