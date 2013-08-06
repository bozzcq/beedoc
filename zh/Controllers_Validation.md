# 表单验证

表单验证是用于数据验证和错误收集的模块。

## 安装及测试

安装：

	go get github.com/astaxie/beego/validation

测试：

	go test github.com/astaxie/beego/validation

## 示例

直接使用示例：

	import (
		"github.com/astaxie/beego/validation"
		"log"
	)

	type User struct {
		Name string
		Age int
	}

	func main() {
		u := User{"man", 40}
		valid := validation.Validation{}
		valid.Required(u.Name, "name")
		valid.MaxSize(u.Name, 15, "nameMax")
		valid.Range(u.Age, 0, 140, "age")
		if valid.HasErrors {
			// 如果有错误信息，证明验证没通过
			// 打印错误信息
			for _, err := range valid.Errors {
				log.Println(err.Key, err.Message)
			}
		}
		// or use like this
		if v := valid.Max(u.Age, 140); !v.Ok {
			log.Println(v.Error.Key, v.Error.Message)
		}
		// 定制错误信息
		minAge := 18
		valid.Min(u.Age, minAge).Message("少儿不宜！")
		// 错误信息格式化
		valid.Min(u.Age, minAge).Message("%d不禁", minAge)
	}


通过 struct tag 使用示例：

	import (
		"github.com/astaxie/beego/validation"
	)

	// 验证函数写在 "valid" tag 的标签里
	// 各个函数之间用分号 ";" 分隔，分号后面可以有空格
	// 参数用括号 "()" 括起来，多个参数之间用逗号 "," 分开，逗号后面可以有空格
	// 正则函数(Match)的匹配模式用两斜杠 "/" 括起来
	// 各个函数的key值为字段名
	type user struct {
		Id   int
		Name string `valid:"Required;Match(/^(test)?\\w*@;com$/)"` // 对Name段要Required和Match的验证
		Age  int    `valid:"Required;Range(1, 140)"` // 对Age字段进行Required和Range的验证
	}

	func main() {
		valid := Validation{}
		u := user{Name: "test", Age: 40}
		b, err := valid.Valid(u)
		if err != nil {
			// handle error
		}
		if !b {
			// validation does not pass
			// blabla...
		}
	}

Struct Tag 可用的验证函数:

* `Required` 不为空，即各个类型要求不为其零值
* `Min(min int)` 最小值，有效类型:`int`，其他类型都将不能通过验证
* `Max(max int)` 最大值，有效类型:`int`，其他类型都将不能通过验证
* `Range(min, max int)` 数值的范围，有效类型:`int`，他类型都将不能通过验证
* `MinSize(min int)` 最小长度，有效类型:`string slice`，其他类型都将不能通过验证
* `MaxSize(max int)` 最大长度，有效类型:`string slice`，其他类型都将不能通过验证
* `Length(length int)` 指定长度，有效类型:`string slice`，其他类型都将不能通过验证
* `Alpha` alpha字符，有效类型:`string`，其他类型都将不能通过验证
* `Numeric` 数字，有效类型:`string`，其他类型都将不能通过验证
* `AlphaNumeric` alpha字符或数字，有效类型:`string`，其他类型都将不能通过验证
* `Match(pattern string)` 正则匹配，有效类型:`string`，其他类型都将被转成字符串再匹配(fmt.Sprintf("%v", obj).Match)
* `AlphaDash` alpha字符或数字或横杠`-_`，有效类型:`string`，其他类型都将不能通过验证
* `Email` 邮箱格式，有效类型:`string`，其他类型都将不能通过验证
* `IP`  IP格式，目前只支持IPV4格式验证，有效类型:`string`，其他类型都将不能通过验证
* `Base64` base64编码，有效类型:`string`，其他类型都将不能通过验证
* `Mobile` 手机号，有效类型:`string`，其他类型都将不能通过验证
* `Tel` 固定电话号，有效类型:`string`，其他类型都将不能通过验证
* `Phone` 手机号或固定电话号，有效类型:`string`，其他类型都将不能通过验证
* `ZipCode` 邮政编码，有效类型:`string`，其他类型都将不能通过验证

### API 文档

请移步 [Go Walker](http://gowalker.org/github.com/astaxie/beego/validation)。