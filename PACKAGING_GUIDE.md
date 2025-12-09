# GeoTaichi 打包指南

本指南详细介绍了如何将GeoTaichi项目打包为WHL文件，便于分发和安装。

## 1. 项目概述

GeoTaichi是一个高性能多尺度多物理场模拟框架，基于Taichi构建，支持DEM、MPM和DEMPM等多种模拟方法。

主要模块包括：
- **geotaichi**: 主包，提供统一的API接口
- **src/dem**: 离散元方法模块
- **src/mpm**: 物质点方法模块
- **src/mpdem**: DEM-MPM耦合模块
- **src/utils**: 通用工具函数

## 2. 打包前的准备工作

### 2.1 安装必要的构建工具

```bash
pip install --upgrade setuptools wheel build
```

### 2.2 确保项目文件完整

在打包前，请确保以下文件存在且内容正确：

- **pyproject.toml**: 项目配置文件
- **README.md**: 项目说明文档
- **LICENSE**: 许可证文件

## 3. 详细的打包步骤

### 3.1 创建/验证pyproject.toml文件

pyproject.toml文件包含了项目的元数据和构建配置，内容如下：

```toml
[build-system]
requires = ["setuptools>=61.0"]
build-backend = "setuptools.build_meta"

[project]
name = "geotaichi"
version = "0.1.0"
authors = [
  { name = "Shi-Yihao, Guo-Ning", email = "zju.multiscale.geomechanics@gmail.com" }
]
description = "A High Performance Multiscale and Multiphysics Simulator"
readme = "README.md"
license = { file = "LICENSE" }
classifiers = [
    "Programming Language :: Python :: 3",
    "License :: OSI Approved :: GNU General Public License v3 (GPLv3)",
    "Operating System :: OS Independent",
    "Intended Audience :: Science/Research",
    "Topic :: Scientific/Engineering :: Physics",
]
requires-python = ">=3.8"
dependencies = [
    "taichi==1.6.0",
    "scipy",
    "trimesh",
    "imageio",
    "matplotlib",
    "psutil",
    "pynvml",
    "shapely"
]

[tool.setuptools.packages.find]
where = [".", "src"]

[tool.setuptools.package-data]
geotaichi = ["assets/**/*", "asserts/**/*"]
"src" = ["**/*.py"]
```

### 3.2 清理之前的构建文件（可选）

如果之前进行过构建，建议清理之前的构建文件：

```bash
rm -rf build dist *.egg-info
```

在Windows上，可以使用：

```bash
rd /s /q build dist *.egg-info
```

### 3.3 执行构建命令

使用Python的build模块进行构建：

```bash
python -m build --wheel --no-isolation
```

- `--wheel`: 指定生成为wheel格式（.whl文件）
- `--no-isolation`: 在当前环境中构建，不创建隔离环境

构建成功后，WHL文件将生成在`dist`目录下。

### 3.4 测试构建（可选）

如果只是想测试构建过程，不实际生成分发文件，可以指定输出目录为临时目录：

```bash
python -m build --wheel --no-isolation --outdir .\test_build
```

## 4. 使用打包后的WHL文件

### 4.1 安装WHL文件

构建完成后，可以通过以下命令安装生成的WHL文件：

```bash
pip install dist/geotaichi-0.1.0-py3-none-any.whl
```

### 4.2 在Python中导入使用

安装完成后，可以在Python中导入并使用GeoTaichi：

```python
import geotaichi as gi

# 初始化GeoTaichi
gi.init(arch="gpu", default_fp="float64")

# 创建DEM模拟
dem = gi.DEM()

# 创建MPM模拟
mpm = gi.MPM()

# 创建DEMPM耦合模拟
dempm = gi.DEMPM()
```

## 5. 常见问题和解决方案

### 5.1 依赖安装失败

如果遇到依赖安装问题，可以尝试使用不同的PyPI镜像源：

```bash
pip install --upgrade setuptools wheel -i https://pypi.org/simple/
```

### 5.2 构建错误

- **pyproject.toml解析错误**：检查文件格式和语法，确保没有重复的配置块
- **文件权限问题**：确保当前用户有足够的权限创建和写入文件
- **包数据未包含**：检查`package-data`配置，确保包含了所有需要的文件

### 5.3 导入错误

如果安装后无法导入模块，可能是因为：
- 安装路径不在Python的搜索路径中
- 依赖项版本不兼容
- 打包时遗漏了某些必要文件

## 6. 高级选项

### 6.1 指定Python版本

可以在构建时指定目标Python版本：

```bash
python -m build --wheel --no-isolation --python-tag py39
```

### 6.2 生成源代码分发

除了wheel格式外，还可以生成源代码分发（sdist）：

```bash
python -m build --sdist
```

### 6.3 上传到PyPI（可选）

如果要将包发布到PyPI，可以使用twine：

```bash
pip install twine
twine upload dist/*
```

## 7. 注意事项

1. 确保项目版本号遵循语义化版本规范（MAJOR.MINOR.PATCH）
2. 定期更新依赖项版本，确保安全性和兼容性
3. 在打包前进行充分的测试，确保功能正常
4. 更新README.md和其他文档，反映最新的项目状态

---

**最后更新时间**：2024年
**维护者**：multiscale geomechanics lab, Zhejiang University
