# 地图

> 后端在游戏界面初始化时就传给前端
>
> TODO：
>
> 1.增加和用户相关的信息——历史得分，历史代码等

```json
{
    "id": "关卡编号",
    "name": "关卡名字", // 勇闯xxxx之类的
    "length": "地图长度",
    "width": "地图长度",
    "start": {
        "x": "x坐标",
        "y": "y坐标"
    },
    "end": {
        "x": "x坐标",
        "y": "y坐标"
    },
    "treasure": { // 由于先只实现前三关，因此只需要两个参数，后续可以继续添加
        "x": "x坐标",
        "y": "y坐标"
    },
    "character": {
        "type": "人物形象", //人物形象，1为宇航员，2为遥控机器人，3为外星人
        "position": {
            "x": "x坐标",
            "y": "y坐标"
        },
        "state": "人物朝向" // u,d,l,r
    }
}
```

# 代码

#### 前端传给后端的代码分析函数

```json
{
    "id": "关卡编号", //只有该项为必须项，其他项都为可选可重复的
    "type": "程序执行方式", // 可选值为continue和onestep，onestep的话只返回一条动作
    "code": {
        "goStraight": "具体步数",
        "turnLeft": "NULL",
        "turnRight": "NULL",
        "inspect": "NULL", // 探索面前一格的信息，空格返回1，障碍2，宝藏3，边沿4
        "if": {
            "condition": { // 必须包含一个inspect函数
                "expression": "具体的关系符号", // 支持==,!=
                "val": "条件对应的限制值" // 取值范围与inspect函数返回值一致
            },
            "code": { // 可以嵌套其他代码
                ...
            },
            "else": { //可以为空，嵌套其他代码

            }
        },
        "while": {
            "condition": { // 必须嵌套一个inspect函数
                "expression": "具体的关系符号", // 支持==，!=
                "val": "条件对应的限制值" // 取值范围与inspect函数返回值一致
            },
            "code": { // 嵌套其他代码

            }
        },
        "open": "NULL" //打开面前一格的宝箱,成功返回1,失败返回0
    }
    
}
```

#### 后端传给前端的动作序列

```json
{
    "turnLeft": "NULL",
    "turnRight": "NULL",
    "goUp": "走的步数",
    "goDown": "走的步数",
    "goLeft": "走的步数",
    "goRight": "走的步数",
    "collectSuccess": "NULL", // 前端根据宝箱开启的成功与否展示不同的特效
    "collectFail": "失败的原因",
    "endMissionSuccess": "得分", //当前端读取到这个，意味着指令序列结束
    "endMissionFail": "失败的原因" //当前端读取到这个，意味着指令序列结束
}
```