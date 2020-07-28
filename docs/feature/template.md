# 使用模板

有时候, 对非常非常非常长的消息链的排版和调整可能会让你感到厌烦,
所以我们提供了 模板(Template) 机制来帮助开发者生成规范的消息链.

## 安装
模板机制并不内置于 Graia Application for Mirai 中,
所以你需要通过你的包管理器获取:

``` bash
pip install graia-template
# 使用 poetry
poetry add graia-template
```

## 使用
当你顺利安装了库后, 你就可以导入并使用了:

``` python
from graia.template import Template
```

你可以通过这样的方式使用模板:

``` python
Template("Hello, $target!").render(
    target=At(member.id)
)
```

我们通过 `$` 在模板字符串内定义一个模板块, 并通过模板块定位, 生成出最后的消息链.

以下模板块是可以接受的
```
$target
$t1
$t2
$0
$01
$02
$83123132
```

以下模板块无效:
```
$1aff
$-232
$<234w>
$我不是模板块
$の
$😈
```

?> **提示** 你可以通过以下代码判断你的模板块是否有效

``` python
import regex
regex.match(r"^(?|(\$[a-zA-Z_][a-zA-Z0-9_]*)|(\$[0-9]*))$", "$我不是模板块")
```

获得 `Template` 实例后, 我们就可以通过 `Template.render` 方法生成消息链了:

!> **注意** 生成的消息链不一定是可以被安全发送的. 还记得吗?
你可以通过 `MessageChain.isSendable` 来了解消息链是否可以被安全发送.

``` python
Template(f"Hello, $target! 在你的 Github, 今天共收获了 {star_increase} 个 star, Congratulations!").render(
    target=At(member.id)
)
```

`Template.render` 只接受 `Union[InternalElement, ExternalElement]`,
并且我们使用了 `pydantic.validate_arguments` 对传入的参数进行检查,
所以你得小心点使用这个方法.