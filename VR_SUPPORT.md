# VR Support - Meta Quest 3

This document describes the VR features and optimizations added to M3UAndroid for Meta Quest 3 compatibility.

## Overview

M3UAndroid now supports Virtual Reality (VR) mode on Meta Quest devices, including Quest, Quest 2, Quest Pro, and Quest 3 (codename: eureka). The app can run in both standard 2D mode and VR mode, providing a dual-mode experience.

## Added Features

### 1. VR Permissions

The following Oculus/Meta-specific permissions have been added to enable advanced VR features:

- **Hand Tracking** (`com.oculus.permission.HAND_TRACKING`)
  - Enables hand tracking for controller-free interaction
  - Allows users to interact with the app using their hands naturally

- **Spatial Anchors** (`com.oculus.permission.USE_ANCHOR_API`)
  - Enables spatial anchors for persistent placement of virtual objects
  - Useful for remembering UI element positions in physical space

- **Scene Understanding** (`com.oculus.permission.USE_SCENE`)
  - Enables scene understanding for environment awareness
  - Allows the app to understand the physical environment

### 2. VR Hardware Features

The following hardware features are declared as optional (`android:required="false"`), ensuring the app remains compatible with non-VR devices while enabling enhanced features on VR headsets:

- **VR Head Tracking** (`android.hardware.vr.headtracking`)
  - Core VR feature for head position and rotation tracking
  - Version 1 specification

- **Hand Tracking** (`oculus.software.handtracking`)
  - Optional software feature for hand tracking
  - Falls back to controller input if not available

- **Passthrough** (`com.oculus.feature.PASSTHROUGH`)
  - Optional feature for mixed reality experiences
  - Allows users to see their physical environment through the headset

### 3. VR Intent Filters

The main activity now includes the VR launcher category:

- `com.oculus.intent.category.VR`
  - Makes the app discoverable in the Meta Quest launcher
  - Enables proper VR mode initialization when launched from VR

### 4. VR Application Metadata

Application-level metadata has been added:

- **VR Mode**: `vr_dual`
  - Enables dual-mode operation (both 2D and VR)
  - App can run on phones, tablets, and VR headsets

- **Supported Devices**: `quest|quest2|questpro|eureka`
  - Declares compatibility with Meta Quest device family
  - `eureka` is the codename for Quest 3

## Benefits

### For VR Users
- Immersive media viewing experience in VR
- Natural hand gesture controls (optional)
- Mixed reality mode with passthrough (optional)
- Spatial audio for enhanced immersion
- Optimized for Meta Quest 3 hardware

### For Non-VR Users
- No impact on existing functionality
- All features remain optional and backwards compatible
- App continues to work on phones and tablets as before

## Technical Implementation

All VR features are implemented as optional (`android:required="false"`), which means:

1. **Backwards Compatibility**: The app remains installable on non-VR Android devices
2. **Progressive Enhancement**: VR features activate automatically when available
3. **No Runtime Changes Required**: VR support is declarative via manifest entries

## Testing VR Features

To test VR features on Meta Quest devices:

1. Enable Developer Mode on your Quest device
2. Install the app via Android Debug Bridge (ADB) or through the Meta Quest Developer Hub
3. Launch the app from the Meta Quest launcher
4. VR mode will activate automatically if supported

## Future Enhancements

Potential future VR improvements:
- VR-specific UI layouts optimized for headset viewing
- Controller input mapping for Quest controllers
- Spatial audio implementation
- Hand tracking gesture controls
- Room-scale positioning for theater mode
- Social VR features for shared viewing

## Requirements

- **Minimum Android SDK**: 26 (Android 8.0)
- **Target Android SDK**: 36
- **Meta Quest OS**: Compatible with all Quest devices running latest OS
- **Build Tools**: Android Gradle Plugin 8.x+

## References

- [Meta Quest Developer Documentation](https://developer.oculus.com/documentation/native/android/mobile-intro/)
- [Android VR Guidelines](https://developer.android.com/develop/vr)
- [Meta Quest Manifest Requirements](https://developer.oculus.com/documentation/native/android/manifest/)

## Support

For VR-specific issues or questions:
- Check the [Issues](https://github.com/oxyroid/M3UAndroid/issues) page
- Join the [Telegram Discussion](https://t.me/m3u_android_chat)
- Review Meta Quest [Developer Forums](https://communityforums.atmeta.com/t5/Quest-Development/bd-p/quest-game-dev)
