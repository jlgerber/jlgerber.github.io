# Intro

## QtQuick encompasses a number of components:
- QML Module
  - Engine
  - Language
- QML API
- C++ API

## Primarily declaritive but has imperative aspects via javascript

## Four pillars of qt quick
- nested elements
- properities
- binding
- notifications

## Pros and Cons

pros | cons
--- | ---
 modular |  QML/ C++ / JavaScript
 rearrangeable | Performance? Depends on the platform. See dosc
 quick prototyping | less familiar
 extensible | quick prototyping can be a con
 network aware | runtime vs compile time

## Examples
see QtCreator's examples tab. look at QtQuick Demo Tweat Search

## Hello World application.

```
import QtQuick 2.2
import QtQuick.Control 1.2
ApplicaitonWindow {
  visible: true
  width: 640
  height: 480
  title: qsTr("Hello World")

  menuBar: MenuBar {
    Menu {
    title: qsTr("File")
      MenuItem {
        text: qsTr("Exit")
        onTriggered: Qt.quit()
      }
    }
  }
  Text {
    text: qsTr("Hello World")
    anchors.centerIn: parent
  }
}
```

# QML syntax
## Types
### Basic Types
- int
- bool
- real
- double
- string
- url ( list of qml objects. best used for static list of objects )
- list
- enumeration
- var ( can refer to any type)

### Qt Quick Imported Types
- color
- font
- matrix 4x4
- quaternion
- vector2D
- vector3d
- date
- point
- size
- rect

#### Color

can define color using
- SVG color names
- ARGB Quad #FF112233
- RGB Hex #112233
- Qt.Lighter, Qt.Darker
- Qt.tint, Qt.hsla

#### Global Objects
- Qt ( eg Qt.qsTr )
  - qsTr
  - font
  - lighter
  - darker
  - resolvedUrl
  - size
  - quit
- QML ( console.log() )
  - XmlHttpRequest
  - Offline Storage/ Database APIs
  - Logging

#### Font
- family
- bold
-italic
- underline
- pointSize
- pixelSize

#### Dates
- specify as YYYY-MM-DD string
- Format with Qt.formatDate() or Qt.formatDateTime()
- Qdate <-> date

## QML Object

### QML object declaration
object declarations have a name ( Item ), and group an id attribute and one or more properties between curly brace delimeters.
```
Item {
  id: myItemId
  width: 640
}
```

Often properties are written one per line, but you can put multiple props on the same line and separate them with semi-colons.

### Attributes

Every qml object has a set of attributes and each instance can be thought of as an instance of a class.

- Objects don't have to have an id, but it is recommended.
- Ids must be camel cased.
- We are going to use an id suffix with all id properties.
- We are going to name the root id property `rootId`

We can `bind` attributes using ids. For example, here our secont Text instance sets it's text to the first text object's text by way of an id reference.
```
Text {
  id: textId
  text: "Here is some simple text"
}
Text {
  id: nextTextId
  text: textId.text
}
```
### Child Objects
Qml can have child objects. you can nest elements

```
Item {
  id: myItemId
  width: 50
  Rectangle {
    id: color: "teal"
  }
}
```
The coordinate system for positioning the child object comes from the parent. All items have z ordering. By default, earlier items come before later items.

### Custom Properties

#### Specific types
you can declare a custom property by using the property key word, followed by the type and name, follwed by a colon and the default value.

```
property string firstString: "this is a custom property"
```

#### Variant properties

```
property var largeFont: 48
```

Custom properties automatically get signals created on their behalf which fire every time the value changes. They also get signal handler created named like so on<propertyName>Changed.

#### Property Aliases

You can create properties which alias or reference other properties explicitly using the alias keyword.
```
property alias myTextField: textId.text
```

#### Property Scope

 peopwery xLater loaded docs can look in earlier ones but its a bad idea. The alias property can forward a property to an outer scope, and that is a much better idea.

### Grouped Properties

- use inline group notation
- use dot notation
- use block group notation

```
// using inline group notation
Text {
  font {pixelSize: 0; italic: true }
}

// using dot notation
Text {
  font.pixelSize: largeFont
  font.italic: true
}

// using block group notation
Text{
  font {
    pixelSize: 0
    italic: true
  }
}
```

### Building Components
Two ways

1. build up a tree of qml objects with nesting and put it in a separate file
2. build components inline

# Types

## Inheritance hierarchy

QtObject->Item->visual types

## Item

Item has no visual appearance. It is usually used to group related elements. It has a number of important attribute groups which all visual types inherit.

### Attribute Groups

- Geometry
- Layout
- Key Handling
- Transformation
- Visual

#### Geometry

x | left psoition
y | top position
width | horizontal extent
height | vertical extent
z | stacking order

you want to avoid explicitly setting geo if you can.

#### Layout

anchors | position relative to a parent or sibling
margins | anchor specific

#### Key Handling

keys | Keys.onPressed
KeyNavigation | move between focusable items
focus | true / false

#### Transformations

transformation | rotation scale and translate
transformOrigin | the origin point for scaling and rotating
rotation | rotation in degrees
scale | reduce ( > 0 and <1 ) or enlarge (>1), mirror(<0)

#### Visual

opacity | transparent(0), opaque(1)
visible | true  / false
clip | true / false
smooth | true / false ( leave it true )

#### States

states | a set of property configurations per state. can be used for a wide variety of features.

## Visual Type Redux

### Rectangle
- border
- color
- radius
- gradient

### Text
- color
- font
- style
- Unicode
- html

### Image
- Source ( Resource, File, Internet )
- Display ( mirror, fillMode, alignment )
- Performance (asynchronous, cache, smooth )

### More
  - BorderImage (slice and dice an image )
  - AnimatedImage
  - Screen (figure out your monitor)

# Javascript
## importing techniques
- importing into qml
- importing into javascript
- importing into javascript and pulling into the namespace of the caller

```
// assume we have added util.js to our project
// in qml given the javascript
import "util.js" as Util1

// in javascript library (notice the period in front of import )
.import "util.js" as Util1

// in javascript library
// this will bring everything in util.js into the scope of the javascript module in which the import apears
Qt.import("util.js)
```

## function scope

If you define a function locally ( within a component ) you can access it directly

```

Button {
  function foobar(p1) {
    print ("foobar: " + p1)
  }
  onClicked: foobar("b1")
}
```

However, you must use id qualified invocation if your function lives outside your immediate scope

```
Item {
  id: itemId
  function foo(p1) {
    print("foo " + p1)
  }
  Button {
    id: buttonId
    onClicked: itemId.foo("button")
  }
}
```

## Qt.bind
If you want to update a property that is being set dynamically, you cannot set it directly. this will break existing bindings.

```
property double phi: 1.618
Button {
  id: setWidthId
  width : height * phi
  onClicked: width = phi * height
}

Button {
  id: setHeightId
  onClicked: setWidthId.height *= 1.2
}
```
Rather, you should use Qt.bind

```
Button {
  id: setWidthId

  onClicked: width = Qt.bind(function() {return height * phi;})
}
```

# User Input

( mouse keyboard touch others. see Qt Sensors docs )

We are looking at the Basic inputs. There are advanced input controls as well.

## TextInput
    for single line text entry ( although it will wrap to multi lines)
## TextEdit
    for multiple lines of text and suitable for rich text entry. does not provide scrollbars. ( see TextArea)

## Text input utility
- Int Validator
- Double Validator
- RegExp Validator
- Focus Scope

## Gestures
- Flickable
- PinchArea
- MultiPointTouchArea

## Events
- KeyEvent
- DragEvents
- WheelEvent
- MouseEvent
- TouchPoint
- PinchEvent
