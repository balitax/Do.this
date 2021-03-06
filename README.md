# Do.this

Do.this is a Swift 3 quick async helper inspired by node.js Async

### Usage:

```swift
Do.this { this in
    
    //do stuff
    this.done(result: someResult)
    
}.then (name: "result step") { this in
    
    //do more stuff
	
	//every this has an index property and an optional name property
	//which can be useful for debugging and error handling
	print("this: \(this.name ?? String(this.index))")
		
    //you can also access the previous result if needed:
    print("previousResult: \(this.previousResult)")
	
	//if you're doing anything async, don't forget to call done()	
	DispatchQueue.main.asyncAfter(deadline: .now() + 2) {
     	
		//async stuff
		
	    //btw, result is optional
	    this.done()
    }    
    
}.then (after: 2) { this in

    //you can also add a delay directly
    this.done()

}.then { this in
    
    //if an error happened, pass it in the done callback (you can still pass a result)
    //if you pass an error, the chain will break (see catch and finally below)
    let error = SomeError.bummer
    this.done(result: someResult, error: error)
    
}.then (on: DispatchQueue.global(qos: .background)) { this in
    
    //this will execute in a background queue
    //(+you can combine this with a delay)
    print("on: \(DispatchQueue.currentLabel)")
    this.done(result: someResult)
    
}.then (on: .main) { this in
    
    //this will be execute in the main queue
    //if you dont specify a queue, this will be executed on the current (last used) queue
    print("on: \(DispatchQueue.currentLabel)")
    this.done(result: someResult)
    
}.catch { this in
    
    //catch an error, this will point to the step that triggered the error	
    print("catched error: \(this.error) from \(this.name ?? String(this.index))")
    
}.finally { this in
    
    //finally - will execute even if an error happened
    print("finally (previousResult: \(this.previousResult))")
    exp.fulfill()
}
```

## Requirements

Swift 3

## Installation

Do.this is available through [CocoaPods](http://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod "Do.this"
```

## Author

Barak Harel

## License

Do.this is available under the MIT license. See the LICENSE file for more info.
