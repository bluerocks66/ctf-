# .swp文件
非正常关闭vi编辑器时会生成一个.swp文件
使用vi，经常可以看到swp这个文件,那这个文件是怎么产生的呢，当你打开一个文件，vi就会生成这么一个.(filename)swp文件 以备不测（不测下面讨论），如果你正常退出，那么这个这个swp文件将会自动删除 。下面说不测。
不测分为：
```
1、当你用多个程序编辑同一个文件时。
2、非常规退出时。
```
第一种情况的话，为了避免同一个文件产生两个不同的版本（vim中的原话），还是建议选择readonly为好。
第二种情况的话，你可以用vim -r filename恢复，然后再把swp文件删除（这个时候要确保你的swp文件没有用处了，要不然你会伤心的）
swp文件的来历，当你强行关闭vi时，比如电源突然断掉或者你使用了Ctrl+ZZ，vi自动生成一个.swp文件，下次你再编辑时，就会出现一些提示。

你可以使用
```bash
vi -r {your file name}
```
来恢复文件，然后用下面的命令删除swp文件，不然每一次编辑时总是有这个提示。
```bash
rm .{your file name}.swp
```
在网上搜到了一个类似的提示，不同的linux提示可能不一样
```bash
“.xorg.conf.swp” already exists!
[O]pen Read-Only, (E)dit anyway, (R)ecover, (Q)uit:
```
当然可以用R键恢复。
vi编辑器要正常退出可以使用Shift-ZZ 。
