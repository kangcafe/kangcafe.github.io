# kangkk

```
▄    ▄  ▄▄▄▄▄▄▄▄▄▄▄  ▄▄        ▄  ▄▄▄▄▄▄▄▄▄▄▄  ▄    ▄  ▄    ▄  
▐░▌  ▐░▌▐░░░░░░░░░░░▌▐░░▌      ▐░▌▐░░░░░░░░░░░▌▐░▌  ▐░▌▐░▌  ▐░▌
▐░▌ ▐░▌ ▐░█▀▀▀▀▀▀▀█░▌▐░▌░▌     ▐░▌▐░█▀▀▀▀▀▀▀▀▀ ▐░▌ ▐░▌ ▐░▌ ▐░▌
▐░▌▐░▌  ▐░▌       ▐░▌▐░▌▐░▌    ▐░▌▐░▌          ▐░▌▐░▌  ▐░▌▐░▌  
▐░▌░▌   ▐░█▄▄▄▄▄▄▄█░▌▐░▌ ▐░▌   ▐░▌▐░▌ ▄▄▄▄▄▄▄▄ ▐░▌░▌   ▐░▌░▌   
▐░░▌    ▐░░░░░░░░░░░▌▐░▌  ▐░▌  ▐░▌▐░▌▐░░░░░░░░▌▐░░▌    ▐░░▌    
▐░▌░▌   ▐░█▀▀▀▀▀▀▀█░▌▐░▌   ▐░▌ ▐░▌▐░▌ ▀▀▀▀▀▀█░▌▐░▌░▌   ▐░▌░▌   
▐░▌▐░▌  ▐░▌       ▐░▌▐░▌    ▐░▌▐░▌▐░▌       ▐░▌▐░▌▐░▌  ▐░▌▐░▌  
▐░▌ ▐░▌ ▐░▌       ▐░▌▐░▌     ▐░▐░▌▐░█▄▄▄▄▄▄▄█░▌▐░▌ ▐░▌ ▐░▌ ▐░▌
▐░▌  ▐░▌▐░▌       ▐░▌▐░▌      ▐░░▌▐░░░░░░░░░░░▌▐░▌  ▐░▌▐░▌  ▐░▌
▀    ▀  ▀         ▀  ▀        ▀▀  ▀▀▀▀▀▀▀▀▀▀▀  ▀    ▀  ▀    ▀  
```

> “Never memorize something that you can look up.” - Albert Einstein（绝不要去死记硬背那些可随手能查阅到的东西 爱因斯坦）

## 文件名称

```html
n.n.浮点数.0938b3d6-46da-4295-9517-9e38f8fe2bb7.html
```

|   参数   |  描述  |
| -------- | -------- |
| 第一个参数 | 是否编译 |
| 第二个参数 | 是否发布 |
| 第三个参数 |  uuid   |
| 第四个参数 | 文件后缀 |

```
文件
  |———是否编译———n———不编译
        |             |
        y             |
        |             |
     编译文件          |
        |             |
        |——————————是否发布文件———n———不发布
                      |
                      y
                      |
                  发布文件
```

测试环境只执行编译参数，发布环境执行编译、发布参数，默认缺省（y.n---编译不发布）。

## 环境

测试环境、发布环境、清空环境。
