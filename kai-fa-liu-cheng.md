```flow

st=>start: 详读需求, 标准疑问点
op1=>operation: 评审需求, 提出疑问
con1=>condition: 是否还有疑问
op2=>operation: 整理需求,设计公共组件,提炼公共方法
op3=>operation: 开发前端代码, 实现需求
op4=>operation: 发开发环境,联调
con2=>condition:  是否有bug
op5=>operation: 修改问题
op6=>operation: 检测代码, 优化代码
op7=>operation: 送测试
con3=>condition: 是否有bug
op8=>operation: 修改问题
e=>end: 开发完成


st->op1->con1
con1(yes)->op1
con1(no)->op2
op2->op3->op4->con2
con2(yes)->op5->op6
con2(no)->op6
op6->op7->con3
con3(yes)->op8->e
con3(no)->e

```
