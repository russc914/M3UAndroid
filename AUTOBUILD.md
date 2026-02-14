# APK Autobuild Documentation

This document describes the automated APK building process for the M3UAndroid project.

## Overview

M3UAndroid uses GitHub Actions to automatically build APK files for both the smartphone and TV variants of the app. The builds are triggered automatically on code changes and can also be manually triggered.

## Automated Build Triggers

### 1. Automatic Builds

APKs are automatically built in the following scenarios:

- **Push to master branch**: When code is pushed to the master branch (excluding documentation and configuration files)
- **Pull Requests**: When a pull request is opened or updated against the master branch

### 2. Manual Builds

You can manually trigger a build at any time:

1. Go to the [Actions tab](../../actions) in the GitHub repository
2. Select "Android CI" workflow
3. Click "Run workflow"
4. Choose your build options:
   - **release**: Build only release APKs (default)
   - **debug**: Build only debug APKs
   - **both**: Build both release and debug APKs

## Build Variants

### Smartphone App
- **Application ID**: `com.m3u.smartphone`
- **Version**: 1.15.1 (versionCode 145)
- **Target SDK**: 33
- **Min SDK**: 26

### TV App
- **Application ID**: `com.m3u.tv`
- **Version**: 1.0.1 (versionCode 2)
- **Target SDK**: 33
- **Min SDK**: 26

## Build Outputs

### Release Builds

Release builds are optimized with:
- ProGuard minification enabled
- Resource shrinking enabled
- Multi-ABI splits for smartphone (universal APK included)
- Debug signing (for testing purposes)

**Output files:**
- Smartphone: `app/smartphone/build/outputs/apk/release/*.apk`
- TV: `app/tv/build/outputs/apk/release/*.apk`

### Debug Builds

Debug builds are optimized for development with:
- No minification
- No resource shrinking
- Faster build times
- Debug signing

**Output files:**
- Smartphone: `app/smartphone/build/outputs/apk/debug/*.apk`
- TV: `app/tv/build/outputs/apk/debug/*.apk`

## Downloading Built APKs

### From GitHub Actions

1. Go to the [Actions tab](../../actions)
2. Click on the workflow run you want
3. Scroll down to the "Artifacts" section
4. Download the desired APK:
   - `smartphone-release-apk` - Smartphone release builds
   - `smartphone-debug-apk` - Smartphone debug builds
   - `tv-release-apk` - TV release builds
   - `tv-debug-apk` - TV debug builds

Artifacts are retained for 30 days.

### From Telegram (Release Builds Only)

Release builds are automatically posted to the project's Telegram channel after successful builds on the master branch. This does not apply to pull request builds.

## Build Process

The autobuild process consists of the following steps:

1. **Checkout**: Retrieve the latest code from the repository
2. **Setup JDK**: Install Java Development Kit 17 (Zulu distribution)
3. **Setup Gradle**: Configure Gradle build system with caching
4. **Clean**: Clean up managed devices (for benchmarking)
5. **Build**: Compile and package the APK files
6. **Upload**: Store APK artifacts in GitHub Actions
7. **Telegram**: Post release builds to Telegram channel (master branch only)

## Build Configuration

### Gradle Configuration

The project uses Gradle 8.11.1 with the following key configurations:

- **Compile SDK**: 36
- **Java Version**: 17
- **Kotlin**: Latest stable version
- **AGP**: Android Gradle Plugin 8.7.2

### Build Performance

- **Concurrent builds**: Disabled during CI to avoid conflicts
- **Cache**: Gradle dependencies are cached between builds
- **Cleanup**: Gradle home cache is cleaned after each build

## Troubleshooting

### Build Failures

If a build fails, check the following:

1. **Dependency Issues**: Verify all dependencies are available in repositories
2. **Gradle Version**: Ensure Gradle wrapper is using the correct version
3. **Java Version**: Confirm JDK 17 is being used
4. **Secrets**: For Telegram uploads, ensure bot credentials are configured

### Local Builds

To build APKs locally:

```bash
# Build smartphone release APK
./gradlew :app:smartphone:assembleRelease

# Build smartphone debug APK
./gradlew :app:smartphone:assembleDebug

# Build TV release APK
./gradlew :app:tv:assembleRelease

# Build TV debug APK
./gradlew :app:tv:assembleDebug

# Build all variants
./gradlew assembleRelease
./gradlew assembleDebug
```

## CI/CD Workflow File

The autobuild configuration is defined in `.github/workflows/android.yml`.

Key features:
- Parallel job support
- Conditional builds based on inputs
- Artifact upload with 30-day retention
- Telegram integration for release builds
- Path-based trigger filtering

## VR Support

As of the latest update, the smartphone variant includes Meta Quest 3 VR support. See [VR_SUPPORT.md](VR_SUPPORT.md) for details on VR-specific features.

## Development Workflow

### For Contributors

1. Fork the repository
2. Make your changes in a feature branch
3. Open a pull request
4. The CI will automatically build your changes
5. Download artifacts from the PR's Actions tab to test

### For Maintainers

1. Review pull requests with automated builds
2. Merge approved PRs to master
3. Master branch builds are automatically posted to Telegram
4. Release builds can be distributed from GitHub releases

## Future Enhancements

Potential improvements to the autobuild system:

- [ ] Automated testing before build
- [ ] Code signing with release keys
- [ ] Google Play Store automated publishing
- [ ] Build time optimization
- [ ] Multi-architecture optimization
- [ ] Automated changelog generation
- [ ] APK size tracking and reporting

## Support

For issues related to the autobuild system:
- Check [GitHub Actions logs](../../actions) for detailed error messages
- Review [Issues](../../issues) for known problems
- Join the [Telegram Discussion](https://t.me/m3u_android_chat)

## References

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Android Gradle Plugin](https://developer.android.com/build)
- [Gradle Build Tool](https://gradle.org/)
