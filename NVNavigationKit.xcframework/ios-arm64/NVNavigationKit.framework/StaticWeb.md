# Convert docc archive to static web page

$(xcrun --find docc) process-archive \
transform-for-static-hosting NVNavigationKit.doccarchive \
--output-path NVNavigationKitStaticWeb.doccarchive \
--hosting-base-path NVNavigationFramework
