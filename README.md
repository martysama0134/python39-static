# python39-static-master
python3.9.6 compiled statically on vs2019

To make it static, I've enabled Py_NO_ENABLE_SHARED MS_NO_COREDLL in every project. It's currently in MT MTd.

To make sure the library is linked properly in the program, you have to edit pyconfig.h.

At the beginning of pyconfig.h add:

```cpp
#ifndef Py_NO_ENABLE_SHARED
#define Py_NO_ENABLE_SHARED
#endif

#ifndef MS_NO_COREDLL
#define MS_NO_COREDLL
#endif
```

For the linking you need:
```cpp

/* For an MSVC DLL, we can nominate the .lib files used by extensions */
#ifdef MS_COREDLL
#       if !defined(Py_BUILD_CORE) && !defined(Py_BUILD_CORE_BUILTIN)
                /* not building the core - must be an ext */
#               if defined(_MSC_VER)
                        /* So MSVC users need not specify the .lib
                        file in their Makefile (other compilers are
                        generally taken care of by distutils.) */
#                       if defined(_DEBUG)
#                               pragma comment(lib,"python39_d.lib")
#                       elif defined(Py_LIMITED_API)
#                               pragma comment(lib,"python3.lib")
#                       else
#                               pragma comment(lib,"python39.lib")
#                       endif /* _DEBUG */
#               endif /* _MSC_VER */
#       endif /* Py_BUILD_CORE */
#else
#   if defined(_DEBUG)
#       pragma comment(lib,"python39_d-static.lib")
#   else
#       pragma comment(lib,"python39-static.lib")
#   endif /* _DEBUG */
#   pragma comment(lib,"Pathcch.lib")
#endif /* MS_COREDLL */
```
