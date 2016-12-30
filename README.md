# ZeroMethodSwizzling
you can call this func to make your Method Swizzling


    override func viewDidLoad() {
        super.viewDidLoad()
        self.test1()
        ZeroSwap(cls: ViewController.self , originalSelector: #selector(ViewController.test1), swizzledSelector: #selector(ViewController.test2))
        self.test1()
    }
    
    dynamic func test1() {
        print("i m test1")
    }
    
    dynamic func test2() { 
        print("u print test2 sure")
    }



/// MethodSwizzling , when the swizzled function belong subClass , call this Function .and The cls = superClass   

/// warning : you must to add 'dynamic' before 'func' ; like this : < dynamic func test() { <#your code#>}>


```
public func ZeroSwap(cls : AnyClass,originalSelector : Selector, swizzledSelector : Selector ) {
    
    
    let originalMethod = class_getInstanceMethod(cls, originalSelector)
    let swizzledMethod = class_getInstanceMethod(cls, swizzledSelector)
    // 防止子类替换父类方法,所以分开两种情况
    //第一种情况是要复写的方法(overridden)并没有在目标类中实现(notimplemented)，而是在其父类中实现了。第二种情况是这个方法已经存在于目标类中(does existin the class itself)
    if class_addMethod(cls, originalSelector, method_getImplementation(swizzledMethod), method_getTypeEncoding(swizzledMethod)) {
        class_replaceMethod(cls, originalSelector, method_getImplementation(swizzledMethod), method_getTypeEncoding(swizzledMethod))
    }else {
        method_exchangeImplementations(originalMethod, swizzledMethod)
    }
}

```
///MethodSwizzling , when the swizzled function belong different class and not subClass , call this Function . You should input different class

/// warning : you must to add 'dynamic' before 'func' ; like this : < dynamic func test() { <#your code#>}>


```
public func ZeroSwap(originalCls : AnyClass,originalSelector : Selector,swizzledCls : AnyClass, swizzledSelector : Selector ) {

    method_exchangeImplementations(class_getInstanceMethod(originalCls, originalSelector), class_getInstanceMethod(swizzledCls, swizzledSelector))
    
}

```
// swift 3.0 dispath_once  maybe you need this func to Swizzling method , it can make your code excute once

```
public extension DispatchQueue {
    
    private static var _onceTracker = [String]()
    
    /**
     Executes a block of code, associated with a unique token, only once.  The code is thread safe and will
     only execute the code once even in the presence of multithreaded calls.
     
     - parameter token: A unique reverse DNS style name such as com.vectorform.<name> or a GUID
     - parameter block: Block to execute once
     */
    public class func once(token: String, block:()->Void) {
        objc_sync_enter(self)
        defer { objc_sync_exit(self) }
        
        if _onceTracker.contains(token) {
            return
        }
        
        _onceTracker.append(token)
        block()
    }
}
```
参考:http://www.jianshu.com/p/a6b675f4d073
