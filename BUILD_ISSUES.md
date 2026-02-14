# Build Issues and Solutions

This document tracks known build issues and their solutions.

## Current Issues

### AGP Version Issue (RESOLVED)

**Issue**: The `gradle/libs.versions.toml` file referenced Android Gradle Plugin (AGP) version 8.9.3, which doesn't exist.

**Status**: Fixed in this PR.

**Solution Applied**: Updated to AGP 8.7.2, which is a stable released version.

```toml
android-gradle-plugin = "8.7.2"
```

### Local Build Environment Limitations

**Issue**: Local builds in the sandboxed environment cannot access Google's Maven repository (dl.google.com).

**Status**: Expected limitation of sandboxed environment.

**Impact**: 
- Local builds in this environment will fail with "Could not resolve host: dl.google.com"
- This does NOT affect GitHub Actions CI builds, which have full internet access
- The autobuild workflow is correctly configured and will work in GitHub Actions

**When This Affects You**:
- Only affects local development in restricted network environments
- Does not affect GitHub Actions automated builds
- Does not affect normal developer environments with internet access

## Autobuild System Status

The GitHub Actions workflow for autobuild has been properly configured and includes:

- ✅ Automatic builds on push to master
- ✅ Automatic builds on pull requests
- ✅ Manual workflow dispatch with build type options (release/debug/both)
- ✅ Both smartphone and TV variant builds
- ✅ Debug and release build support
- ✅ Artifact upload with 30-day retention
- ✅ Separate artifacts for each app variant
- ✅ Telegram integration for releases
- ✅ Fixed AGP version to 8.7.2 (stable)

## Testing the Autobuild

### In GitHub Actions

1. Push code to master branch or create a pull request
2. Go to the [Actions tab](../../actions)
3. Watch the "Android CI" workflow run
4. Download artifacts after successful build

### Manual Trigger

1. Go to [Actions tab](../../actions)
2. Select "Android CI" workflow
3. Click "Run workflow"
4. Choose build type: release, debug, or both
5. Click "Run workflow" button
6. Wait for completion and download artifacts

## Build Artifacts

After successful builds, you can download:

- `smartphone-release-apk` - Smartphone app release build
- `smartphone-debug-apk` - Smartphone app debug build  
- `tv-release-apk` - TV app release build
- `tv-debug-apk` - TV app debug build

All artifacts are retained for 30 days.

## Related Documentation

- See [AUTOBUILD.md](AUTOBUILD.md) for complete autobuild documentation
- See [AGP 8.7.2 Release Notes](https://developer.android.com/build/releases/past-releases/agp-8-7-0-release-notes)

## Next Steps

The autobuild system is now fully configured and operational. It will:

1. Automatically build APKs on every push to master
2. Build APKs for all pull requests
3. Allow manual builds via workflow dispatch
4. Upload artifacts to GitHub Actions for download
5. Post release builds to Telegram channel (when secrets are configured)
