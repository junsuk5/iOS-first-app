# Swift 로 첫 iOS 앱 개발 정리

https://developer.apple.com/library/ios/referencelibrary/GettingStarted/DevelopiOSAppsSwift/Lesson2.html#//apple_ref/doc/uid/TP40015214-CH5-SW1

# Build a Basic UI 편

## AppDelegate.swift 의 중요 메소드
앱을 작성할 때 기본으로 생성되는 클래스. 앱 전체의 라이프사이클을 관리하는 클래스

## 검증
```swift:AppDelegate.swift
import UIKit

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?


    func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
        // Override point for customization after application launch.
        print("앱 처음 시작")
        return true
    }

    func applicationWillResignActive(application: UIApplication) {
        print("앱이 종료 되기 직전")
    }

    func applicationDidEnterBackground(application: UIApplication) {
        print("앱 종료 후")
    }

    func applicationWillEnterForeground(application: UIApplication) {
        print("앱을 실행하려고 할 때")
    }

    func applicationDidBecomeActive(application: UIApplication) {
        print("앱 실행 후")
    }

    func applicationWillTerminate(application: UIApplication) {
        println("フリックしてアプリを終了させた時に呼ばれる")
    }

}
```

# Connect the UI to Code 편

## UIViewController
MVC의 C 부분. 앱의 화면이나 화면의 이동을 관리

### 사용예
```ViewController.swift
class ViewController: UIViewController {
```

## @IBOutlet
Interface Builder 에서 작성한 버튼 등의 컴포넌트와 코드내의 프로퍼티와 연결하는 속성.
IB 는 Interface Builder 의 약자.
스토리보드에서 컴포넌트에 ctl 누르면서 드래그하면 자동으로 삽입, 연결됨.

### 사용예
```ViewController.swift
    @IBOutlet weak var nameTextField: UITextField!
    @IBOutlet weak var mealNameLabel: UILabel!
```

## UITextField
텍스트 필드를 관리하는 클래스.
유저가 편집 가능한 문자열을 표시.
안드로이드의 EditText 에 해당.

### 사용예
```ViewController.swift
@IBOutlet weak var nameTextField: UITextField!
```

## @IBAction
메소드 앞에 적으면 그 메소드를 Interface Builder 에서 지정, 사용하는 것이 가능.
@IBOutlet 과 같이 스토리보드에서 컴포넌트를 ctl + 드래그하여 코드에 올려 놓으면 자동으로 삽입되면서, 연결 됨.

### 사용예
```ViewController.swift
@IBAction func setDefaultLabelText(sender: UIButton) {
}
```

## UITextFieldDelegate
UITextField에 관한 delegate 프로토콜.
이 프로토콜을 사용하여 UITextField 로부터 여러가지 메세지를 받을 수 있음.
예를 들어 키보드의 리턴 값 콜백은 textFieldShouldReturn 메소드를 구현하면 됨.

delegate는 오브젝트의 처리를 다른 오브젝트에 맡기는 것.

Android 에서의 리스너 같은 역할인 듯.

### 사용예
```ViewController.swift
class ViewController: UIViewController, UITextFieldDelegate {
```

## ViewDidLoad
ViewController 의 View 가 로드된 후 호출됨

### 사용예
```
ViewController.swift
    override func viewDidLoad() {
        super.viewDidLoad()

        // Handle the text field’s user input through delegate callbacks.
        nameTextField.delegate = self
    }
```

## TextFieldShouldReturn
UITextFieldDelegate 의 메소드.
TextField 의 입력으로 리턴 키를 눌렀을 때 호출 됨.
### 사용예
```ViewController.swift
    func textFieldShouldReturn(textField: UITextField) -> Bool {
        // Hide the keyboard.
        textField.resignFirstResponder()
        return true
    }
```

## resignFirstResponder
TextField 에 대해서 구현하면 키보드를 닫을 수 있음.

### 사용예
```ViewController.swift
    func textFieldShouldReturn(textField: UITextField) -> Bool {
        // Hide the keyboard.
        textField.resignFirstResponder()
        return true
    }
```

## textFieldDidEndEditing
TextField 의 편집이 종료된 직후에 호출됨

### 사용예
```ViewController.swift
    func textFieldDidEndEditing(textField: UITextField) {
        mealNameLabel.text = textField.text
    }
```

# Work with View Controllers 편

## UIImageView
그림을 표시하는 클래스.
단일 그림이나 복수 그림을 이용한 애니메이션 수행 기능.

### 사용예
```ViewController.swift
@IBOutlet weak var photoImageView: UIImageView!
```

## UITapGestureRecognizer
UIGestureRecogniter 의 서브 클래스.
탭을 인식.

numberOfTapsRequired : 인식하는 탭의 수
numberOfTouchesRequired : 탭 하는 손가락 수 지정

### 사용예
```ViewController.swift
    @IBAction func selectImageFromPhotoLibrary(sender: UITapGestureRecognizer) {
```

## UIImagePickerController
사진이나 동영상을 선택하거나 촬영할 때 사용하는 클래스.

UIImagePickerControllerDelegate 와 UINavigationControllerDelegate 의 구현 필요.

### 사용예
```ViewController.swift
// UIImagePickerController is a view controller that lets a user pick media from their photo library.
let imagePickerController = UIImagePickerController()
```

## UIImagePickerControllerDelegate
포토 갤러리에서 사진이나 동영상을 선택하는 등, 이미지 피커를 사용할 때 필요한 메소드가 정의 되어 있는 프로토콜.

UIImagePickerController 를 이용하여 사진 등을 가져올 때 구현할 필요 있음.

### 사용예
```ViewController.swift
class ViewController: UIViewController, UITextFieldDelegate, UIImagePickerControllerDelegate, UINavigationControllerDelegate {
```

## UINavigationControllerDelegate
화면이동을 관리하는 UINavigationController 를 이용할 때 필요한 프로토콜.

UINavigationController 의 서브 클래스인 UIImagePickerController를 사용시 이 프로토콜의 구현 필요.

### 사용예
```ViewController.swift
class ViewController: UIViewController, UITextFieldDelegate, UIImagePickerControllerDelegate, UINavigationControllerDelegate {
```

## UIImagePickerControllerSourceType
그림을 어디에서 얻을 것인지 지정.

열거형으로 선언 되어 있음.
- PhotoLibrary
- Camera
- SavedPhotosAlbum

UIImagePickerController 에 sourceType 프로퍼티로 정의 되어 있음.

### 사용예
```ViewController.swift
// Only allow photos to be picked, not taken.
imagePickerController.sourceType = .PhotoLibrary
```

## presentViewController
UIViewController 의 메소드.
지정한 뷰 컨트롤러를 모달 한다.

예제에서는 imagePickerController 를 지정하여 포토갤러리를 표시한다.

### 사용예
```ViewController.swift
presentViewController(imagePickerController, animated: true, completion: nil)
```

## imagePickerControllerDidCancel
UIImagePickerControllerDelegate 프로토콜에 정의 되어 있음.
호출한 이미지 피커 뷰에서 유저가 cancel 을 했을 경우 호출 됨.

dismissModalViewControllerAnimated 메소드를 구현하여 이미지 피커의 표시를 없을 수 있음.

### 사용예
```ViewController.swift
func imagePickerControllerDidCancel(picker: UIImagePickerController) {
    // Dismiss the picker if the user canceled.
    dismissViewControllerAnimated(true, completion: nil)
}
```

## dismissViewControllerAnimated
모달로 표시 되어 있는 뷰 컨트롤러를 닫는다.

### 사용예
```ViewController.swift
func imagePickerControllerDidCancel(picker: UIImagePickerController) {
    // Dismiss the picker if the user canceled.
    dismissViewControllerAnimated(true, completion: nil)
}
```

## imagePickerController
UIImagePickerControllerDelegate 프로토콜에 의해 정의 되어 있음.
사진이 선택되었을 때 호출 되는 메소드.

### 사용예
```ViewController.swift
func imagePickerController(picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [String : AnyObject]) {
    // The info dictionary contains multiple representations of the image, and this uses the original.
    let selectedImage = info[UIImagePickerControllerOriginalImage] as! UIImage

    // Set photoImageView to display the selected image.
    photoImageView.image = selectedImage

    // Dismiss the picker.
    dismissViewControllerAnimated(true, completion: nil)
}
```

# Implement a Custom Control 편

## UIView
화면 표시와 영역내의 콘텐츠를 표시하는 클래스.
튜토리얼에서는 rating 부분을 커스텀 뷰로 작성.

안드로이드의 View와 동일.

### 사용예

```RatingControl.swift
import UIKit

class RatingControl: UIView {

}
```

## init?(coder aDecoder: NSCoder)

UIView 의 서브클래스에서 오버라이드 해야하는 initializer.
Interface Builder 에서 그 View를 호출할 때 사용 됨.
Interface Builder로 뷰를 구축하지 않는 경우 initializer로서init(frame frame: CGRect)를 오버라이드 한다.

### 사용예

```RatingControl.swift
required init?(coder aDecoder: NSCoder) {
    super.init(coder: aDecoder)
}
```

## intrinsicContentSize
뷰의 사이즈를 지정하여 반환, 그것을 레이아웃 시스템에 전달하는 역할.

안드로이드의 onMeasure.

### 사용예
```RatingControl.swift
override func intrinsicContentSize() -> CGSize {
    return CGSize(width: 240, height: 44)
}
```

## addTarget
UIControl 클래스에 정의되어 있는 메소드.
특정 이벤트가 일어날 때 지정한 타겟에 대해, 지정한 액션을 일어하게 함.

버튼을 눌렀을 때, 자신의 ratingButtonTapped 을 기동하는 등.

### 사용예
```RatingControl.swift
button.addTarget(self, action: "ratingButtonTapped:", forControlEvents: .TouchDown)
```

## layoutSubviews()
UIView 클래스의 메소드.
이 안에 서브뷰의 레이아웃 처리를 작성하여, view의 상태에 맞춰서 필요할 때에 레이아웃 처리가 됨.

안드로이드의 onLayout()의 역할인 듯.

### 사용예
```RatingControl.swift
override func layoutSubviews() {
    var buttonFrame = CGRect(x: 0, y: 0, width: 44, height: 44)

    // Offset each button's origin by the length of the button plus spacing.
    for (index, button) in ratingButtons.enumerate() {
        buttonFrame.origin.x = CGFloat(index * (44 + 5))
        button.frame = buttonFrame
    }
}
```

## enumerate()

Swift 의 표준함수.
배열에서 각 키와 값을 튜플로 꺼내옴.

### 사용예
```RatingControl.swift
for (index, button) in ratingButtons.enumerate() {
    buttonFrame.origin.x = CGFloat(index * (44 + 5))
    button.frame = buttonFrame
}
```

## setImage(_:forState:)
UI 버튼의 메소드
버튼에 이미지 설정.
forState로 버튼의 상태에 따라 이미지를 설정 가능.
버튼의 상태에 따란 이미지를 변경 가능.

안드로이드의 selector.

### 사용예
```RatingControl.swift
button.setImage(emptyStarImage, forState: .Normal)
button.setImage(filledStarImage, forState: .Selected)
button.setImage(filledStarImage, forState: [.Highlighted, .Selected])
```

## adjustsImageWhenHighlighted
UIButton 클래스의 프로퍼티.
버튼 상태가 highlighted가 될 때 이미지를 변경할 것인지 말지 진위값.

### 사용예
```RatingControl.swift
button.adjustsImageWhenHighlighted = false
```

## didSet
프로퍼티 감시.
프로퍼티에 설정하면 프로퍼티 값이 변할 때의 처리를 작성 가능.

프로퍼티 값이 변하기 전에 처리하고 싶을 땐 willSet.

### 사용예
```RatingControl.swift
var rating = 0 {
	didSet {
    	setNeedsLayout()
	}
}
```

## setNeedsLayout()
layoutSubviews()을 호출하여, 레이아웃을 갱신.

안드로이드의 invalidate.

### 사용예

```RatingControl.swift
var rating = 0 {
    didSet {
        setNeedsLayout()
    }
}
```

# Define Your Data Model 편

##XCTAssertNotNil()
Xcode 의 테스트 프레임워크 'XCTest' 에서 사용.
인수로 넘긴 값이 Nil 이 아닌지를 테스트 하는 메소드.

### 사용예
```FoodTrackerSampleTests.swift
// Success case.
let potentialItem = Meal(name: "Newest meal", photo: nil, rating: 5)
XCTAssertNotNil(potentialItem)
```

## XCTAssertNil()
Nil 인지 체크

### 사용예
```FoodTrackerSampleTests.swift
// Failure cases.
let noName = Meal(name: "", photo: nil, rating: 0)
XCTAssertNil(noName, "Empty name is invalid")

let badRating = Meal(name: "Really bad rating", photo: nil, rating: -1)
XCTAssertNil(badRating, "Negative ratings are invalid, be positive")
```







