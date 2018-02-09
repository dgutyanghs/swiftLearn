
# Table of Contents

1.  [Swift 笔记](#org9f7dafb)
    1.  [Computed Property](#orgd8e098b)
        1.  [If a computed property’s setter does not define a name for the new value to be set, a default name of newValue is used.](#org48b4eae)
    2.  [Global constant and variables are always computed lazily](#org7a345a6)
    3.  [Type property syntax](#org9f6be2b)
        1.  ['static' VS 'class' type property](#orgc4cede0)
    4.  [泛型参数声明例子](#org4f742cb)
    5.  [DispatchQueue.main.asyncAfter使用方法](#orgba80cf6)
    6.  [RxSwift中 thottle 和 debounce区别](#org8859a47)


<a id="org9f7dafb"></a>

# Swift 笔记


<a id="orgd8e098b"></a>

## Computed Property


<a id="org48b4eae"></a>

### If a computed property’s setter does not define a name for the new value to be set, a default name of newValue is used.

    struct Point {
        var x = 0.0, y = 0.0
    }
    struct Size {
        var width = 0.0, height = 0.0
    }
    struct Rect {
        var origin = Point()
        var size = Size()
        var center: Point {
            get {
                let centerX = origin.x + (size.width / 2)
                let centerY = origin.y + (size.height / 2)
                return Point(x: centerX, y: centerY)
            }
            set(newCenter) {
                origin.x = newCenter.x - (size.width / 2)
                origin.y = newCenter.y - (size.height / 2)
            }
        }
    }
    var square = Rect(origin: Point(x: 0.0, y: 0.0),
                      size: Size(width: 10.0, height: 10.0))
    let initialSquareCenter = square.center
    square.center = Point(x: 15.0, y: 15.0)
    print("square.origin is now at (\(square.origin.x), \(square.origin.y))")
    // Prints "square.origin is now at (10.0, 10.0)"
      // Shorthand Setter Declaration
    
      // If a computed property’s setter does not define a name for the new value to be set, a default name of newValue is used. Here’s an alternative version of the Rect structure, which takes advantage of this shorthand notation:
    
      struct AlternativeRect {
          var origin = Point()
          var size = Size()
          var center: Point {
              get {
                  let centerX = origin.x + (size.width / 2)
                  let centerY = origin.y + (size.height / 2)
                  return Point(x: centerX, y: centerY)
              }
              set {
                  origin.x = newValue.x - (size.width / 2)
                  origin.y = newValue.y - (size.height / 2)
              }
          }
      }


<a id="org7a345a6"></a>

## Global constant and variables are always computed lazily

    NOTE
    
    Global constants and variables are always computed lazily, in a similar manner to Lazy Stored Properties. Unlike lazy stored properties, global constants and variables do not need to be marked with the lazy modifier.
    
    Local constants and variables are never computed lazily.


<a id="org9f6be2b"></a>

## Type property syntax


<a id="orgc4cede0"></a>

### 'static' VS 'class' type property

    You define type properties with the static keyword. For computed type properties for class types, you can use the class keyword instead to allow subclasses to override the superclass’s implementation.

    struct SomeStructure {
        static var storedTypeProperty = "Some value."
        static var computedTypeProperty: Int {
            return 1
        }
    }
    enum SomeEnumeration {
        static var storedTypeProperty = "Some value."
        static var computedTypeProperty: Int {
            return 6
        }
    }
    class SomeClass {
        static var storedTypeProperty = "Some value."
        static var computedTypeProperty: Int {
            return 27
        }
        class var overrideableComputedTypeProperty: Int {
            return 107
        }
    }


<a id="org4f742cb"></a>

## 泛型参数声明例子

    class func startToScan<T:UIViewController> (parentVc: T) where T:MyLBXScanDelegate, T:LBXScanViewControllerDelegate  {
        let vc = ScanCamera.ZhiFuBaoStyle()
        vc.delegate = parentVc
        vc.scanResultDelegate = parentVc
        parentVc.navigationController?.present(vc, animated: true, completion: nil)
    }

函数有返回值

    
    class func startToScan<T:UIViewController> (parentVc: T) ->LBXScanViewController where T:MyLBXScanDelegate, T:LBXScanViewControllerDelegate  {
        let vc = ScanCamera.ZhiFuBaoStyle()
        vc.delegate = parentVc
        vc.scanResultDelegate = parentVc
        parentVc.navigationController?.present(vc, animated: true, completion: nil)
        return vc

parentVc 是UIViewController的类型或子类，同时遵守MyLBXScanDelegate和 LBXScanViewControllerDelegate协议。


<a id="orgba80cf6"></a>

## DispatchQueue.main.asyncAfter使用方法

    let deadlineTime = DispatchTime.now() + .seconds(1)
    DispatchQueue.main.asyncAfter(deadline: deadlineTime) {
        print("test")
    }


<a id="org8859a47"></a>

## RxSwift中 thottle 和 debounce区别

            rtDisposed = rtObservable.observeOn(speechScheduler)
                .throttle(20.0, scheduler:speechScheduler) //20.0放行一个最新消息， 
    //            .debounce(20.0, scheduler: speechScheduler) //20.0内无新消息，将action,如有，取消旧的消息，重新等待
                .filter { $0 > 0 }
                .subscribe( onNext:{
                   [unowned self] value  in
                    self.handleRealTimeHR(value)
                },
                 onCompleted:{
                    print("dispose completed")
                }, onDisposed:{
                    print("disposed rtDisposed")
                })

