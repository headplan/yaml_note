# PHP扩展

```
# 转换为YAML
# mixed $data 数据
# int $encoding 编码 => YAML_ANY_ENCODING, YAML_UTF8_ENCODING, YAML_UTF16LE_ENCODING, YAML_UTF16BE_ENCODING.四个常量  
# int $linebreak 换行 => YAML_ANY_BREAK, YAML_CR_BREAK, YAML_LN_BREAK, YAML_CRLN_BREAK.
# array $callbacks 回调
string yaml_emit();
# 第一个参数是保存文件的地址,生成YAML文件到指定目录
# 其他参数一样
bool yaml_emit_file();

```



