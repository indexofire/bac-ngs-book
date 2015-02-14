## Perl

#### 检查是否安装了 perl 模块

```
~$ perl -e 'use Bio::Seq'
```

如果安装了模块则没有打印输出，否则会提示找不到模块。

#### 用 CPAN 安装模块

```
~$ perl -MCPAN -e shell

install HTML::TokeParser::Simple
```

#### 用 cpan 脚本安装模块

```
~$ cpan HTML::TokeParser::Simple
```
S
