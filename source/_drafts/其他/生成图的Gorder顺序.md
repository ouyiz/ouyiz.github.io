
## ✅ 第一步：准备环境和代码

1. **克隆项目代码（包含子模块）**：
    
    ```bash
    git clone --recurse-submodules https://github.com/lecfab/rescience-gorder.git
    cd rescience-gorder
    ```
    
2. **安装依赖**：
    
    - Python：确保你有 Python 3.8+ 和以下包：
        
        ```bash
        pip install matplotlib numpy
        ```
        
    - C++ 编译环境：需支持 C++14，例如：
        
        ```bash
        sudo apt install build-essential
        ```
        
    - Perf 工具（可选，用于性能分析）：
        
        ```bash
        sudo apt install linux-tools-common
        ```
        

---

## ✅ 第二步：准备图数据

1. 将你的原始图数据文件（例如 `mygraph.txt`）放入目录：
    
    ```
    rescience-gorder/datasets/dl/
    ```
    
2. 运行标准化脚本，将数据格式转换为项目支持的格式：
	添加如下代码到 `normalise.py` 中，例如加在 `elif name == 'epinion':` 的下面：
```python
	elif sys.argv[1] == "wikitalk":
    file = "dl/WikiTalk.txt"
    f = open(file)
    l = f.readline()
    output = ""

    n, m = -1, 0
    while len(l):
        if l.strip() != "":  # 跳过空行
            nums = l.strip().split("\t")
            if len(nums) != 2:
                l = f.readline()
                continue
            u, v = int(nums[0]), int(nums[1])
            output += outputFormat(u, v)
            m += 1
            n = max([n, u, v])
            if m % 1000000 == 0: print(m)
        l = f.readline()
    f.close()
    createFile(output, n, m)
```

```bash
    cd datasets
    python3 normalise.py mygraph
    ```
    
它会读取 `dl/mygraph.txt`，输出为 `edgelist-mygraph-xxx.txt`，该文件是一个标准化后的边列表（格式为：每行 `a b`，节点编号从 0 开始且连续）。
    

---

## ✅ 第三步：生成 Gorder 排序文件

1. **进入编译目录并编译代码**：
    
    ```bash
    cd ../src
    make
    ```
    
2. **执行 Gorder 排序**（替换成你生成的 edgelist 文件名）：
    
    ```bash
    ./ord ../datasets/edgelist-mygraph-xxx.txt gorder -d -o mygraph-gorder.txt
    ```
    
    - 参数说明：
        
        - `../datasets/edgelist-mygraph-xxx.txt`：标准化后的图文件路径；
            
        - `gorder`：使用 Gorder 排序算法；
            
        - `-d`：表示输入是有向图；
            
        - `-o mygraph-gorder.txt`：输出排序文件，一行一个节点的新编号。
            

---
