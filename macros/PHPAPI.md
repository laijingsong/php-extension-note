# macros
### PHPAPI
php内核源码中，很多地方可以看到PHPAPI这个宏。这个宏是做什么的呢。
我们来看它的定义
```
php.h
#ifdef PHP_WIN32
#	include "tsrm_win32.h"
#	include "win95nt.h"
#	ifdef PHP_EXPORTS
#		define PHPAPI __declspec(dllexport)
#	else
#		define PHPAPI __declspec(dllimport)
#	endif
#	define PHP_DIR_SEPARATOR '\\'
#	define PHP_EOL "\r\n"
#else
#	if defined(__GNUC__) && __GNUC__ >= 4
#		define PHPAPI __attribute__ ((visibility("default")))
#	else
#		define PHPAPI
#	endif
#	define THREAD_LS
#	define PHP_DIR_SEPARATOR '/'
#	define PHP_EOL "\n"
#endif
```
这其实是跟编译有关的一个宏:gcc编译控制动态库导出函数。
从以上代码可以看出，当定义了 __GNUC__ 并且 版本大于4，就会替换为：__attribute__ ((visibility("default"))，当gcc版本小于四时，是没有意义，这个宏是没 有值 的。
> __attribute__ ((visibility("default"))) 相当于window平台下面的__declspec(dllexport)

##### gcc的visbility属性说明：
1.当-fvisibility=hidden时
动态库中的函数默认是被隐藏的即 hidden. 除非显示声明为__attribute__((visibility("default"))).
2.当-fvisibility=default时
动态库中的函数默认是可见的.除非显示声明为__attribute__((visibility("hidden"))).

参考文章:[ GCC系列: __attribute__((visibility("")))](http://blog.csdn.net/veryitman/article/details/46756683)