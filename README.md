# PodKit

PodKit is a tool that helps with [CocoaPods](https://github.com/CocoaPods/CocoaPods) organization.

## Quick Start

Add `PodKit` requirement, configuration and install hooks into your `Podfile`:

```ruby
require_relative 'Git/PodKit/source/pod_kit'

# Include base configuration per-target types.
configure :application, 'xcconfigs/macOS/macOS-Application.xcconfig'
configure :framework, 'xcconfigs/macOS/macOS-Framework.xcconfig'
configure :test, 'xcconfigs/macOS/macOS-XCTest.xcconfig'

# Use custom location for CocoaPods groups.
group 'dependency/CocoaPods'

# Remove "[CP]" prefix from CocoaPods build phases.
prefix :build_phase, ''

# Set up install hooks.
pre_install { |installer| PodKit.pre_install(installer) }
post_install { |installer| PodKit.post_install(installer) }

project '…'

target '…' do
  pod '…', '~> 1.0.0'
end
```

## Directives

##### `group 'custom/path'`
Use custom location for CocoaPods's `Frameworks` and `Pods` groups, by default they will be added at the top level of Xcode project. You might want to keep it clean and store them at a different location.

##### `configure :target_type, 'base.xcconfig'`
Include base `*.xcconfig` Xcode build configuration per target type, for example, [xcconfigs](https://github.com/xcconfigs/xcconfigs). CocoaPods sets its own per-target configuration, so you no longer can use yours in Xcode project info. This directive allows to [include](https://pewpewthespells.com/blog/xcconfig_guide.html#includeStatements) own `*.xcconfig` files into ones generated by CocoaPods. Supported target types:

- `:application`
- `:dynamic_library`
- `:framework`
- `:static_library`
- `:test`
- `:xpc`

##### `prefix :phase, '[CP] '`
Set custom CocoaPods build phase prefix, handy when you want to completely remove it. Note, phases should use different prefixes, otherwise `:build_phase` will always get removed resulting in undefined behaviour. Use with caution! Supported phases:

- `:build_phase`
- `:user_build_phase`

## Installation

PodKit can be installed as a Git submodule:

```sh
git submodule add https://github.com/Bitwild/PodKit dependency/Git/PodKit
git submodule update --init --recursive
```
