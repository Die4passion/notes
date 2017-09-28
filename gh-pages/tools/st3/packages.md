#打造自己顺手的代码编辑器

基本配置:
:   所有插件都可以命令安装，所以先安装package control

````
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
````

> 我的`Settings-User`:

````json
{
    //没有打开文件时不关闭窗口
    "close_windows_when_empty": false,
    //使用的主题是boxy主题的一款暗色主题
    "color_scheme": "Packages/Boxy Theme/schemes/Boxy Solarized Dark.tmTheme",
    //设置显示空格
    "draw_white_space": "all",
    //设置字体大小11
    "font_size": 11,
    //高亮选中行
    "highlight_line": true,
    //不使用的包，(暂时禁用的会在这里，可以package control里面disable)
    "ignored_packages":
    [
        "CodeIgniter Snippets",
        "JsFormat",
        //vim模式,还不习惯，所以禁止
        "Vintage"
    ],
    "material_theme_accent_cyan": true,
    "material_theme_contrast_mode": true,
    "material_theme_small_tab": true,
    "material_theme_tabs_autowidth": true,
    "show_encoding": true,
    "show_full_path": false,
    "theme": "Boxy Solarized Dark.sublime-theme",
    "theme_accent_green": true,
    "theme_font_sm": true,
    "theme_icons_materialized": true,
    "theme_scrollbar_colored": true,
    "theme_scrollbar_semi_overlayed": true,
    "theme_sidebar_heading_bold": true,
    "theme_size_sm": true,
    "theme_tab_mouse_wheel_switch": true,
    "theme_tab_selected_label_bold": true,
    "theme_tab_selected_underlined": true
}
````

>key - Bindings-User

````json
/**
 * 自定义的快捷键
 */
//我定义的是makrdown预览和代码缩进,还有对齐等号
[
    {"keys": ["alt+m"], "command": "markdown_preview", "args": { "target": "browser"}},
    {"keys": ["ctrl+alt+l"], "command": "reindent", "args": { "single_line": false}},
    { "keys": ["ctrl+alt+x"], "command": "alignment" }
]
````

>我的常用插件

````json
{
    "installed_packages": [
        "A File Icon",                                    
        "Alignment",    //对齐等号，快捷键上面设置的ctrl+alt+x
        "Bootstrap 3 Autocomplete",  
        "Bootstrap 3 Snippets", 
        "Boxy Theme",       //我默认使用的主题
        "Boxy Theme Addon - Mono File Icons",       //文件样式，必选
        "CodeIgniter Snippets",     //CI框架代码提示
        "Color Highlighter",        //颜色高亮,主要用于高亮前端代码中的颜色
        "ColorPicker",              //取色器
        "ConvertToUTF8",            //写php必备
        "DocBlockr",                //必备，注释的自动完成
        "Emmet",                    //前端开发必备，具体使用语法很多，还没有掌握
        "GitSavvy",                 //git和github同步插件吧，还没有学会使用
        "HTML-CSS-JS Prettify",     //美化前端代码
        "JavaScript Completions",   //JS自动完成插件，写js代码必备吧
        "JsFormat",                 //
        "Markdown Extended",        //对markdown的语法拓展
        "Markdown Preview",         //主要用来网页预览,必备，设置CSS和JS还是觉得太麻烦
        "MarkdownEditing",          //有3个主题，和第一个配合使用
        "Material Theme",           //没事可以切换试试，也很好看，但是感觉不如boxy主题
        "Package Control",          //万物之始
        "PackageSync",              //同步，目前测试同步到另一电脑时要报错
        "ProjectManager",           //项目管理，必备
        "SideBarEnhancements",      //必备，边栏拓展
        "TrailingSpaces",           
        "Yii2 Auto-complete", 
        "Yii2 Snippets"
        "AutoFileName"         //自动补全文件名
    ]
}
````

