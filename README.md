# hflag
golang隐藏进程命令参数，从flag修改而来。Hidden process command parameters for golang, modified from flag.
用法同flag，只增加HideArg函数，详见https://pkg.go.dev/flag 。
## 例：隐藏password
### 代码
```golang
package main

import (
    "time"
    "github.com/devils/hflag"
)

func main() {
    hflag.StringVar(&user, "u", "root", "account, default: root")
    hflag.StringVar(&password, "p", "", "password, default:  ")
    hflag.StringVar(&host, "h", "localhost", "hostname, default: localhost")
    hflag.IntVar(&port, "P", 3306, "port, default: 3306")
    hflag.Parse()
    // 必须在Parse后面执行隐藏命令，否则password的值将等于替换后的"******"
    hflag.HideArg("p")
    time.Sleep(1 * time.Minute)
}
```
### 执行命令
```bash
./main -u root -p 123456 -h localhost -P 3306
```
### ps -ef验证，将看到password替换为“******”
```bash
./main -u root -p ****** -h localhost -P 3306
```
