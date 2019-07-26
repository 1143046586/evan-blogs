   一直以来，大家对开发的代码编辑器都没有一个最好的选择，而vscode是我发现速度相对快，对于新版的html css js元素及属性过滤、提示最准确的代码编辑器，因为插件太多，有的人不知道选择哪些插件最好，本文档教你安装哪些，那些可选,安装如下：

### vscode推荐插件的安装(原文)
https://blog.csdn.net/joyce198800/article/details/79453180


01、open in browser:在浏览器运行预览（alt+b），安装后自动运行√√√

02、OneDark：来自Atom的主题，颜色更为柔和舒服，不伤眼睛的主题，ctrl+k加ctrl+t安装后选择主题√√√

03、auto rename tag:修改HTML标签时，自动修改匹配的标签，安装后自动运行√√√

04、beautify: 可以格式化JSON|JS|HTML|CSS|SCSS,比内置格式化好用，安装后自动运行√√√

05、path autocomplete:路径智能补全，比path intellisense强，可以连续提示，不用按“/”，安装后自动运行√√√

06、filesize:在底部状态栏显示当前文件大小，点击后还可以看到详细创建、修改时间，安装后自动运行√√√

07、html css support:让 html 标签上写class智能提示当前项目所支持的样式（支持vue,内置不支持），安装后自动运行√√√

08、html snippets(0.1.5版本):超级实用且初级的 H5代码片段以及提示，安装后自动运行√√√

09、IntelliSense for css class names：css class输入提示，安装后自动运行√√√

10、css peek:在当前页面自动查找CSS文件并显示内容，安装后自动运行√√√

11、Document this：Js的注释模板，重复按两次ctrl+alt+d，即可添加函数的注释√√√

12、eslint:检测js必备，安装后自动运行，测试安装后自动运行√√√

13、Can I Use：HTML5、CSS3、SVG的浏览器兼容性检查安装后自动运行√√√

14、GBKTOUTF-8:将文件GBK转换为utf-8编码安装后自动运行√√√

15、code spell check：代码单词检测安装后自动运行√√√


15、Debugger for Chrome：方便调试，选安○○○

16、vetur:语法高亮、智能感知、Emmet等，选安○○○

17、Vue 2 Snippets：vue必备，选安○○○

18、VueHelper：Vue2代码段（包括Vue2
api、vue-router2、vuex2），选安○○○

19、git history:可以查看Git log, file, 和line 历史记录，选安○○○

20、code runner: 代码编译运行看结果，支持众多后端语言，选安○○○


21、path intellisense:自动路径提示，默认不带这个功能的，path intellisense更智能，所以无需要安装
22、Guides：代码对齐辅助线，新版vscode已内置该功能，无需安装。

23、auto close tag:自动闭合HTML标签，新版vscode已内置该功能，无需安装。
24、file peek:鼠标移到路径名按住ctrl可打开文件，系统已内置，，无需安装。

25、background：修改vscode的背景，多余不使用，使用系统默认即可。

26、vscode-icons:让vscode资源树目录加上图标，在首选项文件图标主题中选择，多余不使用，使用系统默认即可
27、eclipse keymap:eclipse快捷键，安装后自动运行，修改vscode的快捷键即可。


快捷键：

    1、格式化代码：ctrl+alt+f
    2、建议触发：alt+/


以下为vscode的用户配置：


    {
        // 是否自动保存
        "files.autoSave": "off",
        // git.path的可执行文件路径
        "git.path": "C:/Program Files/Git/bin/git.exe",
        "editor.renderControlCharacters": true,
        //设置主题为OneDark++
        "workbench.colorTheme": "Solarized Light",
        // 显示空格
        "editor.renderWhitespace": "all",
        //自动补齐文件路径时，带入扩展名
        "path-autocomplete.extensionOnImport": true,
        //使autocompletion以外的路径字符串。
        // 控制键入时是否应自动显示建议
        "editor.quickSuggestions": {
            "other": true,
            "comments": true,
            "strings": true
        },
        // 启用后，将在保存文件时剪裁尾随空格。
        "files.trimTrailingWhitespace": true,
        // 启用后，将使用的参数和方法名称的类型进行提示。
        "docthis.inferTypesFromNames": true,
        // 当 editor.cursorStyle 设置为 "line" 时控制光标的宽度。
        "editor.cursorWidth": 0,
        // 总是显示ESLint状态栏
        "eslint.alwaysShowStatus": true,
        // 打开自动修复保存或关闭
        "eslint.autoFixOnSave": true,
        // 每次保存文件（ctrl+s）时，eslint插件会自动对当前文件进行eslint语法修正！
        "eslint.validate": [
            "javascript",
            "javascriptreact",
            "html",
            {
                "language": "vue",
                "autoFix": true
            }
        ],
        "eslint.options": {
            "plugins": [
                "html"
            ]
        },
        //为了符合eslint的两个空格间隔原则
        "editor.tabSize": 4,
        // 控制编辑器是否应在键入后自动设置行的格式
        "editor.formatOnType": true,
        // 启用/禁用 HTML 标记的自动关闭。
        "html.autoClosingTags": true,
        // 启用后，按下 TAB 键，将展开 Emmet 缩写。
        "emmet.triggerExpansionOnTab": true,
        // 以像素为单位控制字号。
        "editor.fontSize": 13,
    }