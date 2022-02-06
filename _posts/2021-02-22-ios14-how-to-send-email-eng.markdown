---
title: How to send emails from iOS 14
layout: post
author: Joohee Kim
tags: iOS
---

With iOS 14, third-party app can be the default email app. This means we may have to support sending email with third-party apps. If the user is not using the default email app and use another app like gmail, we may need to handle to open gmail app to send email. Let's see how to do this.

# How to send email via default

![Email composition Interface](/assets/img/2021/02/22/image1.jpeg)
*Email composition Interface*

Apple's `MessageUI` framework provides `MFMailComposeViewController` which shows an email composition interface to send email inside an app. Although this does not automatically send email, so the user need to tap themselves to send email. Here is the example code below.

```swift
import MessageUI

class ViewController: UIViewController {

    ...

    func sendEmail() {
        let composer = MFMailComposeViewController()
        let receiver = "imjhk03@gmail.com"
        let subject = "Bug report: "
        let appVersion = Bundle.main.infoDictionary?["CFBundleShortVersionString"] ?? 1.0    // App version
        let osVersion = UIDevice().systemVersion    // os version
        let message = """
        Hello,

        App Version: \(appVersion)
        iOS Version: \(osVersion)
        """
        
        if MFMailComposeViewController.canSendMail() {
            composerVC.mailComposeDelegate = self

            // Configure the fields of the interface.
            composerVC.setToRecipients([receiver])
            composerVC.setSubject(subject)
            composerVC.setMessageBody(message, isHTML: false)
            
            // Present the view controller modally.
            present(composer, animated: true, completion: nil)
        } else {
            // show failure alert
        }
    }

}
```

You need to `import MessageUI` and conform to `MFMailComposeViewControllerDelegate` protocol. Make sure to check if the user device can send email with `canSendMail()` function. Additionally, we can also get the app's version or the device os version with `Bundle` or `UIDevice`.

To dismiss the email composition interface, we need to conform `MFMailComposeViewControllerDelegate` protocol. In `mailComposeController(_:didFinishWith:error:)` function we dismiss the view controller. We can also handle if the email has sent or cancelled to do more tasks.

```swift
// MARK: - MFMailComposeViewControllerDelegate
extension ViewController: MFMailComposeViewControllerDelegate {
    func mailComposeController(_ controller: MFMailComposeViewController, didFinishWith result: MFMailComposeResult, error: Error?) {
        // Check the result or perform other tasks.
        if result == .sent {
            
        } else {
            
        }
        
        // Dismiss the mail compose view controller.
        controller.dismiss(animated: true)
    }
}
```

<br/>

# How to send email via third party apps

The other way to send email is to create URL with `mailto` scheme and open it. Below code shows the URL with Gmail, Microsoft Outlook, and Spark app.

```swift
let subjectEncoded = subject.addingPercentEncoding(withAllowedCharacters: .urlHostAllowed) ?? subject
let bodyEncoded = body.addingPercentEncoding(withAllowedCharacters: .urlHostAllowed) ?? body
        
let gmailUrl = URL(string: "googlegmail://co?to=\(receiver)&subject=\(subjectEncoded)&body=\(bodyEncoded)")
let outlookUrl = URL(string: "ms-outlook://compose?to=\(receiver)&subject=\(subjectEncoded)&body=\(bodyEncoded)")
let sparkUrl = URL(string: "readdle-spark://compose?recipient=\(receiver)&subject=\(subjectEncoded)&body=\(bodyEncoded)")
```

To open the URL, we need to set `LSApplicationQueriesSchemes` in the `Info.plist`.

```swift
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>googlegmail</string>
    <string>ms-outlook</string>
    <string>readdle-spark</string>
</array>
```

We are going to use AlertSheet to show apps that can open the URL. By using the `canOpenURL(_:)` function, we check and filter the third party apps that the app can open. Then make a tuple to store the URL and the name.

```swift
let sendViaGmail = (url: gmailUrl, title: "Gmail")
let sendViaOutlook = (url: outlookUrl, title: "Outlook")
let sendViaSpark = (url: sparkUrl, title: "Spark")
        
let availableApps = [sendViaGmail, sendViaOutlook, sendViaSpark].filter { availableApp -> Bool in
    guard let url = availableApp.url else { return false }
    return UIApplication.shared.canOpenURL(url)
}
```

After selecting any apps from the AlertSheet, the `open(_:options:completionHandler:)` function will open the third party app to send email. If there are no apps that the app can open, we are going to show a failure alert.

```swift
guard !availableApps.isEmpty else {
    presentAlertCanNotSendEmail()
    return
}
        
presentAlertSheetThirdPartyEmailApps(availableApps)

...
    
private func presentAlertCanNotSendEmail() {
    let alert = UIAlertController(title: "Alert", 
                                  message: "It seems there is no way to send email in your device. Please send email to bugReport@email.com", 
                                  preferredStyle: .alert)
    alert.addAction(UIAlertAction(title: "OK", style: .default))
    present(alert, animated: true)
}
    
private func presentAlertSheetThirdPartyEmailApps(_ availableApps: [AvailableApps]) {
    let alert = UIAlertController(title: nil, message: nil, preferredStyle: .actionSheet)
    for app in availableApps {
        alert.addAction(UIAlertAction(title: app.title, style: .default, handler: { _ in
            guard let url = app.url else { return }
            UIApplication.shared.open(url, options: [:], completionHandler: nil)
        }))
    }
    alert.addAction(UIAlertAction(title: "Cancel", style: .cancel, handler: nil))
        
    present(alert, animated: true)
}
```

If you apply it, you can see the screenshot like below.
![Third Party App Alert Sheet and No available App Alert](/assets/img/2021/02/22/image2-1.jpeg)

WARNING: If you want to test sending an email app, you need to test it on a real device, not on an iOS simulator.

From iOS 14, third-party apps can be set as default email apps, and this post introduce how to support it. If you want to see the full code, check it on [GitHub](https://github.com/imjhk03/ThirdPartyMailSupport/tree/main).