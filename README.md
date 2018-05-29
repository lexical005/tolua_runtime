# tolua_runtime
**Build**<br>
pc: build_win32.sh build_win64.h  (mingw + luajit2.0.4) <br>
android: build_arm.sh build_x86.sh (mingw + luajit2.0.4) <br>
mac: build_osx.sh (xcode + luac5.1.5 for luajit can't run on unity5) <br>
ios: build_ios.sh (xcode + luajit2.1 beta) <br>

NDK 版本:android-ndk-r10e 默认安装到 D:/android-ndk-r10e<br>
https://dl.google.com/android/repository/android-ndk-r10e-windows-x86_64.zip<br>
Msys2配置说明<br>
https://github.com/topameng/tolua_runtime/wiki<br>
配置好的Msys2下载<br>
https://pan.baidu.com/s/1c2JzvDQ<br>

# Libs
**cjson**<br>
https://github.com/mpx/lua-cjson<br>
**protoc-gen-lua**<br>
https://github.com/topameng/protoc-gen-lua<br>
**LuaSocket** <br>
https://github.com/diegonehab/luasocket<br>
**struct**<br>
http://www.inf.puc-rio.br/~roberto/struct/<br>
**lpeg**<br>
http://www.inf.puc-rio.br/~roberto/lpeg/lpeg.html

编译tolua_runtime
1. 从github下载tolua_runtime源码

2. 下载android-ndk-r10e，解压后将路径添加到系统PATH路径

3. 下载配置好的msys2，解压后将路径添加到系统PATH路径

4. 如果需要，重启电脑后，新增的系统PATH路径才会生效

5. 编译Windows平台tolua.dll

5.1 运行msys2/mingw64_shell.bat

5.2 进入tolua_runtime源码目录

5.3	键入并执行：./build_win64.sh

6. 编译android平台libtolua.so

6.1 修改build_arm.sh中的NDK路径为本地路径

6.2 运行msys2/mingw32_shell.bat

6.3 进入tolua_runtime源码目录 

6.4	键入并执行：./build_arm.sh

6.5 NDKABI修改成21后会导致报错：jni/libluajit.a(lj_profile.o):lj_profile.c:function luaJIT_profile_start: error: undefined reference to 'sigemptyset'

备注:
sproto代码，被修改了2处，一处是删除lua_tointegerx：

#if LUA_VERSION_NUM < 502
static int64_t lua_tointegerx(lua_State *L, int idx, int *isnum) {
	if (lua_isnumber(L, idx)) {
		if (isnum) *isnum = 1;
		return (int64_t)lua_tointeger(L, idx);
	}
	else {
		if (isnum) *isnum = 0;
		return 0;
	}
}
#endif

一处是删除LUAMOD_API 