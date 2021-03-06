# Swift 5 Code snippets

## Add leading padding to view added inside an UIStackView

```swift
stackView.layoutMargins = UIEdgeInsets(top: 0, left: 20, bottom: 0, right: 20)
stackView.isLayoutMarginsRelativeArrangement = true
```

[https://stackoverflow.com/questions/32551890/how-to-add-leading-padding-to-view-added-inside-an-uistackview](https://stackoverflow.com/questions/32551890/how-to-add-leading-padding-to-view-added-inside-an-uistackview)

## Timestamp

```swift
// current date and time
let someDate = Date()

// time interval since 1970
let myTimeStamp = someDate.timeIntervalSince1970
```

## Change year in Date

```swift
var component = calendar.dateComponents([.year, .month, .day, .hour, .minute, .second], from: base)
component.year = year
Calendar.current.date(from: component)
```

[https://dev.to/onmyway133/changing-year-in-date-in-swift](https://dev.to/onmyway133/changing-year-in-date-in-swift)

## Date tomorrow

```swift
extension Date {
    static var yesterday: Date { return Date().dayBefore }
    static var tomorrow:  Date { return Date().dayAfter }
    var dayBefore: Date {
        return Calendar.current.date(byAdding: .day, value: -1, to: noon)!
    }
    var dayAfter: Date {
        return Calendar.current.date(byAdding: .day, value: 1, to: noon)!
    }
    var noon: Date {
        return Calendar.current.date(bySettingHour: 12, minute: 0, second: 0, of: self)!
    }
    var month: Int {
        return Calendar.current.component(.month,  from: self)
    }
    var isLastDayOfMonth: Bool {
        return dayAfter.month != month
    }
}
```

[https://stackoverflow.com/a/44009988](https://stackoverflow.com/a/44009988)

## iOS local notification

```swift
// Notification center property
let userNotificationCenter = UNUserNotificationCenter.current()

func requestNotificationAuthorization() {
    let authOptions = UNAuthorizationOptions.init(arrayLiteral: .alert, .badge, .sound)
    
    self.userNotificationCenter.requestAuthorization(options: authOptions) { (success, error) in
        if let error = error {
            print("Error: ", error)
        }
    }
}

override func viewDidLoad() {
    super.viewDidLoad()
    self.requestNotificationAuthorization()
    self.sendNotification()
}

func sendNotification() {
    let notificationContent = UNMutableNotificationContent()
    notificationContent.title = "Test"
    notificationContent.body = "Test body"
    notificationContent.badge = NSNumber(value: 3)
    
    if let url = Bundle.main.url(forResource: "dune",
                                withExtension: "png") {
        if let attachment = try? UNNotificationAttachment(identifier: "dune",
                                                        url: url,
                                                        options: nil) {
            notificationContent.attachments = [attachment]
        }
    }
    
    let trigger = UNTimeIntervalNotificationTrigger(timeInterval: 5,
                                                    repeats: false)
    let request = UNNotificationRequest(identifier: "testNotification",
                                        content: notificationContent,
                                        trigger: trigger)
    
    userNotificationCenter.add(request) { (error) in
        if let error = error {
            print("Notification Error: ", error)
        }
    }
}

// Local notifications
func application(_ application: UIApplication, didReceive notification: UILocalNotification) {
    UIApplication.shared.applicationIconBadgeNumber = 0
}

// Assing self delegate on userNotificationCenter
self.userNotificationCenter.delegate = self

func userNotificationCenter(_ center: UNUserNotificationCenter, didReceive response: UNNotificationResponse, withCompletionHandler completionHandler: @escaping () -> Void) {
    completionHandler()
}

func userNotificationCenter(_ center: UNUserNotificationCenter, willPresent notification: UNNotification, withCompletionHandler completionHandler: @escaping (UNNotificationPresentationOptions) -> Void) {
    completionHandler([.alert, .badge, .sound])
}

```

[https://programmingwithswift.com/how-to-send-local-notification-with-swift-5/](https://programmingwithswift.com/how-to-send-local-notification-with-swift-5/)

## Get bandle file path

```swift
let dbUrl = Bundle.main.url(forResource: "myDb", withExtension: "db")!
let dbPath = dbUrl.path
```

## Mark

```swift

// MARK: A mark comment lives here.

// MARK: - A mark comment lives here. A separation line is added.

// MARK: -

```

[https://stackoverflow.com/a/35963262](https://stackoverflow.com/a/35963262)

## Save photo to album

```swift
UIImageWriteToSavedPhotosAlbum(yourImage, self, #selector(image(_:didFinishSavingWithError:contextInfo:)), nil)


@objc func image(_ image: UIImage, didFinishSavingWithError error: NSError?, contextInfo: UnsafeRawPointer) {
    if let error = error {
        // we got back an error!
        let ac = UIAlertController(title: "Save error", message: error.localizedDescription, preferredStyle: .alert)
        ac.addAction(UIAlertAction(title: "OK", style: .default))
        present(ac, animated: true)
    } else {
        let ac = UIAlertController(title: "Saved!", message: "Your altered image has been saved to your photos.", preferredStyle: .alert)
        ac.addAction(UIAlertAction(title: "OK", style: .default))
        present(ac, animated: true)
    }
}
```

[https://www.hackingwithswift.com/example-code/media/uiimagewritetosavedphotosalbum-how-to-write-to-the-ios-photo-album](https://www.hackingwithswift.com/example-code/media/uiimagewritetosavedphotosalbum-how-to-write-to-the-ios-photo-album)

## Copy text to the clipboard

```swift
let pasteboard = UIPasteboard.general
pasteboard.string = "Hello, world!"

if let string = pasteboard.string {
    // text was found and placed in the "string" constant
}
```

[https://www.hackingwithswift.com/example-code/system/how-to-copy-text-to-the-clipboard-using-uipasteboard](https://www.hackingwithswift.com/example-code/system/how-to-copy-text-to-the-clipboard-using-uipasteboard)

## Get configuration from PLIST

```swift
// Environment.swift

import Foundation

public enum Environment {
  // MARK: - Keys
  enum Keys {
    enum Plist {
      static let rootURL = "ROOT_URL"
      static let apiKey = "API_KEY"
    }
  }

  // MARK: - Plist
  private static let infoDictionary: [String: Any] = {
    guard let dict = Bundle.main.infoDictionary else {
      fatalError("Plist file not found")
    }
    return dict
  }()

  // MARK: - Plist values
  static let rootURL: URL = {
    guard let rootURLstring = Environment.infoDictionary[Keys.Plist.rootURL] as? String else {
      fatalError("Root URL not set in plist for this environment")
    }
    guard let url = URL(string: rootURLstring) else {
      fatalError("Root URL is invalid")
    }
    return url
  }()

  static let apiKey: String = {
    guard let apiKey = Environment.infoDictionary[Keys.Plist.apiKey] as? String else {
      fatalError("API Key not set in plist for this environment")
    }
    return apiKey
  }()
}
```

[https://thoughtbot.com/blog/let-s-setup-your-ios-environments](https://thoughtbot.com/blog/let-s-setup-your-ios-environments)

## WatchOS update complications

```swift
        let server = CLKComplicationServer.sharedInstance()
        for complication in server.activeComplications ?? [] {
                server.reloadTimeline(for: complication)

        }
```

## SFSafariViewController reader mode

```swift
let urlString = "http://www.google.com"
let url = URL(string: urlString)!
let config = SFSafariViewController.Configuration()
config.entersReaderIfAvailable = true
let safariVC = SFSafariViewController(url: url, configuration: config)
present(safariVC, animated: true, completion: nil)
```

[https://stackoverflow.com/a/49107463](https://stackoverflow.com/a/49107463)

## Change default global tint color

```swift
@UIApplicationMain class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?

    func application(application: UIApplication!, didFinishLaunchingWithOptions launchOptions: NSDictionary!) -> Bool {

        self.window = UIWindow(frame: UIScreen.mainScreen().bounds)

        // ...

        self.window!.tintColor = UIColor.orangeColor()

        return true
    }
}
```

[https://stackoverflow.com/a/25095089](https://stackoverflow.com/a/25095089)

## simulator sqlite db paths

```swift
let dirPaths = NSSearchPathForDirectoriesInDomains(.DocumentDirectory, .UserDomainMask, true)
var docsDir = dirPaths[0]

print(docsDir)

// use app grpup

public lazy var applicationDocumentsDirectory: URL? = {
        // The directory the applicatio:  uses to store the Core Data store file. This code uses a directory named "com.nootapp.CoreDataTest" in the application's documents Application Support directory
        return FileManager.default.containerURL(forSecurityApplicationGroupIdentifier: "group.com.appgroup")
}()

guard let storeURL = self.applicationDocumentsDirectory?.appendingPathComponent("CoreData.sqlite") else {
            fatalError("Could not get sqlite url.")
        }

Swift.debugPrint("storeURL", storeURL.absoluteString)
```

## check iCloud login

```swift
if FileManager.default.ubiquityIdentityToken != nil {
    print("iCloud Available")
} else {
    print("iCloud Unavailable")
}
```

[https://stackoverflow.com/a/39053572](https://stackoverflow.com/a/39053572)

## display base64 string image

```swift
if let imageData = Data.init(base64Encoded: base64String) {
imageView.image = UIImage(data: imageData)
}
```

## SF symbols

[https://www.avanderlee.com/swift/sf-symbols-guide/](https://www.avanderlee.com/swift/sf-symbols-guide/)

## hide files

```swift
extension URL {
    /// `true` is hidden (invisible) or `false` is not hidden (visible)
    var isHidden: Bool {
        get {
            return (try? resourceValues(forKeys: [.isHiddenKey]))?.isHidden == true
        }
        set {
            var resourceValues = URLResourceValues()
            resourceValues.isHidden = newValue
            do {
                try setResourceValues(resourceValues)
            } catch {
                print("isHidden error:", error)
            }
        }
    }
}
```

[https://stackoverflow.com/a/34746109](https://stackoverflow.com/a/34746109)

## App and build version

```swift
// app version
if let appVersion = Bundle.main.infoDictionary?["CFBundleShortVersionString"] as? String {
      // present appVersion
}

if let buildVersion = Bundle.main.infoDictionary?["CFBundleVersion"] as? String {
      // present buildVersion
 }

```

[https://stackoverflow.com/a/59350389](https://stackoverflow.com/a/59350389)

## Providing Access to Directories

[Providing Access to Directories](https://developer.apple.com/documentation/uikit/view_controllers/providing_access_to_directories)

## swiftlint script

```sh
if which swiftlint >/dev/null; then
  swiftlint autocorrect
  swiftlint
else
  echo "warning: SwiftLint not installed, download from https://github.com/realm/SwiftLint"
fi
```

[https://stackoverflow.com/a/57051011](https://stackoverflow.com/a/57051011)

## Using KVO to listen to volume changes

```swift
class YourViewController: UIViewController {

    var obs: NSKeyValueObservation?

    override func viewDidLoad() {
        super.viewDidLoad()

        let audioSession = AVAudioSession.sharedInstance()
        self.obs = audioSession.observe( \.outputVolume ) { (av, change) in
            print("volume \(av.outputVolume)")
        }
    }
}
```

[https://stackoverflow.com/a/51437265](https://stackoverflow.com/a/51437265)
[https://www.ralfebert.de/ios-examples/swift/property-key-value-observer/](https://www.ralfebert.de/ios-examples/swift/property-key-value-observer/)

## Find list of Local Notification the app has already set

```swift
let center = UNUserNotificationCenter.current()

override func viewWillAppear(_ animated: Bool) {
    super.viewWillAppear(animated)

    center.getPendingNotificationRequests { (notifications) in
        print("Count: \(notifications.count)")
        for item in notifications {
          print(item.content)
        }
    }
}

```

[https://stackoverflow.com/a/39034576](https://stackoverflow.com/a/39034576)