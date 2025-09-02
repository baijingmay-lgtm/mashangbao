# 项目说明

## Tailwind CSS 生产环境配置指南

您的项目当前使用了Tailwind CSS的CDN版本，但在生产环境中，建议使用本地安装的方式以获得更好的性能和安全性。

### 为什么不应该在生产环境中使用CDN版本?
- CDN版本可能包含您未使用的CSS类，导致加载不必要的代码
- 依赖第三方CDN服务可能导致可用性问题
- 无法自定义Tailwind配置
- 无法使用高级功能如JIT编译和purge

### 如何在生产环境中使用Tailwind CSS

#### 步骤1: 安装Node.js
如果您还没有安装Node.js，请从[Node.js官网](https://nodejs.org/)下载并安装。

#### 步骤2: 初始化项目
打开命令行，导航到项目目录，运行以下命令初始化Node.js项目:
```bash
npm init -y
```

#### 步骤3: 安装Tailwind CSS
```bash
npm install tailwindcss postcss-cli autoprefixer
```

#### 步骤4: 创建Tailwind配置文件
```bash
npx tailwindcss init -p
```
这将创建`tailwind.config.js`和`postcss.config.js`文件。

#### 步骤5: 配置Tailwind
编辑`tailwind.config.js`文件，添加您的HTML文件路径，以便Tailwind可以purge未使用的CSS:
```javascript
module.exports = {
  content: ['./*.html', './web/**/*.html'],
  theme: {
    extend: {
      colors: {
        primary: '#165DFF',
      },
    },
  },
  plugins: [],
}
```

#### 步骤6: 创建Tailwind输入文件
我们已经创建了`resources/css/tailwind.css`文件，其中包含了必要的Tailwind指令和自定义工具类。

#### 步骤7: 添加构建脚本
编辑`package.json`文件，添加以下脚本:
```json
{
  "scripts": {
    "build:css": "tailwindcss -i ./resources/css/tailwind.css -o ./resources/css/tailwind.min.css --minify"
  }
}
```

#### 步骤8: 构建生产版本的CSS
```bash
npm run build:css
```
这将生成一个最小化的CSS文件`resources/css/tailwind.min.css`。

#### 步骤9: 更新HTML文件引用
将HTML文件中的Tailwind引用从`tailwind.css`改为`tailwind.min.css`:
```html
<link href="../resources/css/tailwind.min.css" rel="stylesheet">
```

### 临时解决方案
如果您暂时无法设置Node.js环境，可以继续使用CDN版本，但添加purge配置以减少CSS体积。我们已经在HTML文件中添加了相关配置:
```javascript
tailwind.config = {
  content: ['./*.html', './web/**/*.html'],
  theme: {
    extend: {
      colors: {
        primary: '#165DFF',
      },
    },
  }
}
```

### 注意事项
- 生产环境中请务必使用本地构建的Tailwind CSS
- 每次修改HTML文件后，需要重新运行`npm run build:css`以更新CSS文件
- 定期更新Tailwind CSS版本以获取最新功能和安全修复