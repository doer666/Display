Vscode mingw下opencv及多线程C++socket—Apple的学习笔记

	目的：由于vslam都是用c++开发的，所以要多练习c++11，所以就自己设计项目来练手
	立项：（适用于0.1版本）
	名称：Display
	功能：一个client将数据集中的png图片一张张读取，并且通过tcp socket协议传输给server端进行显示。
	     特殊控制功能包括Server端的按键s是控制socket传输数据的开关，按键j是控制是否将当前传输的图片数据实时显示的开关。
	工具链：coding用Vscode，编译用CMake+MinGw8.1（环境搭建可以参考我之前的文章）
	数据集：在client端是读取png的数据集
	工程共享路径: https://github.com/AppleCai/Display

设计说明：详见Design文件夹中的word说明文档
20190519更新为0.2版本，对应code在02文件夹中。变更说明

	1. 重新设计了交互协议。对于接收头及数据的解析使用了状态模式设计。
	2. 添加了心跳检测功能：server每隔1s发给client，client检查若5s内没收到则断开连接。
	3. 数据/控制/显示进行了分离设计，删除了之前使用线程挂起的方式来实现暂停方式。
	4. 代码进行了重构，线程由原来的socket和key，还增加了HMI和控制线程。
	5. 文件结构变更，拆分为如下6个模块
		socket主要用来传输命令和数据。
		控制模块主要控制是否正常传输数据，是否传输心跳数据命令，是否暂停显示。
		key主要扫描s键(是否暂停传输)，j键（是否暂停显示）

20190526更新为0.3版本，对应code在03文件夹中。变更说明
	1. 将Server端的接收改成select异步模式，使能断线后重连。
	2. 通过opencv图片叠加，添加logo及接收到的帧进行计数，显示页数。故增加了pic文件夹。
	3. 移植到POSIX平台进行开发。
	4. 对应client的测试code在进行sokect重新connect时候，使用了select异步IO模型，设置timeout为1s，加快了重连速度减少阻塞时间。
