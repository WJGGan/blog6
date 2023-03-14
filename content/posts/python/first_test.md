
---
weight: 2

title: "Python实验（一）"
date: 2023-03-14T15:59:44+08:00
draft: false
authors: ["WJG"]
description: "大三下Python课第一次实验"

tags: ["Python", "编程","ChatGPT"]
categories: ["学习"]

lightgallery: true
---

大三下Python课第一次实验，并在实验结束后使用ChatGPT来编写代码
<!--more-->

## （一）狐狸游戏

{{<admonition question>}}

模拟抓狐狸小游戏。假设一共有一排 5 个洞口，狐狸最开始的 时候在其中一个洞口，然后玩家随机打开一个洞口，如果里面有狐狸就抓到 了，如果洞口里没有狐狸就第二天再来抓，但是第二天狐狸会在玩家来抓之前 跳到隔壁洞口里。如果在规定的次数内抓到了狐狸就提前结束游戏并提示成 功；如果规定的次数用完还没有抓到狐狸，就结束游戏并提示失败。

{{</admonition>}}

### 自己书写的代码如下


```python
import random
site=random.randrange(1,6)##随机生成狐狸所在的初始洞口
hole=[1,2,3,4,5]
movefoward=[-1,1]##狐狸可以移动的两个方向
print('Game begin!You have 3 chance')
try:
   choice=int(input('Input your choice:'))##确定第一次选定的洞口
except:
    print('Error!Please input a number!')
for i in range(0,3):
    if choice!=site:
        print(i+1,'game you choose',choice,",it's wrong")
        move=random.choice(movefoward)##确定狐狸移动的方向
        if site!=1 and site!=5:
            site+=move 
        elif site==1:
            site+=1
        elif site==5:
            site-=1
    elif choice==site:
        print('Congraduation!You succeed!')
        break
    try:
       if i!=2:
        choice=int(input('input your choice:'))##确定新选定的洞口
    except:
        print('Error!Please input a number!')
if i==2:
   print('you failed...')

```

### 尝试用ChatGPT来编写代码
ChatGPT给出了如下代码：
```python
import random

# 初始化狐狸的位置
fox_hole = random.randint(1, 5)

# 初始化抓狐狸的次数
num_attempts = 0

# 开始游戏
while num_attempts < 3:
    # 玩家随机选择一个洞口
    player_choice = int(input("请选择一个洞口（1-5）："))

    # 如果选择了狐狸所在的洞口，游戏结束，玩家胜利
    if player_choice == fox_hole:
        print("恭喜你，你抓到了狐狸！")
        break

    # 否则，狐狸会跳到相邻的洞口里
    else:
        print("很遗憾，这个洞口里没有狐狸。")
        fox_hole = fox_hole + random.choice([-1, 1])
        fox_hole = max(1, min(5, fox_hole))  # 确保狐狸不会跳出洞口的范围

    # 增加抓狐狸的次数
    num_attempts = num_attempts + 1

# 如果已经抓了3次还没有抓到狐狸，游戏结束，玩家失败
if num_attempts == 3:
    print("很遗憾，你没有在规定的次数内抓到狐狸。")


```

### 并给出如下解释

{{<admonition info"解释">}}

在这个程序中，我们首先使用 `random` 模块随机选择一个洞口作为狐狸的初始位置。然后，在一个 `while` 循环中，我们让玩家随机选择一个洞口，然后判断是否抓到了狐狸。如果抓到了狐狸，游戏结束，玩家胜利。否则，狐狸会跳到相邻的洞口里，我们再次进入循环，直到玩家抓到狐狸或者抓狐狸的次数达到了 3 次。

注意，我们使用了 `random.choice([-1, 1])` 来随机选择狐狸跳到相邻洞口的方向，这里 `-1` 表示向左跳，`1` 表示向右跳。同时，我们使用 `max(1, min(5, fox_hole))` 来确保狐狸不会跳出洞口的范围（即洞口的编号必须在 1 到 5 之间）。

{{</admonition>}}

不得不感叹AI惊人的处理能力，只需要几秒钟便可以理解出人类的自然语言并将其抽象化，同时还对代码的书写思路进行了讲解，代码十分简介易懂，我真的怀疑它可能找到一模一样的题了

## (二)搜索红色加粗文本

{{< admonition question >}}

读取 Word 文件中的所有段落文本，然后输出其中所有红色的文本和加粗的文本以及同时具有这两种属性的文本。具体步骤如下：

（1）在命令提示符环境使用 pip install python-docx 命令安装扩展库python-docx。

（2）创建测试用的 Word 文档 test.docx，写入测试内容，并根据需要设置红色文本和加粗文本。

（3）编写程序查找并输出 Word 文档 test.docx 中的红色文本和加粗文本。

{{< /admonition >}}



```python
from docx import Document
from docx.shared import Length, Pt, RGBColor

document = Document('testnew.docx')##导入文档
##依次循环段落
for p in document.paragraphs:
    ##逐字遍历
    for i in p.runs:
        if i.bold or i.font.color.rgb==RGBColor(255,0,0):
            print(i.text)

```



### 使用ChatGPT来编写代码

我仅给出了问题的前半部分，未告诉其具体的步骤，自动识别出需要使用python-docx包，并给出如下代码

```python
import docx

# 打开 Word 文档
doc = docx.Document('example.docx')

# 遍历文档中的所有段落
for paragraph in doc.paragraphs:
    # 遍历段落中的所有文本块
    for run in paragraph.runs:
        # 如果文本块是红色的，输出它的文本
        if run.font.color.rgb == docx.shared.RGBColor(255, 0, 0):
            print("红色文本：", run.text)
        # 如果文本块是加粗的，输出它的文本
        if run.bold:
            print("加粗文本：", run.text)
        # 如果文本块既是红色的又是加粗的，输出它的文本
        if run.font.color.rgb == docx.shared.RGBColor(255, 0, 0) and run.bold:
            print("红色加粗文本：", run.text)

```

{{<admonition info"解释">}}

在这个程序中，我们首先使用 `docx` 模块打开一个 Word 文档。然后，我们遍历文档中的所有段落，对于每个段落，我们遍历它中间的所有文本块。对于每个文本块，我们判断它是否是红色的、加粗的，或者同时具有这两种属性。如果是其中一种属性，我们输出它的文本。如果同时具有这两种属性，我们也输出它的文本。注意，在 `docx` 模块中，文本块被称为“runs”。

{{</admonition>}}



### ChatGPT nb!!!!!!
