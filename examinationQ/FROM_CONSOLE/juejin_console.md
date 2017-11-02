# 掘金控制台前端题目
> 看到掘金网上一篇文章讲立即执行函数的其他写法，立即打开console测试了一下，然后看到了掘金的前端题目。  

**题目** :    
-----------
This machine is a server. DO NOT POWER IT DOWN!!
-----------

我们在寻找一位前端工程师：[https://xitu.io/jobs](https://xitu.io/jobs)
```javascript
class JuejinFrontendEnginnerSpecification implements Specification {
  isSatisfiedBy (person) {
    return person.isInteresting() && person.canWriteBUG()
  }
}

class JuejinFrontendEnginner extends FrontendEngineer {
  constructor (person) {
    super(person)
    this.thingList = [
      'ES6+',
      'Node.js v8+',
      'Vue.js v2.4+',
      'SSR',
      'Chrome (Extension|Headless)',
      'Weixin',
      'Docker',
      ...,
      'rm -rf /',
      'escape'
    ]
  }
  doSomeInterestingThings () {
    this.thingList.forEach(this.tryToPlay.bind(this))
  }
}

const juejinFrontendEnginnerSpecification = new JuejinFrontendEnginnerSpecification()

if (juejinFrontendEnginnerSpecification.isSatisfiedBy(you)) {
  new JuejinFrontendEnginner(you).doSomeInterestingThings()
}
```
以上代码可以如何改进？发送答案及简历到前端组邮箱：webuster@xitu.io（注明「FROM_JUEJIN_CONSOLE」）

**思考**    
Js中 prototype 我厘清过一遍逻辑，在没有实际应用的情况下。这不是我的强项。    
出了原型，主要的代码量就在中间一个array了。  
.bind(this) 的操作，只在react中常见过，但我react只是个入门，不知道最新的react有没有新的bind(this)姿势。  
等灯等灯，thingList中，如果按照现在代码逻辑，前边的事情还没做，就直接 [`rm -rf /`, `escape`] 了，
招了个写bug的程序员，然后代码给它删了，让它跑路，不符合常理。至少，前面的 `SSR` ,`Chrome Extension` 要先做了。

我想到的改进点：
 1. 语义
 2. 事件时间顺序


**答复**  

1. 语义
  Specification有isSatisfiedBy，体现以人为本的话，创建个 new Candidate(me),发起examination。
  上面这条，我自己觉得有点搞笑。
  person.isInteresting() ? person自己给自己评价一个有趣属性，显然容易有偏差，比如，我自以为思路很灵活清奇，而他人看来非常混乱。 应该在Examination中，isInteresting(candidate.examination)
  person.canWriteBUG() 招会写bug的?
  这一整个li都是用来搞笑的。

2. 事件时间顺序  

```javascript  
import * as Promise from 'bluebird'
var shuffle = require('lodash.shuffle')

...
    doSomeInterestingThings () {
      this.thingList
          .filter(thing => !/^rm\s.*?/.exec(thing)) // 过滤删库 
          .shuffle()  // 做一件有意思的事情，一般不按部就班来吧
          .reduce(async (_,thing)=>
              (await Promise.promiseify(this.tryToPlay).bind(this)(thing))
              ,()=>{}
            )
    }
...

```  
以上。  

没get到优化点不发简历了。  


**TODO**  
还想改成RxJs,添加中间件，统计doSome..行为分布。  
还想加上tensorFlow 根据行为分布，模型分类。  
可惜，还都没学会。

  
  