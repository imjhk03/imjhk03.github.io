---
title: iOS에서 카메라 캡처
layout: post
tags: [UIKit, Camera, Photo, AVFoundation]
---

![https://images.unsplash.com/photo-1486962532485-55d6645c218e?q=80&w=3870&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D](https://images.unsplash.com/photo-1486962532485-55d6645c218e?q=80&w=3870&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D)
_Image from Unsplash_ 

사이드 프로젝트 진행하면서 Vision Framework을 다뤄봤는데, 자연스럽게 카메라 기능에 대한 기술도 접하게 되었다. 단순하게 시스템 카메라 UI를 사용할 수 있고 직접 카메라 UI를 구현할 수 있었는데, 사이드 프로젝트 특성에 따라 자체 커스텀 카메라 UI를 구현하게 되었다.

이 블로그 포스트는 `AVFoundation` 사용해서 자체 커스텀 카메라 UI를 어떻게 구현하는지, 대략적인 `AVFoundation` 프레임워크 설명이랑 캡처 아키텍처 등을 다룰 것이다.

>이 블로그 포스트의 코드는 iOS 환경, UIKit 기반으로 설명되어 있습니다. 관련 내용은 macOS와 동일하게 적용될 수 있습니다.
{: .prompt-info }

## Overview
아이폰 카메라를 사용하여 사진이나 영상을 찍을 수 있으며, `AVFoundation`을 사용하여 카메라 캡처 기능을 구현할 수 있다. `AVFoundation`은 시청각 미디어 작업을 위한 모든 기능을 갖춘 프레임워크로, iOS 및 macOS에서 비디오, 사진 및 오디오 캡처 서비스를 제공한다.

`AVFoundation`은 자체 커스텀 카메라 UI를 사용하여 카메라 관련 초점, 노출, 안정화 옵션 등을 직접 제어할 수 있게 해준다. 반면, 단순하고 쉽게 시스템 카메라 UI를 사용해서 사진이나 영상을 찍는 기능을 원한다면 `UIImagePickerController`를 사용할 수 있다. `UIImagePickerController`는 전면 카메라로 전환, 플래시 설정 등 기본적인 기능들을 제공하여 손쉽게 사용할 수 있다. 앱의 특성에 따라 적절한 프레임워크를 선택하면 된다.

|     | UIImagePickerController                                      | AVFoundation                                                 |
|-----|--------------------------------------------------------------|--------------------------------------------------------------|
| 사용법 | * 사용하기 쉬운 고수준 API로, 기본적인 사진 및 비디오 캡처 기능을 제공<br>* 코드 몇 줄로 사진을 찍거나 사진 라이브러리에서 이미지를 선택 가능 | * 비디오 및 오디오 캡처, 재생, 편집, 파일 변환 등 다양한 미디어 작업을 수행할 수 있음<br>* 실시간으로 미디어 스트림에 접근하여 고급 기능(실시간 필터 적용, 얼굴 인식 등)을 구현할 수 있음 |
| UI  | * iOS에서 제공하는 기본적인 사용자 인터페이스를 사용<br>* 커스터마이징이 제한적이며 기본 제공 UI 외의 기능을 추가하기 어려움 | * 다양한 설정 및 커스터마이징 옵션을 제공하여 세밀한 제어가 가능<br>* 다양한 미디어 형식 및 해상도를 지원하고, 카메라와 마이크의 다양한 설정을 조정할 수 있음 |
| 단점  | * 단순한 사진 및 비디오 캡처 및 선택 기능만 제공<br>* 고급 기능이나 설정(특정 해상도 설정, 실시간 필터 적용 등)을 사용할 수 없음 | * 저수준 API를 사용하므로 초기 설정과 구현이 복잡할 수 있음<br>* 사용자는 세션, 입력, 출력, 프레임 버퍼 등을 직접 설정하고 관리해야 함 |

## 캡처 아키텍처
`AVFoundation` 프레임워크를 사용해서 이미지(혹은 비디오) 캡처 구현할 때, 캡처 아키텍처에서 주요한 부분이 바로 세션, 입력과 출력이다. 캡처 세션은 입력 장치에서 캡처 출력을 연결하고 캡처 동작을 구성하고 데이터의 흐름을 조정한다. 입력은 카메라나 마이크와 같은 하드웨어 장치에서 데이터를 제공하며 출력은 입력에서 제공한 데이터를 가공하여 종류에 따라 데이터를 생성한다. 예를 들어 PhotoOutput 같이 고사양 사진이나 Live Photo들을, VideoDataOutput이나 AudioDataOutPut은 비디오 또는 오디오 버퍼를 앱으로 전달한다.

![Camera Capture Architecture](/assets/img/2024/08/14/image1.png)
_From Apple Developer_

## 필수 항목
아이폰과 같은 기기에서 카메라랑 마이크를 사용하기 위해서는 접근 권한이 꼭 필요하다. 앱이 처음으로 캡처 디바이스를 사용하기 전에 시스템에서 사용자가 지정한 앱 메시지가 포함된 알림을 통해 사용자에게 앱에 캡처 디바이스에 대한 접근 권한을 부여하도록 요청한다.

접근 권한에 대한 설정은 기억하기 때문에 나중에 해당 캡처 디바이스를 사용해도 알림이 다시 표시되지 않는다. 사용자는 iOS의 경우 설정 > 개인정보 보호에서 앱의 권한 설정을 변경할 수 있다.

### 접근 권한 알림 설정
카메라와 마이크에 대한 접근 권한은 Info.plist 파일에 키를 추가하여 접근 권한에 대한 메시지를 작성할 수 있다.
* 카메라 사용 접근 권한 키는 [NSCameraUsageDescription](https://developer.apple.com/documentation/bundleresources/information_property_list/nscamerausagedescription)
* 마이크 사용 접근 권한 키는 [NSMicrophoneUsageDescription](https://developer.apple.com/documentation/bundleresources/information_property_list/nsmicrophoneusagedescription)

![Info.plist file settings](/assets/img/2024/08/14/image2.png)

카메라를 사용하기 전에 접근 권한 상태를 확인하고 사용하는 것이 좋다. [AVCaptureDevice](https://developer.apple.com/documentation/avfoundation/avcapturedevice) [authorizationStatus\(for:\)](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1624613-authorizationstatus) 메서드를 사용해서 확인하고, 만약 접근 권한이 거부되어 있다면 [requestAccess\(for:completionHandler:\)](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1624584-requestaccess) 메서드를 사용하여 사용자에게 다시 접근 권한에 대한 알림을 띄울 수 있다. 앱이 만약 필수적으로 카메라를 사용하지 않고 카메라 기능을 사용하는 경우가 따로 있다면, 그때 접근 권한을 체크하는 방법도 있다. 자세한 내용은 [애플 개발자 사이트](https://developer.apple.com/documentation/avfoundation/capture_setup/requesting_authorization_to_capture_and_save_media#2958841)에서 참고하면 좋다.

### 미디어 캡처 저장 권한
카메라나 마이크를 통해 캡처한 미디어를 사진 라이브러리에 저장하고 싶다면 사진 라이브러리에 대한 접근 권한이 꼭 필요하다. 미디어의 종류에 따라 접근 권한이 다르다. 접근 권한에 따라 사용하는 클래스도 다른데, 자세한 내용은 [애플 개발자 사이트](https://developer.apple.com/documentation/avfoundation/capture_setup/requesting_authorization_to_capture_and_save_media#2958849)에서 참고하면 좋다.

## 카메라 캡처 설정하기
우선 입력 디바이스에서 미디어 출력으로의 데이터 흐름을 관리하는 미디어 캡처의 기본인 `AVCaptureSession`을 먼저 준비한다.
```swift
let session = AVCaptureSession()
```

모든 캡처 세션은 최소한 하나의 캡처 입력과 캡처 출력이 필요하다. 캡처 입력은 미디어 소스인 카메라 혹은 마이크 같은 하드웨어이고 캡처 출력은 캡처 입력이 제공한 데이터를 사용해서 사진이나 동영상 파일들을 만든다.

입력을 담당하는 `AVCaptureDevice`를 설정하고 `AVCaptureDeviceInput`으로 한번 감싼 다음에 세션에 추가한다. `AVCaptureDevice`에서 여러 가지 카메라 디바이스를 선택할 수 있다. 여기서는 `AVCaptureDevice.default`를 사용해서 지원되는 디바이스의 듀얼 카메라 또는 싱글 카메라 디바이스에서 싱글(광각) 카메라 중 가장 적합한 후면 카메라를 선택한다.

```swift
let captureDevice: AVCaptureDevice
if let device = AVCaptureDevice.default(.builtInDualCamera, for: .video, position: .back) {
    print("Using built in dual camera")
    captureDevice = device
} else if let device = AVCaptureDevice.default(.builtInWideAngleCamera, for: .video, position: .back) {
    print("Using built in wide angle camera")
    captureDevice = device
} else {
    fatalError("Missing expected back camera device.")
}

guard let deviceInput = try? AVCaptureDeviceInput(device: captureDevice) else {
    print("Could not create device input.")
    return
}
if captureSession.canAddInput(deviceInput) {
    captureSession.addInput(deviceInput)
}
```

iOS는 카메라 디바이스를 선택하는 방법이 `AVCaptureDevice.default` 와 다른 방법이 있는데 [Choosing a Capture Device](https://developer.apple.com/documentation/avfoundation/capture_setup/choosing_a_capture_device)에 자세히 설명되어 있다.

적절한 카메라 디바이스를 설정했다면, 캡처할 미디어의 종류를 결정하여 출력을 설정하고 세션에 추가한다. 예를 들어 사진을 캡처하고 싶다면 `AVCapturePhotoOutput`을 세션에 더한다.

```swift
let photoOutput = AVCapturePhotoOutput()
guard captureSession.canAddOutput(photoOutput) else { return }
captureSession.sessionPreset = .photo
captureSession.addOutput(photoOutput)
captureSession.commitConfiguration()
```

만약 비디오를 캡처하고 싶다면 `AVCaptureVideoDataOutput`을 세션에 더한다.
```swift
// 실시간 성능을 유지하기 위해 늦게 도착한 비디오 프레임을 폐기하도록 출력 설정
videoDataOutput.alwaysDiscardsLateVideoFrames = true
// 1 비디오 출력에서 샘플 버퍼를 처리할 delegate와 queue를 설정
videoDataOutput.setSampleBufferDelegate(self, queue: videoDataOutputQueue)
// 2 비디오 출력에 특정 픽셀 형식 타입(420YpCbCr8 Bi-Planar Full Range)을 사용하도록 구성
videoDataOutput.videoSettings = [
    kCVPixelBufferPixelFormatTypeKey as String: kCVPixelFormatType_420YpCbCr8BiPlanarFullRange
]
if captureSession.canAddOutput(videoDataOutput) {
    captureSession.addOutput(videoDataOutput)
} else {
    print("Could not add VDO output")
    return
}
```

위 코드에서 1번과 2번에 대해서 조금 더 부연 설명이 필요하다.

1. [setSampleBufferDelegate\(_:queue:\)](https://developer.apple.com/documentation/avfoundation/avcapturevideodataoutput/1389008-setsamplebufferdelegate) 메서드는 비디오 출력에 대한 샘플 버퍼를 처리할 델리게이트를 설정한다. [AVCaptureVideoDataOutputSampleBufferDelegate](https://developer.apple.com/documentation/avfoundation/avcapturevideodataoutputsamplebufferdelegate) 프로토콜을 채택하는 객체가 샘플 버퍼를 수신할 것이고, 큐는 샘플 버퍼 처리 작업이 실행될 스레드를 결정한다. 비디오 프레임 처리는 시간이 많이 걸릴 수 있기 때문에 메인 큐 대신 백그라운드 큐를 사용하는 것이 일반적이다. 이 큐의 역할은 다음과 같다.
* **비디오 프레임 처리**: 샘플 버퍼 델리게이트 메서드는 이 큐에서 실행된다
* **작업 분리**: 메인 스레드의 작업과 비디오 프레임 처리 작업을 분리하여 UI 응답성을 유지한다.


    이 큐는 `videoDataOutput.alwaysDiscardsLateVideoFrames` 프로퍼티와 관련되어 있다. 백그라운드 큐에서 프레임을 처리하는 동안, 처리 속도가 비디오 프레임 캡처 속도보다 느리면 프레임이 대기열에 쌓이게 된다. 그래서 `alwaysDiscardsLateVideoFrames`가 `true`로 설정되어 있으면, 대기 중인 오래된 프레임은 자동으로 폐기되어 최신 프레임만 처리된다. 실시간 비디오 스트리밍이나 실시간 영상 처리가 필요한 경우, 최신 프레임을 우선 처리하여 지연을 최소화할 수 있다.

    델리게이트를 설정하면 메서드도 함께 추가한다. 이 `AVCaptureVideoDataOutputSampleBufferDelegate`는 샘플 버퍼가 준비될 때 호출되는 메서드를 제공한다. 메서드를 이용해서 샘플 버퍼를 사용하여 비디오 프레임을 처리할 수 있다. 예를 들어 이미지 인식이나 비디오 필터링 작업을 수행할 수 있다.

    ```swift
    // MARK: - AVCaptureVideoDataOutputSampleBufferDelegate
    extension ViewController: AVCaptureVideoDataOutputSampleBufferDelegate {

        func captureOutput(_ output: AVCaptureOutput, didOutput sampleBuffer: CMSampleBuffer, from connection: AVCaptureConnection) {
		    // 샘플 버퍼 처리 코드
        }

    }
    ```

2. `videoDataOutput.videoSettings`에서 비디오 출력에 대한 설정을 하는 부분인데, 여기서는 픽셀 형식을 설정했다. `kCVPixelBufferPixelFormatTypeKey`는 픽셀 버퍼의 형식을 지정하는 키이고, `kCVPixelFormatType_420YpCbCr8BiPlanarFullRange`는 픽셀 형식의 값이다. 작업하고 있는 앱 특성상 실시간으로 카메라를 이용해서 텍스트를 인식하고 Vision Framework을 이용해서 텍스트를 추출하는 작업이 필요하여 고품질의 비디오 캡처와 후처리에 적합한 것으로 설정했다. 다양한 픽셀 형식이 있는데 깊고 자세한 내용은 [TN3121: Selecting a pixel format for an AVCaptureVideoDataOutput](https://developer.apple.com/documentation/technotes/tn3121-selecting-a-pixel-format-for-an-avcapturevideodataoutput)에서 확인하면 좋다.

여기까지 캡처 세션에 필요한 입력과 출력을 설정했다. 다음은 사용자가 카메라의 입력 화면을 실시간으로 보여주는 화면 작업이 필요하다.

## 카메라 미리보기 화면
사용자가 사진을 찍거나 동영상 녹화를 시작하기 전에 카메라의 입력을 볼 수 있도록 미리보기 화면을 만드는 것이 중요하다. 세션이 실행 중일 때마다 카메라의 실시간 비디오 피드를 표시하는 `AVCaptureVideoPreviewLayer`를 캡처 세션에 연결하여 이러한 미리보기를 제공할 수 있다.

`AVCaptureVideoPreviewLayer`는 Core Animation layer이므로 다른 `CALayer` 서브클래스와 마찬가지로 인터페이스에서 표시하고 스타일을 지정할 수 있다. `UIKit` 앱에 미리보기 레이어를 추가하는 가장 간단한 방법은 layerClass가 `AVCaptureVideoPreviewLayer`인 `UIView` 서브클래스를 만드는 것이다.

```swift
import UIKit
import AVFoundation

class PreviewView: UIView {

    override class var layerClass: AnyClass {
        return AVCaptureVideoPreviewLayer.self
    }

    /// 컴파일 시점에 타입이 고정된 형태로 레이어를 가져오는 것을 쉽게 해주는 래퍼
    var videoPreviewLayer: AVCaptureVideoPreviewLayer {
        guard let layer = layer as? AVCaptureVideoPreviewLayer else {
            fatalError("Unexpected layer type.")
        }
        return layer
    }

    var session: AVCaptureSession? {
        get { videoPreviewLayer.session }
        set { videoPreviewLayer.session = newValue }
    }

}
```

캡처 세션과 함께 미리보기 레이어를 사용하려면 layer의 session 프로퍼티를 설정한다.
```swift
// Set up the preview view.
previewView.session = captureSession
```

## 캡처 세션 실행하기
입력과 출력, 미리보기를 다 설정했다면 [startRunning\(\)](https://developer.apple.com/documentation/avfoundation/avcapturesession/1388185-startrunning) 메서드를 사용해서 캡처 세션을 실행한다. 이 메서드를 호출하면 카메라 같은 입력 장치에서 데이터가 흐르기 시작한다.

```swift
// Starting the capture session is a blocking call. Perform setup using
// dedicated serial dispatch queue to prevent blocking the main thread.
captureSessionQueue.async {
    self.setupCamera()
}

private func setupCamera() {
    // setup input
    ...

    // setup output
    ...

    captureSession.startRunning()
}
```

세션이나 카메라 장치에 대해 수행되는 모든 작업(카메라 입력 추가, 출력 추가, 해상도 설정 등)과 설정은 블로킹 호출(blocking calls)이다. 블로킹 호출은 해당 작업이 완료될 때까지 프로그램의 실행이 중단되는 것을 의미한다. 특히 설정 작업이 시간이 걸릴 수 있는 경우, 메인 스레드의 응답성을 저하시킬 수 있다.

그래서 이러한 작업들을 백그라운드 직렬 큐(Background Serial Queue)로 디스패치하는 것이 권장된다. 백그라운드 직렬 큐를 사용하면 메인 스레드의 작업을 방해하지 않고도 설정 작업을 안전하게 수행할 수 있다.

기본적인 카메라 설정 외에도 초점, 노출 등의 세부적인 옵션을 설정하거나 비디오 프레임을 수신 받은 후 필터를 입히거나 사진을 캡처 후의 동작을 추가하면 된다. 필요한 작업을 마친 후에는 `stopRunning()` 메서드를 호출해서 세션을 종료하도록 해야 한다.

## Conclusion
간단히 `AVFoundation` 프레임워크를 사용해서 카메라 캡처하는 기능을 구현하는 방법을 살펴봤다. 시스템 카메라 UI가 아닌 커스텀 카메라 인터페이스를 사용해서 필요에 따라 추가적인 옵션들을 설정할 수 있는데, 앱의 특성에 맞게 필요한 기능들을 잘 구현한 카메라 인터페이스를 만들 수 있을 것 같다. 다음에는 카메라 미리보기 화면에서 특정 영역에 있는 부분만 체크해서 텍스트를 추출해 볼 수 있도록 작업할 예정이다.

<br>
**Resources**
<br>
[Capture setup | Apple Developer Documentation](https://developer.apple.com/documentation/avfoundation/capture_setup)
<br>
[Requesting authorization to capture and save media | Apple Developer Documentation](https://developer.apple.com/documentation/avfoundation/capture_setup/requesting_authorization_to_capture_and_save_media)
<br>
[Setting Up a Capture Session | Apple Developer Documentation](https://developer.apple.com/documentation/avfoundation/capture_setup/setting_up_a_capture_session)
<br>
[Camera Capture on iOS · objc.io](https://www.objc.io/issues/21-camera-and-photos/camera-capture-on-ios/)
