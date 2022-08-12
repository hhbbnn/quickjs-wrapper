# QuickJS For Android/JVM
QuickJS wrapper for Android/JVM.

## [中文说明](README-CN.md)

## Feature
- Java types are supported with JavaScript
- Support promise execute
- JavaScript exception handler
- Optimize not a function with its name when type error

Experimental Features Stability not guaranteed.
- Compile bytecode
- ESModule (import, export)

## Download

[![Maven Central](https://img.shields.io/maven-central/v/wang.harlon.quickjs/wrapper-java.svg?label=Maven%20Central&color=blue)](https://search.maven.org/search?q=g:%22wang.harlon.quickjs%22%20AND%20a:%22wrapper-java%22)

```Groovy
repositories {
  mavenCentral()
}

dependencies {
  // For Android
  implementation 'wang.harlon.quickjs:wrapper-android:latest.version'
  // For JVM
  implementation 'wang.harlon.quickjs:wrapper-java:latest.version'
}
```

### SNAPSHOT 
[![Wrapper](https://img.shields.io/static/v1?label=snapshot&message=wrapper&logo=apache%20maven&color=yellowgreen)](https://s01.oss.sonatype.org/content/repositories/snapshots/wang/harlon/quickjs/wrapper-java/) <br>

<details>
 <summary>See how to import the snapshot</summary>

#### Including the SNAPSHOT
Snapshots of the current development version of Wrapper are available, which track [the latest versions](https://s01.oss.sonatype.org/content/repositories/snapshots/wang/harlon/quickjs/wrapper-java/).

To import snapshot versions on your project, add the code snippet below on your gradle file:
```Gradle
repositories {
   maven { url 'https://s01.oss.sonatype.org/content/repositories/snapshots/' }
}
```

Next, add the dependency below to your **module**'s `build.gradle` file:
```gradle
dependencies {
    // For Android
    implementation "wang.harlon.quickjs:wrapper-android:latest-SNAPSHOT"
    // For JVM
    implementation "wang.harlon.quickjs:wrapper-java:latest-SNAPSHOT"
}
```

</details>

## Usage

### Create QuickJSContext

```Java
QuickJSContext context = QuickJSContext.create();
```

### Evaluating JavaScript

```Java
QuickJSContext context = QuickJSContext.create();
context.evaluate("var a = 1 + 2;");
```

### Supported Java Types
Currently, the following Java types are supported with JavaScript:
- `boolean`
- `int` when calling JavaScript from Java.
- `double`
- `String`
- `null`
- `JSObject` represents a JavaScript object
- `JSFunction` represents a JavaScript function
- `JSArray` represents a JavaScript Array
                
### Set Property
Java

```java
JSObject globalObj = context.getGlobalObject();
JSObject obj1 = context.createNewJSObject();
obj1.setProperty("stringProperty", "hello");
obj1.setProperty("intProperty", 1);
obj1.setProperty("doubleProperty", 0.1);
obj1.setProperty("booleanProperty", true);
obj1.setProperty("functionProperty", (JSCallFunction) args -> {
    return args[0] + "Wang";
});
globalObj.setProperty("obj1", obj1);
```

JavaScript

```javascript
obj1.stringProperty; // hello string
obj1.intProperty; // 1
obj1.doubleProperty; // 0.1
obj1.booleanProperty; // true
obj1.functionProperty('Harlon'); // HarlonWang
```                

### Get Property
JavaScript

```JavaScript
var obj1 = {
	stringProperty: 'hello string',
	intProperety: 1,
	doubleProperty: 0.1,
	booleanProperty: true,
	functionProperty: (name) => { return name + 'Wang'; }
}
```
Java

```Java
JSObject globalObject = context.getGlobalObject();
JSObject obj1 = globalObject.getJSObjectProperty("obj1");
obj1.getProperty("stringProperty"); // hello
obj1.getProperty("intProperty"); // 1
obj1.getProperty("doubleProperty"); // 0.1
obj1.getProperty("booleanProperty"); // true
obj1.getJSFunctionProperty("functionProperty").call("Harlon"); // HarlonWang
```

### Compile ByteCode

```Java
byte[] code = context.compile("'hello, world!'.toUpperCase();");
context.execute(code);
```

### ESModule
Java
```Java
JSModule.setModuleLoader(new JSModule.Loader() {
    @Override
    public String getModuleScript(String moduleName) {
        return "export var name = 'Hello world';\n" +
                "export var age = 18;";
    }
});
```
JavaScript
```JavaScript
import {name, age} from './a.js';

console.log('name：' + name); // Jack
console.log('age：' + age); // 18
```

## Concurrency
JavaScript runtimes are single threaded. All execution in the JavaScript runtime is guaranteed thread safe, by way of Java synchronization.

## Reference

- [quickjs-java](https://github.com/cashapp/quickjs-java)
- [quack](https://github.com/koush/quack)
- [quickjs-android](https://github.com/taoweiji/quickjs-android)                
