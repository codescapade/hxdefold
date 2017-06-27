![image](https://cloud.githubusercontent.com/assets/576184/15849567/80f4ada8-2c93-11e6-8430-5b5dbe5e58a3.png)

# Haxe/Lua externs for the Defold game engine

[![Build Status](https://travis-ci.org/hxdefold/hxdefold.svg?branch=master)](https://travis-ci.org/hxdefold/hxdefold)

**Defold API version: 1.2.107**

## Example

```haxe
// define script component data
// fields having @property metadata are visible in component editor
typedef GreeterData = {
    @property(true) var greet:Bool;
}

// define script by inheriting `Script` class with specified data type
class Greeter extends defold.support.Script<GreeterData> {
    // override callback function
    override function init(data:GreeterData) {
        if (data.greet)
            trace('Hello, World!');
    }
}
```

## Documentation

Required Haxe compilation flags:
 * `-lib hxdefold` - use this library, obviously
 * `-lua something.lua` - the output lua module
 * `--macro defold.support.ScriptMacro.use(".")` - generate glue `.script` files for script classes, first argument is the path to Defold project root folder

Recommended additional Haxe compilation flags:
 * `-D analyzer-optimize` - enable static compile-time optimizations resulting in smaller and faster code
 * `-D luajit` - compile for LuaJIT, because that's what Defold uses

More docs to be written...

Here's some [generated Haxe API documentation](http://hxdefold.github.io/hxdefold/).

And example Defold projects, ported from Lua:
 * https://github.com/hxdefold/hxdefold-example-sidescroller
 * https://github.com/hxdefold/hxdefold-example-platformer
 * https://github.com/hxdefold/hxdefold-example-frogrunner
 * https://github.com/hxdefold/hxdefold-example-magiclink
 * https://github.com/hxdefold/hxdefold-example-throwacrow

## How does it work?

Since version 3.4, Haxe supports compiling to Lua, making it possible to use Haxe with Lua-based engines, such as Defold.

However, this requires a bit of autogenerated glue code, because Defold expects scripts to be in separate files, while
Haxe compiles everything in a single lua module. So what we do, is generate a simple glue `.script` file for each class
extending `defold.support.Script` base class.

For example, for the `Greeter` script from this README, this glue code is generated in the `Greeter.script` file:

```lua
-- Generated by Haxe, DO NOT EDIT (original source: src/Greeter.hx:8: lines 8-14)

require "main"

go.property("greet", true)

local script = Greeter.new()

function init(data)
    script:init(data)
end
```

You can then use this script in Defold game objects.
