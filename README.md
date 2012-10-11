Library goluago aims to become a port of Lua 5.1 interpreter to Go Language Toolchain
(first 8c/6c/5c, ultimately maybe pure Go).

The library is at early stage of development (not feature complete/not ready
for production use), but some important goals were already completed successfully:

STATUS
------

### What works - new in preview 2:

  * [MILESTONE] parsing & compiling Lua scripts to bytecode (via luaL_loadbuffer())
  * [Lua] standard libraries: basic library (print(), ipairs(), assert(), loadstring(), etc.)
  * [Lua] passes parts of the Lua testsuite (tested on fragment of "calls.lua")
  * [API] basic strings API (lua_pushlstring(), lua_tolstring())
  * [API] exposing Go funcs into Lua (lua_pushgofunction(), lua_dump())
  * [tools] Lua script for embedding Lua testsuite parts in Go unit tests (wrap-test.lua)
  * [API, Lua] string <-> number conversions

### What works since preview 1:

  * [API] creating/destroying new Lua state (luaL_newstate(), lua_close())
  * [API] basic integer operations on stack (lua_pushinteger(), lua_gettop(), lua_equal())
  * [API] running simple precompiled Lua code chunk (luaL_loadbuffer(), lua_call(),
    tested on precompiled chunk "return 2+3")
  * compiled and tested on: x86 (8c+8g), x64 (6c+6g), Windows/Linux
  * [tools] Lua script to simplify generation of Go+C wrappers for 
    Lua API functions (internal/gen_wrappers.lua)



WANTED / PLANS
--------------

### Crucial:

  * to implement (leveraging Go fmt.Sprintf) a simplified C sprintf() function,
    in extent enough to cover the handful of use cases present in Lua sources
    (this shall enable more descriptive errors from Lua internals, and make it
    possible to dump the Lua stack to screen via lua_tostring() for easier debugging);
  * to fix/implement all needed C standard library functions marked with FIXME
    (leveraging Go standard library where possible)
    -- the embedded nyi() call shall emit a runtime panic when a stub function is
    called, making them easier to spot.
  * specifically, employ better algorithm for the strstr() function, which now
    uses naive O(n*k) approach
  * MILESTONE: enable rest of the Lua standard library
  * MILESTONE: get goluago to pass the Lua 5.1 testsuite (lua5.1-tests/*.lua)
  * MILESTONE: expose full Lua API in Go (except functions not compatible with Go)

### Would be nice-ies:

  * write unit tests for C standard library functions implemented for Lua;
  * improve C standard library
    * maybe use some non-GPL Open Source code (FreeBSD? some stdlib for embedded systems?);
  * extract C standard library to separate package;
  * later, "ANSI C to Go translator", in extent needed by the Lua sources?

**NOTE:** you're welcome to contribute anything if you fancy, and not necessarily
from this list. The points above are mostly ideas as to what I believe would be 
most useful at this moment.


BACKGROUND INFO
---------------

### Lua?

http://lua.org

Lua is a nice scripting language. Small, written in ANSI C, highly portable,
reportedly fast. Especially popular among computer games producers, but not only.
Easily embeddable (one of its core goals), mature (Lua 1.0 ~ 1993, many applications
worldwide since then). MIT-style licensed. Umm... oh, and I like it.


### Go Language?

http://golang.org

Farly new language developed at Google. Authors are however experienced in
building and experimenting with programming languages (specifically C, Limbo), with 
notable uses, including OS development (Unix, Plan9/Inferno). Highly portable
(x86/x64, ARM; Linux/*nix, Windows, Mac OS X, other ports in progress), written
(bootstrapped) in C. BSD-style licensed. Popularity hard to determine given short
time on market, but has several nice and interesting features and properties
(and I like it too).


INSTALLATION
------------

You need the Go language toolkit version 1 (see: http://golang.org).
With the environment properly set up, type:

    go get github.com/akavel/goluago/internal
    go test github.com/akavel/goluago/internal

To retrieve and run a sample program:

    go get github.com/akavel/goluago/testlua
    cd $GOROOT   # or, see $GOPATH; on Windows %GOROOT%, %GOPATH%
    bin/testlua  # on Windows: bin\testlua.exe


LICENSE
-------


### Where not otherwise specified, MIT-licensed as:


Copyright (c) 2011-2012 Mateusz Czapliński

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.


### Lua code MIT-licensed as:


Copyright (C) 1994-2008 Lua.org, PUC-Rio.  All rights reserved.

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.