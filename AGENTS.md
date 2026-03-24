# Kindly Chat SDK - Android Public SDK Project Rules

## Project Overview
This is the **public release repository** for the Kindly Chat SDK for Android. This repository contains the packaged AAR, documentation, and example projects for external developers.

### Related Project Paths
- **Android Source**: `/Users/alexy/src/Android/Projects/kindly-chat-sources-android`
- **Android Wiki**: `/Users/alexy/src/Android/Projects/kindly-chat-sdk.wiki`
- **Android Public SDK**: `/Users/alexy/src/Android/Projects/kindly-chat-sdk` (current project)
- **iOS Source**: `/Users/alexy/src/iOS/Projects/kindly-chat-sources-ios`
- **iOS Wiki**: `/Users/alexy/src/iOS/Projects/KindlySDK.wiki`
- **iOS Public SDK**: `/Users/alexy/src/iOS/Projects/KindlySDK`

## Repository Structure

### Public SDK Components
- **kindly-chat-sdk.aar**: Compiled SDK library
- **Example Projects**: Sample Android applications demonstrating integration
- **Documentation**: Public API documentation and guides
- **build.gradle**: Gradle build configuration
- **proguard-rules.pro**: ProGuard/R8 optimization rules
- **README.md**: Getting started guide
- **CHANGELOG.md**: Version history and changes

### Distribution Formats
- **Maven Central**: Primary distribution method
- **JitPack**: Alternative distribution
- **Manual Integration**: Direct AAR integration
- **Gradle Dependencies**: Standard Android dependency management

## Public API Design

### SDK Entry Points
- **ChatKindlySDK**: Main SDK class with public interface
- **Configuration**: Public configuration options
- **Callbacks**: Public callback interfaces
- **Theme**: Public theming interface

### API Principles
- **Minimal Surface Area**: Expose only necessary public APIs
- **Clear Naming**: Self-documenting method and property names
- **Consistent Patterns**: Follow Android SDK conventions
- **Backward Compatibility**: Maintain API stability across versions

### Public Interface Guidelines
- Use `@JvmStatic` for Java compatibility where needed
- Provide comprehensive KDoc documentation
- Include usage examples in documentation
- Mark internal APIs as `internal` or `private`
- Follow Kotlin coding conventions

## Example Projects

### Sample App Structure
- **Basic Integration**: Simple chat implementation
- **Advanced Features**: Custom theming and configuration
- **Jetpack Compose Example**: Modern Compose integration
- **View System Example**: Traditional View system implementation

### Example Code Quality
- **Complete Examples**: Fully functional sample apps
- **Best Practices**: Demonstrate proper SDK usage
- **Error Handling**: Show proper error handling patterns
- **Documentation**: Well-commented example code
- **Modern Android**: Use latest Android development practices

## Documentation Standards

### Public Documentation
- **API Reference**: Complete public API documentation
- **Integration Guide**: Step-by-step setup instructions
- **Customization Guide**: Theming and configuration options
- **Migration Guide**: Version upgrade instructions
- **Jetpack Compose Guide**: Modern UI integration

### Code Documentation
- Use KDoc for Kotlin documentation
- Include parameter descriptions
- Provide usage examples
- Document return values and error conditions
- Include `@sample` annotations for examples

## Release Management

### Version Control
- **Semantic Versioning**: Follow semver for releases
- **Release Tags**: Tag all public releases
- **Release Notes**: Detailed changelog for each version
- **Breaking Changes**: Clear documentation of breaking changes

### Quality Assurance
- **Testing**: Comprehensive test coverage
- **Example Validation**: Ensure all examples work
- **Documentation Review**: Verify documentation accuracy
- **API Review**: Review public API changes
- **ProGuard Testing**: Verify obfuscation compatibility

## Distribution Channels

### Package Repositories
- **Maven Central**: Primary distribution method
- **JitPack**: Alternative for development builds
- **GitHub Packages**: Internal distribution

### Release Process
- Build AAR from source
- Update version numbers
- Generate documentation
- Test example projects
- Publish to Maven Central
- Update dependency documentation

## Android-Specific Considerations

### Gradle Integration
- **Dependencies**: Clear dependency declarations
- **Version Catalogs**: Support for modern dependency management
- **Build Variants**: Support for different build configurations
- **ProGuard Rules**: Proper obfuscation rules

### Compatibility
- **Android API Level**: Minimum SDK version requirements
- **Kotlin Version**: Kotlin language version support
- **Jetpack Compose**: Compose BOM compatibility
- **Architecture Components**: AndroidX library compatibility

### Performance
- **AAR Size**: Optimize library size
- **Method Count**: Monitor DEX method count
- **Dependencies**: Minimize transitive dependencies
- **R8/ProGuard**: Proper optimization rules


### Build Commands

**Important: Always build the SDK after making changes to ensure compilation integrity.**

#### iOS SDK Build
When making iOS changes, use this command to verify the build:
```bash
xcodebuild -project Kindly.xcodeproj -scheme Kindly -configuration Release build-for-testing
```
This command builds the Kindly framework and verifies all Swift files compile correctly.

#### Android SDK Build  
When making Android changes, use this command to verify the build:
```bash
./gradlew :kindlysdk:build
```
This command builds the kindlysdk module and verifies all Kotlin/Java files compile correctly.

**Usage Guidelines:**
- Run the appropriate build command whenever you modify source code
- Both commands must complete successfully before submitting changes
- Address any compilation errors before proceeding with implementation
- These builds verify syntax and dependencies without requiring simulators/devices

## Integration Support

### Developer Experience
- **Clear Setup Instructions**: Easy-to-follow integration steps
- **Troubleshooting**: Common issues and solutions
- **Support Channels**: How to get help
- **Community**: Developer community resources

### Build System Support
- **Gradle**: Primary build system support
- **Maven**: Alternative build system support
- **Dependency Management**: Clear dependency declarations
- **Version Conflicts**: Guidance on resolving conflicts

## Modern Android Features

### Jetpack Compose
- **Compose UI**: Modern declarative UI
- **Material Design 3**: Latest design system
- **Theme Integration**: Seamless theme integration
- **Performance**: Optimized Compose usage

### Architecture Components
- **ViewModel**: Proper lifecycle management
- **StateFlow**: Reactive state management
- **Coroutines**: Asynchronous programming
- **Navigation**: Modern navigation patterns

### Development Tools
- **Android Studio**: IDE compatibility
- **Lint Rules**: Custom lint rules for proper usage
- **Debug Tools**: Development and debugging support
- **Testing**: Unit and instrumentation testing support 