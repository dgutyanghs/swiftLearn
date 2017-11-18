
# Table of Contents

1.  [Swift 笔记](#org110dfe8)
    1.  [Computed Property](#orgb344b32)
        1.  [If a computed property’s setter does not define a name for the new value to be set, a default name of newValue is used.](#org718e522)


<a id="org110dfe8"></a>

# Swift 笔记


<a id="orgb344b32"></a>

## Computed Property


<a id="org718e522"></a>

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

