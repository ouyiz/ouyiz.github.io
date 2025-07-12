1.获得许可
2.下载安装包
[Gurobi Software - Gurobi Optimization](https://www.gurobi.com/downloads/gurobi-software/)
3.把压缩包放到服务器
4.解压
```
tar -zxvf gurobi12.0.2_linux64.tar.gz
```
5.进入文件夹
```
cd gurobi1202/linux64/bin
```

6.激活
注意此处用户不可以是`root`
```
grbgetkey xxxxx(激活码)
```
激活成功后会有`gurobi.lic`文件，位置在激活的输出中有显示。