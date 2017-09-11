### Introduction

Allows the usage of ES6/Babel with PlayCanvas by patching the PlayCanvas script loader in both the Editor and the Engine
runtime.  When installed the created scripts also can specify a `.construct` method on their prototype which
will be called after the main PlayCanvas constructor.  This enables script to be run at creation time, which was
lost in PlayCanvas Scripts 2 which otherwise only calls user supplied code when the item is first enabled.

Additionally adds the ability to set an object containing the definition of attributes for the script.  This enables Intellisense
systems to show those variables defined on the script as being available.

In addition to the traditional method of defining attributes the user may also use the new `pc.attr` object which has
helpers to quickly create attributes with less clutter and typing while retaining the full functionality.


### Installation

```language-shell
npm install --save createscript
```

### Usage

```language-javascript
import 'createscript'

...

let MyScript = pc.createScript('myscript')
MyScript.attributes.set({
    stringAttribute:            pc.attr.string,
    stringAttributeWithDefault: pc.attr.string.default("Default Value"),
    stringAttributeWithEnum:    pc.attr.string
                                    .enum({ 
                                        "Mode A": "a", 
                                        "Mode b": b
                                    })
                                    .default("b"),
    numericAttribute:           pc.attr.number,
    animationAttribute:         pc.attr.animation,
    entityAttribute:            pc.attr.entity,
    booleanAttribute:           pc.attr.boolean.enum({ "On": true, "Off": false}),
    vectorAttribute:            pc.attr.vec3.default(0,0,1),
    anotherVectorAttribute:     pc.attr.vec3.default(pc.Vec3.UP),
    yetAnotherVectorAttribute:  pc.attr.vec3.default([0,1,0])
})

MyScript.prototype.construct = function() {
    //Called at instantiation time, even if not enabled - this.entity is set
}

```

Includes all current attributes with "sensible" names.

Available attributes are:

`string`, `number`, `boolean`, `entity`, `animation`, `audio`, `vec3`, `curve`, `curveSet`, `model`,
`material`, `json`, `text`, `html`, `css`, `shader`, `font`, `binary`, `texture`, `scene`

### Requirements

Requires PlayCanvas Engine to be running on the page.  Uses ES6/Babel/PlayCanvas template.
