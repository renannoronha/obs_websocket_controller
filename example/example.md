```dart
// import package
import 'package:obs_websocket_dart/obs_websocket_dart.dart';

// instanciate the class
ObsWebsocket obs = new ObsWebsocket();

// set debug level
// 0 -> nothing
// 1 -> what's happening
// 2 -> every send response
// 3 -> everything
obs.setDebug(1);

// pass the necessary data for the connection
obs.setAddress('127.0.0.1');
obs.setPort('4444');
obs.setPassword('*****');

// call the connect funcion and it does the rest for you
await obs.connect();

// get list of scenes, response as a variable
var getSceneList = await obs.send('GetSceneList');
for (dynamic scene in getSceneList['scenes']) {
    print(scene['name']);
}

// get list of scenes, response as a future
obs.send('GetSceneList').then((getSceneList) {
    for (dynamic scene in getSceneList['scenes']) {
        print(scene['name']);
    }
});

// listen to obs events, return a stream
obs.event('StreamStatus').listen((event) {
    print('StreamStatus: ' + event['stream-timecode'].toString() + ' at ' + event['kbits-per-sec'].toString() + 'Kb/s');
});

```
