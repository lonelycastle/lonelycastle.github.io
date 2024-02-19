markdown 编辑器

1、vscode
2、在线：https://markdown.lovejade.cn/


基本语法如下

1、标题

# 标题字号，几个 # 就是几号


2、链接

[链接标题](www.baidu.com)


3、粗体

**内容**


4、斜体

*内容*（***粗斜体***）


5、删除线

~~内容~~


6、高亮

`内容`（也可以是简单的代码）


7、列表

- 内容
* 内容
+ 内容


8、有序列表

1. 内容


9、引用

> 内容
>>嵌套引用


10、分割线

---


11、待办事项

- [x] 内容


12、代码

```c
#include <stdio.h>
int main() {
    printf("hello world\n");
    return 0;
}
```


13、图片

![图片标题](https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png)  
如果要修改图片 size:
<img src="https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png" height = "50" alt="图片名称" align=center />


14、表格

可以使用冒号来定义表格的对齐方式，如下：
| 姓名   | 年龄 |     工作 |
| :----- | :--: | -------: |
| 小可爱 |  18  | 吃可爱多 |
| 小小勇敢 |  20  | 爬棵勇敢树 |
| 小小小机智 |  22  | 看一本机智书 |

[关于表格](https://itmyhome.com/markdown/article/extension/table.html)


15、latex 公式

$$
E=mc^2
$$


16、脚注

脚注与链接的区别如下所示：
```markdown
链接：[文字](链接)
脚注：[文字](脚注解释 "脚注名字")
```


17、段落内换行

行末加两个空格再回车，否则不会换行。


其他

还可以画流程图、甘特图、序列图、饼图