# Setup Steps from Tutorial 11 to 12
This guide will show you the steps to perform if you are working from Tutorial 11 source code.

# Components to install
We need to update the Firebase App module.

```
yarn upgrade @react-native-firebase/app --latest
yarn upgrade @react-native-firebase/auth --latest
yarn upgrade @react-native-firebase/messaging --latest
yarn add @react-native-firebase/firestore

cd ios
pod update Firebase/Messaging
pod install --repo-update
cd ..

cd android && ./gradlew clean && cd ..
```

# Setup FireStore
- Create a new Database and select the location
- Edit the rule to the following to enalbe user access

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null && request.auth.uid != null;
    }
  }
}
```
- Create Indexes for sort the result based on addedOn desc.
```
Collection ID: FavWord
Fields Indexed: userId Ascending, addedOn Descending
Query Scope: Collection
```

# iOS 14 Bugfix
Due to the update from iOS 13 to 14, this cause a bug in displaying image in the app. To solve the issue, edit the /node_modules/react-native/Libraries/Image/RCTUIImageViewAnimated.m file to replace the displayLayer method as shown below.

```
- (void)displayLayer:(CALayer *)layer
{
  if (!_currentFrame) {
    _currentFrame = self.image;
  }

  if (_currentFrame) {
    layer.contentsScale = self.animatedImageScale;
    layer.contents = (__bridge id)_currentFrame.CGImage;
  }
}
```
