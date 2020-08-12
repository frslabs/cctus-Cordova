## VIDUS - Cordova Plugin

### I. Introduction

The FRS Video SDK has APIs to capture interactive realtime selfie video with customizable actions. Each functionality is separated into unique `nodes`. The APIs are contained in the following  input nodes

##### 1. Simple Recorder Node

Captures a video recording with a user defined time

##### 2. Challenge Code Node

Captures a video recording which will include a user-read 4 digit code prompted by a customizable machine-read question

##### 3. Challenge Text Node

Captures a video recording which will include a user-prompted answer with a customizable machine-read question

##### 4. Declaration Node

Captures a video recording which will include a user-read or machine-read text/declaration

### II. Integration steps

#### 1. Adding plugin to the app

- Extract the plugin ZIP file `VIDUS.zip`
- Update the maven credentials in `/path/to/VIDUS/src/android/VIDUS.gradle` file
- Add the plugin to your app

    `cordova plugin add /path/to/VIDUS`
    
#### FOR IOS
    
    - Add the swift support to cordova app :  `cordova plugin add cordova-plugin-add-swift-support --save`
    
    - Add permissions to your app to capture video and audio
    
    `cordova plugin add cordova-plugin-ios-camera-permissions --variable CAMERA_USAGE_DESCRIPTION="To record the video" --variable MICROPHONE_USAGE_DESCRIPTION="To record the audio"`
    
    
###### Save/Edit Netrc settings to install custom pod

You will need a valid netrc credentials to install forus from maven, which can be obtained by contacting `support@frslabs.com`. 

1. Create or edit .netrc file under current user's home directory
2. Write the below lines into that file, replace <YOUR_USERNAME> and <YOUR_PASSWORD> with your credentials which is shared through email and save the file.
```ruby
machine vidus-sdk-ios.repo.frslabs.space
login <YOUR_USERNAME>
password <YOUR_PASSOWRD>
```
3. In terminal enter below command to install the pod.
    
    
   - After adding the plugin you need to install the sdk through pod 
    1: first check with this command to see if all the details are added correctly
                `open podfile` 
    2: It should display the following details:

        
        ```Javascript
        source 'https://gitlab.com/frslabs-public/ios/vidus.git'
        
        source 'https://github.com/CocoaPods/Specs.git'
         
         platform :ios, '11.0'       //If changes are needed to platform ios version, you can change it here
         
         use_frameworks!
         
         target 'appios' do
             project 'appios.xcodeproj'
             pod 'VIDUS', '1.1.0'
             pod 'Alamofire', '~> 4.9.1'
         end
    
    3: Use this command to install :  `pod install`

#### 2. Usage OF ANDROID 

- Load the plugin
  
    ```Javascript
    VideoSDKPlugin = cordova.require("com.frslabs.cordova.plugin.vidus.VIDUS");
    ```

- Initialize VideoSDKSettings
  
    ```Javascript
    var showInstructionFlag = true;
    var showPreview = false;
    var showRerecord = false;
    var apiBaseUrl = "BASE_UR";
    var keyId = "KEY_ID";
    var keySecret = "KEY_SECRET";

    VideoSDKPlugin.VideoSDKSettings = new VideoSDKPlugin.VideoSDKSettings(PUT_YOUR_LICENCE_KEY,showInstructionFlag,showPreview,showRerecord,apiBaseUrl,keyId,keySecret);
    ```
- Add a node
  
    ```Javascript
    // This unique integer id will be used to get results for the corresponding input node
    var NODE_ID_SIMPLE_RECORDER = 100;

    VideoSDKPlugin.VideoSDKSettings.addNode(NODE_ID_SIMPLE_RECORDER,new VideoSDKPlugin.SimpleRecorderNode().setVideoRecordTime(3));
    ```

- Add multiple nodes
  
    ```Javascript
    var NODE_ID_CHALLENGE_CODE = 101;
    var NODE_ID_CHALLENGE_TEXT = 102;
    var NODE_ID_DECLARATION_MACHINE = 103;
    var NODE_ID_DECLARATION_USER = 104;

    VideoSDKPlugin.VideoSDKSettings.addNode(NODE_ID_CHALLENGE_CODE,new VideoSDKPlugin.ChallengeCodeNode()
                                                                    .setVideoChallengeCodeText("Please tell verification number")
                                                                    .setVideoChallengeCodeVoiceType(VideoSDKPlugin.VOICE_TYPE_FEMALE))

                        .addNode(NODE_ID_CHALLENGE_TEXT,new VideoSDKPlugin.ChallengeTextNode()
                                                                    .setVideoChallengeText("Please tell your date of birth")
                                                                    .setVideoChallengeTextTime(5)
                                                                    .setVideoChallengeTextVoiceType(VideoSDKPlugin.VOICE_TYPE_MALE))  
                                                                                    
                        .addNode(NODE_ID_DECLARATION_MACHINE,new VideoSDKPlugin.DeclarationNode()
                                                                    .setVideoDeclarationText("I certify that the information provided is true and complete to the best of my knowledge.")
                                                                    .setVideoDeclarationSpokenMethod(VideoSDKPlugin.SPOKEN_BY_MACHINE)
                                                                    .setVideoDeclarationVoiceType(VideoSDKPlugin.VOICE_TYPE_FEMALE))

                        .addNode(NODE_ID_DECLARATION_USER, new VideoSDKPlugin.DeclarationNode()
                                                                    .setVideoDeclarationText("I certify that the information provided is true and complete to the best of my knowledge.")
                                                                    .setVideoDeclarationSpokenMethod(VideoSDKPlugin.SPOKEN_BY_USER)
                                                                    .setVideoDeclarationVoiceType(VideoSDKPlugin.VOICE_TYPE_MALE));
    ```
 #### 3. Usage OF IOS 

    - Adding single node
    ```Javascript
        // This unique integer id will be used to get results for the corresponding input node
        var inputParamsDict = {};
        var simpleRecorderNode = 'Simple Recorder Node';
        inputParamsDict['ios_nodeNameArray'] = [simpleRecorderNode];
        var addSimpleRecorderDict = {'nodeName': simpleRecorderNode, 'timeDuration': '2'};
        inputParamsDict['ios_inputParams'] = [addSimpleRecorderDict];
        inputParamsDict['ios_recordingType'] = 'screen';
        inputParamsDict['ios_licenceKey'] = 'your licence key';
    ```

- Add multiple nodes
    ```Javascript
        var inputParamsDict = {};
        var simpleRecorderNode = 'Simple Recorder Node';
        var challengeCodeNode = 'Challenge Code Node';
        var challengeTextNode = 'Challenge Text Node';
        var declarationNode = 'Declaration Node';
        var osvRecorderNode = 'OSV Recorder Node';
        var osvChallengeTextNode = 'OSV Challenge Text Node';
        inputParamsDict['ios_nodeNameArray'] = [simpleRecorderNode,challengeCodeNode.challengeTextNode,declarationNode,osvRecorderNode,osvChallengeTextNode];
                
        var addSimpleRecorderDict = {'nodeName': simpleRecorderNode, 'timeDuration': '2'};
        var addChallengeCodeDict = {'nodeName': challengeCodeNode,'keySecret': 'KEY_SECRET', 'keyId': 'KEY_ID',  'challengeText': 'Sample_Text'};
        var addChallengeTextNodeDict = {'nodeName': challengeTextNode, 'timeDuration': '8',  'challengeText': 'Sample Text'};
        var addDeclarationNodeDict = {'nodeName': declarationNode,'voiceType': 'Spoken by Machine', 'challengeText': 'Sample Text'};
        var addOSVrecorderDict = {'nodeName': osvRecorderNode, 'timeDuration': '8'};
        var addOSVChallengeTextDict = {'nodeName': osvChallengeTextNode, 'timeDuration': '8', 'challengeText': 'sample text'};
        inputParamsDict['ios_inputParams'] = [addSimpleRecorderDict,addChallengeCodeDict,addChallengeTextNodeDict,addDeclarationNodeDict,addOSVrecorderDict,addOSVChallengeTextDict];
        inputParamsDict['ios_recordingType'] = 'screen';
        inputParamsDict['ios_licenceKey'] = 'your licence key';
    ```

- Create callback methods

  
    ```Javascript
    var successCallBack = function success(result){
            console.info(result);

            // You can get the result of each node by the integer ID number passed in the settings.

            var simpleRecorderNodeResult = result[NODE_ID_SIMPLE_RECORDER];
            var challengeCodeNodeResult = result[NODE_ID_CHALLENGE_CODE];
            var challengeTextNodeResult = result[NODE_ID_CHALLENGE_TEXT];
            var DeclarationByMachineNodeResult = result[NODE_ID_DECLARATION_MACHINE];
            var DeclarationByUserNodeResult = result[NODE_ID_DECLARATION_USER];

            // Accessing the individual node result objects
            console.info(simpleRecorderNodeResult.startTime);
            console.info(challengeCodeNodeResult.codeRecordStartTime);

    }

    ```


    ```Javascript
    var failureCallBack = function error(result){
            console.info(result);
    }
    ```    


- Invoke ANDROID plugin

    ```Javascript
    VideoSDKPlugin.invokeFRSVideoSDK(VideoSDKPlugin.VideoSDKSettings, successCallBack, failureCallBack);
   
    ```
    
- Invoke IOS plugin

    ```Javascript
        VIDUS.invokeVidusSDK(inputParamsDict,successCallback,errorCallback);
    `

- Sample success reponse    

    ```Javascript
        {   
            "100": {
                "id": 100,
                "startTime": 0,
                "stopTime": 3
            },
            "101": {
                "codeRecordStartTime": 10,
                "id": 101,
                "startTime": 7,
                "stopTime": 15,
                "videoChallengeCode": "2-3-5-7",
                "videoChallengeCodeMode": 1,
                "videoChallengeCodeText": "Please tell verification number"
            },
            "102": {
                "id": 102,
                "startTime": 19,
                "stopTime": 27,
                "textRecordStartTime": 22,
                "videoChallengeText": "Please tell your date of birth"
            },
            "103": {
                "id": 103,
                "startTime": 31,
                "stopTime": 46,
                "videoDeclarationSpokenMethod": 2,
                "videoDeclarationText": "I certify that the information I am provided is true and complete to the best of my knowledge."
            },
            "104": {
                "id": 104,
                "startTime": 51,
                "stopTime": 66,
                "videoDeclarationSpokenMethod": 1,
                "videoDeclarationText": "I certify that the information I am provided is true and complete to the best of my knowledge."
            },
            "videoImagePath": "/data/data/com.nous.inglifeio.cordova.hellocordova/files/videosdk/video-1551249277243.mp4"
        }
    ```

- Sample failure response

    ```Javascript
        {
            "error": "Activity closed unexpectedly"
        }
    ```
## Vidus IOS Error Codes

Following error codes will be returned on the `onVidusFailure` method of the callback

| CODE | DESCRIPTION                  |
| ---- | ---------------------------- |
| 803  | Camera Permission deny             |
| 804  | SDK is interrupted       |
| 805  | Vidus SDK license expire            |
| 806  | Vidus SDK license is invalid |
| 807  | Invalid Config         |
| 808  | Transaction fail       |
| 809  | Internet is not available             |
| 810  | Screen recording permision deny             |
| 812  | Upload audio to server fail           |
| 813  | Microphone permission deny            |
| 814  | Upload screen images fail            |
| 815  | Secret number verification fail            |
| 818  | Upload screen recorded video fail           |
| 819  | Timeout             |
| 820  | Minimum time duration for recording is not set            |
| 821  | Recording Video is Failed            |

