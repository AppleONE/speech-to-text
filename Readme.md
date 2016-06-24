#### Getting Started
1. 우선 Cordova를 이용한 앱환경을 만든 후에 해당 폴더로 이동합니다.
>$ ionic start speech-to-text blank<br/>
$ cd speech-to-text

2. 플러그인 설치 [ 텍스트 읽어주기와 음성 문자화 ]
>$ cordova plugin add cordova-plugin-tts<br/>
$ cordova plugin add https://github.com/macdonst/SpeechRecognitionPlugin

3. index.html

```html
<body ng-app="starter">
    <ion-pane ng-controller="AppCtrl">
        <ion-header-bar class="bar-stable">
            <h1 class="title">Cordova Text-to-Speech</h1>
        </ion-header-bar>
        <ion-content class="padding">
            <div class="list list-inset">
                <label class="item item-input">
                    <i class="icon ion-speakerphone placeholder-icon"></i>
                    <input type="text" placeholder="Let me speak..." ng-model="data.speechText">
                </label>
            </div>
            <button class="button button-full button-positive" ng-click="speakText()"> Speak! </button>
            <button class="button button-full button-positive"ng-click="record()"> Record </button>
            <div class="card">
                <div class="item item-text-wrap"> {{recognizedText}} </div>
            </div>
        </ion-content>
    </ion-pane>
</body>
```

4. app.js  

~~~javascript
angular.module('starter', ['ionic'])

.controller('AppCtrl', function($scope) {
  $scope.data = {
    speechText: ''
  };
  $scope.recognizedText = '';

  $scope.speakText = function() {
    TTS.speak({
           text: $scope.data.speechText,
           locale: 'en-GB',
           rate: 1.5
       }, function () {
           // Do Something after success
       }, function (reason) {
           // Handle the error case
       });
  };

  $scope.record = function() {
    var recognition = new SpeechRecognition();
    recognition.onresult = function(event) {
        if (event.results.length > 0) {
            $scope.recognizedText = event.results[0][0].transcript;
            $scope.$apply()
        }
    };
    recognition.start();
  };
});
~~~~
