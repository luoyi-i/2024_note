
_____ 字体文件放文件系统

https://yuanze.wang/posts/lvgl-chinese-font-in-bin/

把这个做一遍
vt42 就结束了
---------------------
目的， 使用 .c 格式的字体，并将字体数据放入 Flash 中， 避免反复下载
生成字体
生成字体需要使用lvgl官方推出的命令行工具lv_font_conv，该工具使用javascript编写
# 安装Node.js
为了使用这个工具，首先要在本地安装Node.js解释器。您可以前往官方网站进行下载，选择最新的LTS版即可。

```
node.exe 是一个命令行窗口, 可以运行 javascript 直接看到结果


node.exe 是一个命令行窗口。它是运行在服务端的 javascript 环境，主要用于执行服务器脚本。
可以用来开发和运行基于 javascript 的服务器应用。
输入 node 命令后启动 node.js 的交互环境，输入 js 代码可以立刻看到结果。
```
ssssss

```
当你打开 node.exe 的时候，会打开一个命令行窗口。
这是因为Node.js是运行在服务端的JavaScript环境，它主要用于执行服务器端脚本，而这些操作通常在命令行环境中进行。
Node.js不像传统的桌面应用程序有图形用户界面 (GUI)，它更像一种工具，被用来开发和运行基于 JavaScript 的服务器应用。
在Node.js的命令行中，你可以输入node命令来启动Node.js的交互环境，然后在这个环境中输入JavaScript代码并立刻看到结果。你也可以使用node 文件名.js的方式来执行一个包含JavaScript代码的.js文件。
如果你需要运行针对Web的JavaScript应用，通常会用Node.js配合一些框架，比如Express.js。使用这些框架，你可以方便快捷地构建起一个功能完善的Web服务器
```

_____ 安装了 node 和 npm
https://www.liaoxuefeng.com/wiki/1022910821149312/1023025597810528

node.js 这个下载页面才是对的
https://nodejs.org/en
下载了 node-v20.11.0-x64.msi
可以安装了
我选了工具也安装， 后面自动安装了很多东西， 删了，重装，不选安装 tool 
![Alt text](image.png)
*之前下载的是对的，是可以用。 有错误提示看错误提示，不能想当然*

npm 是 node.js 的包管理工具。 大家都会把自己开发的模块打包到 npm 官网上。
如果要使用，直接通过 npm 安装就可以直接用。
另外， npm 会自动帮我们下载依赖的包，不用我们自己去管理。

npm 在我们安装 node.js 的时候就一起安装好了
_____ 使用 node
一开始下载的 node.exe 是一个 javascript解释器， 输入 js 命令可以直接得到结果。
node 安装好后可以运行 js 文件，也可以从命令行进入 node.exe 的界面。 
和 python 的运行和使用有点像

vscode 可以调试 js 代码
直接打开文件夹，打开左侧调试按键


_____ 模块
hello.js文件就是一个模块，模块的名字就是文件名（去掉.js后缀）

https://www.liaoxuefeng.com/wiki/1022910821149312/1023027697415616


2024.01.31

#### todo

/*1: Show CPU usage and FPS count*/
#define LV_USE_PERF_MONITOR 1
#if LV_USE_PERF_MONITOR
    #define LV_USE_PERF_MONITOR_POS LV_ALIGN_BOTTOM_RIGHT
#endif

/*1: Show the used memory and the memory fragmentation
 * Requires LV_MEM_CUSTOM = 0*/
#define LV_USE_MEM_MONITOR 0


* 继续昨天的 将LVGL的中文字体编译为文件写入Flash中并读取
https://yuanze.wang/posts/lvgl-chinese-font-in-bin/

# 安装 lv_font_conv

![Alt text](image.png)

npm install lv_font_conv -g

added 1 package in 3s

-- >安装成功
# 使用lv_font_conv生成字体

先按照他的做
我想用这个
HarmonyOS_Sans_Condensed_Regular.ttf

lv_font_conv --bpp 4 --size 20 -o font_harmony_sans_20.c --format lvgl --no-kerning --no-prefilter --font HarmonyOS_Sans_SC_Regular.ttf --range 0x20-0xBF --range 0x3000-0x3011 --range 0x4E00-0x9FAF --range 0xFF00-0xFF64

lv_font_conv --bpp 4 --size 20 -o font_harmony_sans_20.c --format lvgl --no-kerning --no-prefilter --font HarmonyOS_Sans_SC_Regular.ttf --range 0x20-0xBF --range 0x3000-0x3011 --range 0x4E00-0x9FAF --range 0xFF00-0xFF64

我只要前 96 个字符
lv_font_conv --bpp 4 --size 20 -o font_harmony_sans_20.c --format lvgl --no-kerning --no-prefilter --font HarmonyOS_Sans_Condensed_Regular.ttf --range 0x20-0x7F

生成的程序放入项目文件夹
D:\work\esp32\esp32_VT42\esp32_gui_spiffs\.pio\lihbdeps\esp32-c3-devkitm-1\lvgl\src\font


编译新字体
extern const lv_font_t font_harmony_sans_20;
#define LV_FONT_DEFAULT &font_harmony_sans_20



static void PAGE_initStyleLabel(void) {
    lv_style_init(&g_styleLabel);
    lv_style_set_text_color(&g_styleLabel, lv_color_white());
    lv_style_set_text_font(&g_styleLabel, &FontSimpleArray); //xxxxx
}
lv_style_set_text_font

2024.02.01
aim

把 font 放 flash

重新用命令行生成字体文件
去掉 kerning
lv_font_conv --bpp 4 --size 20 -o font_harmony_sans_20.c --format lvgl --no-prefilter --font HarmonyOS_Sans_Condensed_Regular.ttf --range 0x20-0x7F


lv_font_conv --bpp 4 --size 20 -o font_harmony_sans_20.c --format lvgl --font HarmonyOS_Sans_Condensed_Regular.ttf --range 0x20-0x7F


_____ guilite font
sumup: guilite font 利用了点阵式
所有字体高度是固定的
宽度是独立的
//FONT
typedef struct struct_lattice {
    unsigned int utf8_code;
    unsigned char width;
    const unsigned char *pixel_buffer;
} LATTICE;

typedef struct struct_lattice_font_info {
    unsigned char height;
    unsigned int count;
    LATTICE *lattice_array;
} LATTICE_FONT_INFO;

extern const LATTICE_FONT_INFO Consolas_32B;
const LATTICE_FONT_INFO Consolas_32B ={
    32,    // height
    84,    // count, number of char
    lattice_array
};

static  LATTICE lattice_array[] = {
        {32, 15, _32},
        {33, 15, _33},
        ----
}

static const unsigned char _32[] = {
0, 255, 0, 225, };
static const unsigned char _33[] = {
0, 111, 170, 1, 255, 2, 212, 1, 0, 11, 170, 1, 255, 2, 212, 1, 0, 11, 170, 1, 255, 2, 212, 1, 0, 11, 127, 1, 255, 2, 212, 1, 0, 11, 127, 1, 255, 2, 170, 1, 0, 11, 127, 1, 255, 2, 170, 1, 0, 11, 127, 1, 255, 2, 170, 1, 0, 11, 127, 1, 255, 2, 170, 1, 0, 11, 85, 1, 255, 2, 127, 1, 0, 11, 85, 1, 255, 2, 127, 1, 0, 11, 85, 1, 255, 2, 127, 1, 0, 11, 85, 1, 255, 2, 127, 1, 0, 41, 127, 1, 255, 2, 85, 1, 0, 10, 43, 1, 255, 4, 0, 10, 43, 1, 255, 4, 0, 11, 127, 1, 255, 2, 85, 1, 0, 110, };

_____ lvgl 字体不是点阵的，应该是矢量图


------ lvgl font
/**
 * Descriptor of a style (a collection of properties and values).
 */
typedef struct {

#if LV_USE_ASSERT_STYLE
    uint32_t sentinel;
#endif

    /*If there is only one property store it directly.
     *For more properties allocate an array*/
    union {
        lv_style_value_t value1;
        uint8_t * values_and_props;
        const lv_style_const_prop_t * const_props;
    } v_p;

    uint16_t prop1;
    uint8_t has_group;
    uint8_t prop_cnt;
} lv_style_t;

/**
 * Descriptor of a constant style property.
 */
typedef struct {
    lv_style_prop_t prop;
    lv_style_value_t value;
} lv_style_const_prop_t;

/**
 * A common type to handle all the property types in the same way.
 */
typedef union {
    int32_t num;         /**< Number integer number (opacity, enums, booleans or "normal" numbers)*/
    const void * ptr;    /**< Constant pointers  (font, cone text, etc)*/
    lv_color_t color;    /**< Colors*/
} lv_style_value_t;





