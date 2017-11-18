
# Table of Contents

1.  [Swift 笔记](#org97fe02a)
    1.  [Computed Property](#org4faf4d3)
        1.  [If a computed property’s setter does not define a name for the new value to be set, a default name of newValue is used.](#orgdd49378)
    2.  [Global constant and variables are always computed lazily](#orgc370580)
    3.  [Type property syntax](#orgbf41707)
        1.  ['static' VS 'class' type property](#orgbecf57a)


<a id="org97fe02a"></a>

# Swift 笔记


<a id="org4faf4d3"></a>

## Computed Property


<a id="orgdd49378"></a>

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


<a id="orgc370580"></a>

## Global constant and variables are always computed lazily

    NOTE
    
    Global constants and variables are always computed lazily, in a similar manner to Lazy Stored Properties. Unlike lazy stored properties, global constants and variables do not need to be marked with the lazy modifier.
    
    Local constants and variables are never computed lazily.


<a id="orgbf41707"></a>

## Type property syntax


<a id="orgbecf57a"></a>

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

