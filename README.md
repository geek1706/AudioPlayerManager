# HSAudioPlayer

[![Build Status](https://travis-ci.org/tschob/HSAudioPlayer.svg?branch=master)](https://travis-ci.org/tschob/HSAudioPlayer)
[![CocoaPods compatible](https://img.shields.io/cocoapods/v/HSAudioPlayer.svg?style=flat)](http://cocoadocs.org/docsets/HSAudioPlayer)
[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/tschob/HSAudioPlayer)
[![License](https://img.shields.io/cocoapods/l/HSAudioPlayer.svg?style=flat)](http://cocoadocs.org/docsets/HSAudioPlayer)
[![Platform](https://img.shields.io/cocoapods/p/HSAudioPlayer.svg?style=flat)](http://cocoadocs.org/docsets/HSAudioPlayer)

## Feature

HSAudioPlayer is a small audio player which takes care of the AVPlayer setup and usage. It uses an internal queue to play multiple items automatically in a row. All path based items which are supported from AVPlayer can be used (MPMediaItems and remote URLs).

## Requirements
- iOS 8+

## Installation

> **Embedded frameworks require a minimum deployment target of iOS 8 or OS X Mavericks (10.9).**

### CocoaPods

[CocoaPods](http://cocoapods.org) is a dependency manager for Cocoa projects. You can install it with the following command:

```bash
$ gem install cocoapods
```

To integrate HSAudioPlayer into your Xcode project using CocoaPods, specify it in your `Podfile`:

```ruby
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '8.0'
use_frameworks!

target '<Your Target Name>' do
    pod 'HSAudioPlayer'
end
```

Then, run the following command:

```bash
$ pod install
```

## Usage

There two basic usages:
- A singleton player which play one item at a time
```
HSAudioPlayer.sharedInstance.setup()
```

- Multiple player instances which can be used at the same time
```
let audioPlayer = HSAudioPlayer.audioPlayer()
```

### Setup

In both usage cases you need to `setup()` the player instance before using it.

```
		HSAudioPlayer.sharedInstance.setup()
```
This will setup the `AVAudioSession` playback plus activation state and initialize the remote control events plus now playing info center configuration. If you want to modify the basic settings you can this during or after calling `setup`.

```
HSAudioPlayer.sharedInstance.setup(useNowPlayingInfoCenter: false, useRemoteControlEvents: false)
```

```
HSAudioPlayer.sharedInstance.setup()
HSAudioPlayer.sharedInstance.playingTimeRefreshRate = 1.0
```

If you want to reveive remote control events you simply have to pass the events from the app delegate to the audio player instace

```
override func remoteControlReceivedWithEvent(event: UIEvent?) {
	HSAudioPlayer.sharedInstance.remoteControlReceivedWithEvent(event)
}
```

### Playback

HSAudioPlayer can play local `MPMediaItem`s and stream items from a remote URL. You can pass either one or multiple items to the player.

The following line will replace the current queue of the audio player with the chosen item. The playback will stop automatically if the item was played.
```
HSAudioPlayer.sharedInstance.play(url: self.trackUrl)
```

If you want to play multiple items you can pass an array and start position. The audio player will replace the current queue with the given array and jump right to the item at the given position. The queue allows the user to rewind also to items with a lower index than the start position.

```
let songs = (MPMediaQuery.songsQuery().items ?? [])
HSAudioPlayer.sharedInstance.play(mediaItems: songs, startPosition: 5)
```


## Author

tschob, Hans Seiffert

## License

HSAudioPlayer is available under the [MIT license](https://github.com/tschob/HSAudioPlayer/blob/master/LICENSE).