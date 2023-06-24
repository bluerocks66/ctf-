# angr的使用
## 安装python虚拟环境并以安装angr为例

```bash
apt-get install python3-dev libffi-dev build-essential virtualenvwrapper
whereis virtualenvwrapper.sh
source /usr/share/virtualenvwrapper/virtualenvwrapper.sh
mkvirtualenv --python=$(which python3) angr && pip install angr
```
其实source这个可以写进.bashrc文件，这样就不用每次source一下
```bash
source /usr/share/virtualenvwrapper/virtualenvwrapper.sh
```
workon可以看到我们有哪些环境
```bash
root@bogon:~# workon
angr
```
切换进入只要后面加环境的名字即可
```bash
root@bogon:~# workon angr
(angr) root@bogon:~#
```
退出虚拟环境
```bash
(angr) root@bogon:~# deactivate
root@bogon:~#
```
## Angr的基本操作

```py
p = angr.Project(‘filename’)#加载可执行程序
flag = claripy.BVS(‘flag’,length*8)#创建一个长度为length的符号向量作为输入
state = p.factory.entry_state()#创建一个状态,默认为程序的入口地址
simgr = p.factory.simgr(state)#构建一个模拟管理器
simgr.explore(find=,avoid=) #寻找正确路径并避开错误路径
simgr.found[0].posix.dumps(0) #获取找到正确路径时的输入值。
# dumps(0)表示输入， dumps(1)表示输出
```
使用手册：https://angr.io/api-doc/


## Angr的解题流程
```
①.新建一个Angr工程，并且载入二进制文件
②.创建程序入口点的state
③.将要求解的变量符号化
④.创建SimulationManager进行程序执行管理
⑤.搜索满足我们目标的state
⑥.求解程序执行到state时，符号化变量所需要的约束条件
⑥.解出约束条件，获得目标值
```

```py
import angr
import claripy

def main():
    #新建一个工程，导入二进制文件，example就是文件的名字，后面的选项是选择不自动加载依赖项
    proj = angr.Project('example',load_options={"auto_load_libs":False})
    #创建一个SimState对象，这也就是我们进行符号执行的核心对象，包括了符号信息、内存信息、寄存器信息等
    state = proj.factory.entry_state(add_options={"SYMBPLIC_WRITE_ADDRESS"})
    #创建一个符号变量，这个符号变量以8位bitvector形式存在，名称为u
    u = claripy.BVS("u",8)
    #把符号变量保存到指定的地址中，这个地址是和二进制文件相关联的，使用IDA打开，此地址对应的.bss段全局变量u的地址
    state.memory.store(0x601041,u) #我对代码进行过重新编译，因此和github提供的二进制文件的地址不相同
    #创建一个Simulation Manager对象，这个对象和我们的状态有关系
    sm = proj.factory.simulation_manager(state)
    #step开始执行，直到路径超过一条的时候停止，也就是我们的if else那里
    sm.step(until = lambda lpg:len(lpg.active) > 1)
    #对于每个可能的state都进行检查，
    for I in range(len(sm.active)):
        print "possible %d : " %I , sm.active[I].state.se.any_int(u)
    return sm.active[0].state.se.any_int(u)


if __name__ == "__main__":
    print repr(main())
```
