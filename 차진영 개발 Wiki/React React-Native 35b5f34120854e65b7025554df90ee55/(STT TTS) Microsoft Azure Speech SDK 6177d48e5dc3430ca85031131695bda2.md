# (STT/TTS) Microsoft Azure Speech SDK

### 키 발급

- [Microsoft Azure](https://azure.microsoft.com/ko-kr) 접속 ⇒ **Azure** 계정 생성
- **Speech-To-Speech** / **Text-To-Speech** 구독 및 지역 선택 (**Korea Central**)
- **key** 및 **region**를 복사하여 프로젝트에 적용 (**.env**등 외부 파일에 적용하여 **key 숨김** 필수)

### 설치

- `npm i microsoft-cognitiveservices-speech-sdk`

### 사용방법

STT와 TTS는 각각 설정에 필요한 변수명이 달라 확인 필수 (**STT**: recognize / **TTS**: synthesize)

1. `speechsdk.SpeechConfig.fromSubscription(KEY, REGION)`으로 speech SDK를 불러온다.
2. `speechConfig.speechVoiceName`에 적용할 언어를 선택한다.
3. `speechsdk.AudioConfig`에 적용할 SDK를 호출한다. (**STT** or **TTS**)
4. `new speechsdk.SpeechRecognizer(speechConfig, audioConfig)`를 통해 1번과 3번에서 선언한 config를 적용한 **Synthesizer**를 생성한다.
5. 수행을 완료하면 `close`로 SDK를 종료한다.
- **STT**의 음성은 마이크 기능을 사용하기 때문에 **마이크 권한 허용**이 필요하다.

### STT

```tsx
  import { ResultReason } from "microsoft-cognitiveservices-speech-sdk";
  
  const speechsdk = require("microsoft-cognitiveservices-speech-sdk");

  const [chat, setChat] = useState("")
  
  const playSTT = async () => {
    const speechConfig = speechsdk.SpeechConfig.fromSubscription(
      process.env.REACT_APP_AZURE_KEY,
      process.env.REACT_APP_AZURE_REGION
    )
    speechConfig.speechRecognitionLanguage = "ko-KR"

    const audioConfig = speechsdk.AudioConfig.fromDefaultMicrophoneInput()
    const recognizer = new speechsdk.SpeechRecognizer(
      speechConfig,
      audioConfig
    )

    recognizer.recognizeOnceAsync((result: any) => {
      if (result.reason === ResultReason.RecognizedSpeech) {
        setChat(result.text)
      }
      recognizer.close()
    });
  };
```

**AudioConfig**

- **wav 파일**로 인식: `fromWavFileInput(fs.readFileSync("audioFile.wav")`
    
    ⇒ `const fs = require('fs')` 필요
    
- **마이크**로 인식: `fromDefaultMicrophoneInput()`

**SpeechRecognizer.reason**

- `ResultReason.RecognizedSpeech`: STT를 성공적으로 인식하여 반환된 값
- `ResultReason.NoMatch`: 음성과 매치된 결과를 찾을 수 없음
- `ResultReason.Canceled`: STT가 취소되었음
    - `CancellationDetails.fromResult(result)`: 해당 과정의 STT의 실패정보를 불러온다
    - `reason: CancellationReason.Error`: STT 수행 과정에서 에러가 발생

**setProperty**

- `PropertyId.SpeechServiceConnection_InitialSilenceTimeoutMs`: 음성이 감지되지 않았을 때 STT가 종료되기까지의 대기시간 (ms)

### **TTS**

```tsx
 import { ResultReason, SynthesisResult } from "microsoft-cognitiveservices-speech-sdk";
 
 const speechsdk = require("microsoft-cognitiveservices-speech-sdk");
   
 const [chat, setChat] = useState("hello world")
 
 const playTTS = async () => {
	 const speechConfig = speechsdk.SpeechConfig.fromSubscription(
        process.env.REACT_APP_AZURE_KEY,
        process.env.REACT_APP_AZURE_REGION
      );
      speechConfig.speechSynthesisVoiceName = "선택한 음성";

      const audioConfig = speechsdk.AudioConfig.fromDefaultSpeakerOutput();
      const synthesizer = new speechsdk.SpeechSynthesizer(
        speechConfig,
        audioConfig
      );
      

      synthesizer.speakTextAsync(chat, 
        (result: SynthesisResult) => {
          if (
            result.reason === speechsdk.ResultReason.SynthesizingAudioCompleted
          ) {
            synthesizer.close();
          }
        },
        (error: string) => {
          console.error(error);
          synthesizer.close();
          synthesizer = null;
        }
      );
    }
 }
```

`speechConfig.speechSynthesisVoiceName`를 통해 음성을 선택할 수 있다.

- 지원 음성은 브라우저 별로 다르다. ⇒ [Microsoft Azure Speech SDK 지원 언어 및 음성](https://learn.microsoft.com/ko-kr/azure/ai-services/speech-service/language-support?tabs=stt#prebuilt-neural-voices)

**SpeechSynthesizer**는 **string**과 **ssml** 두 가지 방법이 있다.

- **string:** `speakTextAsync`에 parameter로 TTS를 실행할 텍스트를 받는다.
- **ssml**
    
    ```jsx
    const ssml = `
      <speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
        <voice name="${speechConfig.speechSynthesisVoiceName}">
          <prosody rate="1.2">
            ${chat}
          </prosody>
        </voice>
      </speak>
    `;
    ```
    
    **ssml**을 통해 음성 종류와 빠르기, **TTS**를 실행할 텍스트 등의 옵션을 한 번에 결정할 수 있다.
    
    - `style`: 말하기 스타일 (밝고 낙관적, 슬픈 억양 등)
    - `styledegree`: 말하기 강도 (0.01 ~ 2)
    - `role`: 역할 (어린 아이 목소리, 노인 목소리 등을 흉내낸다)
    
    `speakSsmlAsync`의 parameter로 위에서 생성한 **ssml**을 받는다.
    

### 참고

- [Microsoft Azure Speech SDK](https://learn.microsoft.com/ko-kr/azure/ai-services/speech-service/speech-sdk)