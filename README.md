# ZeroMethodSwizzling
you can call this func to make your Method Swizzling
//
//  ViewController.swift
//  ZeroMethodSwizzling
//
//  Created by Zero on 2016/12/30.
//  Copyright © 2016年 Zero. All rights reserved.
//

import UIKit



class ViewController: UIViewController {

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

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }


}

