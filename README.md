nbind
=====

nbind is a bindings generator for Node.js plugins inspired by [embind](http://kripken.github.io/emscripten-site/docs/porting/connecting_cpp_and_javascript/embind.html) from [emscripten](http://emscripten.org). The bindings are built along with your C++ library without requiring any generated code, using C++11 templates and simple declarations.

It's at an alpha stage and many embind features are missing, but for usage instructions much of the [embind documentation](http://kripken.github.io/emscripten-site/docs/porting/connecting_cpp_and_javascript/embind.html) is appropriate. The included example illustrates the basic idea, a simple C++ Point class is defined with no JavaScript-related code. Only the following is required to add support for using it from JavaScript:

    #include "Binding.h"

    using namespace nbind;

    NODEJS_BINDINGS(Point) {
        class_<Point>("Point")
            .constructor<>()
            .constructor<double,double>()
            .function("add",&Point::add)
            .function("print",&Point::print);
    }

    NODE_MODULE(example,Bindings::initModule)

It can then be used like this:

    var Point=require('./build/Release/example.node').Point;

    var a=new Point(1,2);
    var b=new Point(10,20);

    a.add(b);

To compile the example, install required packages for both nbind and the example, build and run inside the `example` directory:

    npm install
    cd example
    npm install
    node point.js

License
=======

[The MIT License](https://raw.githubusercontent.com/jjrv/nbind/master/LICENSE)
Copyright (c) 2014 BusFaster Ltd
