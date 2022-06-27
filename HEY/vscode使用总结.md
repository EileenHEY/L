# 安装配置
## 下载材料：
1.  [vscode下载地址](https://code.visualstudio.com/)
2.  [mingow64下载地址](https://www.mingw-w64.org/downloads/)
	***mingow64配置***：
	* Version（选最新）；
	* Architecture（64位电脑选x86_64;32位选i686）；
	* Threads（为win开发程序选win32；为linux、unix、macos等开发程序选posix）；
	* Exception（seh性能好，不支持32位，64位最好选seh；sjlj稳定性好，支持32位）；
	* Build revision（没有选项）*
## 环境配置
>将mingow64文件夹下的bin文件地址放到电脑环境变量path中（右击电脑——>属性——>高级系统设置——>环境变量——>用户变量&系统变量——>path——>编辑——>新建）
## vscode配置
1. 必装插件
	***（1. c/c++***
		扩展设置：
		 ①在C_Cpp › Default: Compiler Path中添加mingw64安装路径中bin文件夹中g++.exe文件路径。（如：C:\Program Files\mingw64\bin\g++.exe）
		 ②在C_Cpp › Default: Include Path中添加mingw64安装路径中lib文件夹的路径。（如：C:\Program Files\mingw64\lib）
		③在C_Cpp › Default: Intelli Sense Mode中指定设置为macos-gcc-x64。
		④指定C_Cpp › Default: Cpp Standard为c++14;指定C_Cpp › Default: C Standard为c11。
	***（2. code runner***
		扩展设置：
		①开启Code-runner: Run In Terminal；
		②开启Code-runner: Save File Before Run。
2. debug调试
***（1. launch.json***
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch",
            "preLaunchTask": "build",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}/${fileBasenameNoExtension}.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": true,
            "MIMode": "gdb",
            "miDebuggerPath": "C:/Program Files/mingw-w64/x86_64-8.1.0-posix-seh-rt_v6-rev0/mingw64/bin/gdb.exe", // 这里修改GDB路径为安装的mingw64的bin下的gdb.exe路径
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ]
        }]
}
```

- ***(2. tasks.json***
```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "build",
            "type": "shell",
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "presentation": {
                "echo": true,
                "reveal": "always",
                "focus": false,
                "panel": "shared"
            },
            "windows": {
                "command": "g++",
                "args": [
                    "-ggdb",
                    "\"${file}\"",
                    "--std=c++11",
                    "-o",
                    "\"${fileDirname}\\${fileBasenameNoExtension}.exe\""
                ]
            }
        }
    ]
}
```