# obs_websocket_dart

I needed a way to connect to [OBS](https://obsproject.com/) (Open Broadcast Software) that was easy to use and allowed me to fully control it. Endded up using this [repository](https://github.com/faithoflifedev/obsWebsocket) as a base to make my own package.

This dart package allow you to connect to [OBS](https://obsproject.com/) using the [obs-websocket](https://github.com/Palakis/obs-websocket) plugin.

## Getting Started

In your flutter project pubspec.yaml add the dependency:

```yml
dependencies:
  ...
  obs_websocket_dart: ^0.0.2
```

## Example

Import the library, create the object, set the data and call the connect function. Be shure to have the [obs-websocket](https://obsproject.com/forum/resources/obs-websocket-remote-control-obs-studio-from-websockets.466/) plugin installed in your OBS instance.

Connection with password:

```dart
import 'package:obs_websocket_dart/obs_websocket_dart.dart';

ObsWebsocket obs = new ObsWebsocket();
obs.setAddress('127.0.0.1');
obs.setPort('4444');
obs.setPassword('*****');
await obs.connect();
```

Connection without password:

```dart
import 'package:obs_websocket_dart/obs_websocket_dart.dart';

ObsWebsocket obs = new ObsWebsocket();
obs.setAddress('127.0.0.1');
obs.setPort('4444');
await obs.connect();
```

## Sending Commands

The commands can be found on the [protocol](https://github.com/Palakis/obs-websocket/blob/4.x-current/docs/generated/protocol.md) page of the [obs-websocket](https://github.com/Palakis/obs-websocket) github page.

```dart
obs.send('GetCurrentScene');

obs.send('SetPreviewScene', {"scene-name": "newScene"});
```

This function returns a Future, so in case of commands that are expeced to return some information can be used this way

```dart
obs.send('GetPreviewScene').then(event) {
    for (dynamic scene in event['scenes']) {
        print('SCENE ' + scene['name']);
        for (dynamic source in scene['sources']) {
            print('SOURCE ' + source['name']);
        }
    }
});
```

## Listening to Events

The events can be found on the [protocol](https://github.com/Palakis/obs-websocket/blob/4.x-current/docs/generated/protocol.md) page of the [obs-websocket](https://github.com/Palakis/obs-websocket) github page.

This function returns a stream. This is one of the ways to use streams in dart.

```dart
obs.event('StreamStatus').listen((event) {
    print('StreamStatus: ' + event['stream-timecode'].toString() + ' at ' + event['kbits-per-sec'].toString() + 'Kb/s');
});
```
