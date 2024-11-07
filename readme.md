# 项目文档生成

语法树分析：tree-sitter-c-sharp

LLM分析：transformers+llama-cpp-python+deepseek-coder

## 项目分析策略

逐级生成分析，使用不同模板。

粗描述生成（自上而下）：

1. 首先读取每个文件，针对每个文件生成粗略描述（限制输入文本长度以免避免模型超限）
2. 根据每个文件，生成文件命名空间（父文件夹）描述（大部分引用的为命名空间）
3. 之后针对每个文件中的类生成描述，利用文件引用将其他文件的粗描述引入作为信息
4. 针对每个文件中的方法生成描述，利用类的描述信息

精细描述生成（自下而上）：

1. 利用一个类中的其他方法的描述，以及类的描述，生成当前方法描述
2. 利用类中所有方法的精细描述，生成类的描述
3. 利用一个文件中所有类的描述，生成文件的描述
4. 结合引用文件描述，重新生成当前文件描述

## 格式转换和输出

markdown parser：

* https://github.com/miyuchina/mistletoe
  * 支持输出格式：HTML、LaTeX、Markdown
* https://marko-py.readthedocs.io/en/latest/

## todo

min宝倾情支援：

使用agent方法在项目层级进行分析，目前llm原生的content长的也就256k、分块基本就是应该是agent类型能handle的。

现有的agent方案这个问题肯定解决的很好：

* https://js.langchain.com/v0.1/docs/use_cases/extraction/how_to/handle_long_text/

* https://github.com/QwenLM/Qwen-Agent
* https://qwenlm.github.io/blog/qwen-agent-2405/