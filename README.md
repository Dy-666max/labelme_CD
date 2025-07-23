# LABELME_CD

> 基于 [labelme](https://github.com/wkentaro/labelme/tree/v5.1.0) 构建的用于变化检测的图像标注工具。

## 项目简介

LABELME_CD 是一个专门用于变化检测任务的图像标注工具，支持同时标注两个时相的图像数据，具有以下特性：

- 支持变化检测专用的标注功能
- 支持两张图分别进行亮度/对比度调节
- 支持频域对齐和直方图校正
- 支持目标级和像素级标注
- 基于 PyQt5 构建的现代化图形界面

https://user-images.githubusercontent.com/29772895/207556216-cb4934b4-f9ed-42c4-a2a2-d8fa9ad1b63e.mp4

## 系统要求

- **操作系统**: Windows 10/11, macOS, Linux
- **Python**: 3.8 或更高版本
- **内存**: 建议 4GB 以上
- **显卡**: 支持 OpenGL 的显卡（用于图形界面）

## 安装说明

### 方法一：使用 Conda 环境（推荐）

```bash
# 1. 创建新的 conda 环境
conda create -n labelme_cd python=3.8 -y

# 2. 激活环境
conda activate labelme_cd

# 3. 克隆项目
git clone https://github.com/shinianzhihou/labelme_cd.git
cd labelme_cd

# 4. 安装依赖
pip install PyQt5 qtpy numpy Pillow PyYAML termcolor natsort colorama imgviz matplotlib opencv-python tqdm

# 5. 验证安装
python -c "import labelme; print('安装成功！')"
```

### 方法二：直接使用 pip

```bash
# 1. 确保使用 Python 3.8+
python --version

# 2. 克隆项目
git clone https://github.com/shinianzhihou/labelme_cd.git
cd labelme_cd

# 3. 安装依赖
pip install PyQt5 qtpy numpy Pillow PyYAML termcolor natsort colorama imgviz matplotlib opencv-python tqdm

# 4. 验证安装
python -c "import labelme; print('安装成功！')"
```

### 通过代理安装依赖（如需）

```bash
pip install <包名> --proxy http://127.0.0.1:7897
# 例如
pip install opencv-python --proxy http://127.0.0.1:7897
pip install tqdm --proxy http://127.0.0.1:7897
```

### 安装问题排查

如果遇到安装问题，请尝试以下解决方案：

1. **conda 命令无法识别**：
   ```bash
   # Windows 用户需要将 Anaconda 路径添加到环境变量
   # 手动添加以下路径到系统 PATH：
   # D:\Anaconda
   # D:\Anaconda\Scripts  
   # D:\Anaconda\condabin
   ```

2. **PyQt5 安装失败**：
   ```bash
   pip install --force-reinstall --no-deps PyQt5
   pip install PyQt5-sip PyQt5-Qt5
   ```

3. **权限错误**：
   ```bash
   # 使用用户权限安装
   pip install --user [包名]
   ```

## 使用方法

### 1. 数据准备

创建如下的目录结构：

```
your_project/
├── A/              # 第一时相图像
│   ├── image1.jpg
│   ├── image2.jpg
│   └── ...
├── B/              # 第二时相图像  
│   ├── image1.jpg
│   ├── image2.jpg
│   └── ...
└── label/          # 标注结果存储（自动创建）
    ├── image1.json
    ├── image2.json
    └── ...
```

**重要说明**：
- `A/` 和 `B/` 目录下的图像文件名必须一一对应
- 支持常见图像格式：`.jpg`, `.png`, `.bmp`, `.tiff` 等
- 可以使用软连接方式组织数据

### 2. 启动应用

#### 方式一：启动后选择目录
```bash
# 激活环境
conda activate labelme_cd

# 启动应用
python -m labelme.__main__

# 然后在界面中选择 A/ 或 B/ 目录
```

#### 方式二：直接指定目录
```bash
# 标注第一时相
python -m labelme.__main__ ./your_project/A/

# 标注第二时相  
python -m labelme.__main__ ./your_project/B/
```

#### 方式三：使用示例数据
```bash
# 项目提供了示例数据
python -m labelme.__main__ ./assets/examples/change_detection/A/
```

### 3. 标注操作

1. **加载图像**：
   - 启动后会自动加载目录中的图像
   - 使用 `←` `→` 键或菜单切换图像

2. **创建标注**：
   - 点击 "Create Polygons" 创建多边形标注
   - 点击 "Create Rectangle" 创建矩形标注
   - 点击 "Create Circle" 创建圆形标注

3. **调整显示**：
   - 使用亮度/对比度滑块调整图像显示
   - 支持缩放和平移操作

4. **保存标注**：
   - 标注会自动保存为 JSON 格式
   - 保存位置：`label/` 目录下

### 4. 导出结果

将标注转换为 mask 格式（可选）：

```bash
python tools/labelme2mask.py -i ./your_project/label/ -o ./your_project/masks/ -f 255
```

参数说明：
- `-i`: 输入标注文件目录
- `-o`: 输出 mask 文件目录  
- `-f`: mask 前景像素值（默认 255）

## 项目结构

```
labelme_cd/
├── labelme/           # 核心代码
│   ├── app.py        # 主应用程序
│   ├── widgets/      # GUI 组件
│   └── utils/        # 工具函数
├── tools/            # 辅助工具
│   └── labelme2mask.py
├── assets/           # 示例数据
│   └── examples/
└── README.md         # 项目说明
```

## 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+O` | 打开图像 |
| `Ctrl+S` | 保存标注 |
| `←` `→` | 切换图像 |
| `Ctrl+Z` | 撤销操作 |
| `Ctrl+Y` | 重做操作 |
| `Delete` | 删除选中标注 |
| `Escape` | 取消当前操作 |

## 常见问题

### Q: 如何标注变化区域？
A: 分别在 A/ 和 B/ 目录的对应图像上标注相同区域，系统会自动关联两个时相的标注。

### Q: 支持哪些标注类型？
A: 支持多边形、矩形、圆形、点等多种标注类型，适用于不同的变化检测任务。

### Q: 如何批量处理？
A: 可以编写脚本调用 `tools/` 目录下的工具进行批量转换和处理。

### Q: 标注文件格式是什么？
A: 使用 JSON 格式存储，兼容原版 labelme 格式，包含坐标、类别、属性等信息。

## 技术支持

- **项目主页**: https://github.com/shinianzhihou/labelme_cd
- **问题反馈**: 请在 GitHub Issues 中提交
- **原版 labelme**: https://github.com/wkentaro/labelme

## 开源协议

本项目基于 GPLv3 开源协议。

## 更新日志

### v5.1.0
- 基于 labelme v5.1.0 构建
- 增加变化检测专用标注功能
- 支持双时相图像对比标注
- 优化图像显示和调整功能
