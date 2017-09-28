##Unix带给我们的编程思想

> 一些闪光的思想

* 过度依赖任何一种技术或者商业模式都是错误的——相反，保持软件及其设计传统的的灵活性才是长存之道。
* Everything (including hardware) is a file
	- 万物皆文件
* Configuration data stored in text
	- 以文本形式储存配置数据
* Small, single-purpose program
	- 程序尽量朝向小而单一的目标设计
* Avoid captive user interfaces
	- 尽量避免令人困惑的用户接口
* Ability to chain program together to perform complex tasks
	- 将几个程序连结起来，处理大而复杂的工作

> The art of Unix Programing

- Rule of Modularity: Write simple parts connected by clean interfaces.
	+ 模块原则： 使用简洁的接口拼接多个部件
	+ 每个程序只做一件事，不要试图在单个程序中完成多个任务
- Rule of Clarity: Clarity is better than cleverness.
	+ 清晰原则： 代码要写得尽量清晰，避免晦涩难懂。
- Rule of Composition: Design programs to be connected to other programs.
	+ 组合原则： 设计是考虑组合，最小立异性为中心
- Rule of Separation: Separate policy from mechanism; separate interfaces from engines.
	+ 分离原则： 策略和机制分离，接口和引擎分离
- Rule of Simplicity: Design for simplicity; add complexity only where you must.
	+ 简单原则： 设计要简洁，尽可能降低复杂度
- Rule of Parsimony: Write a big program only when it is clear by demonstration that nothing else will do.
	+ 吝啬原则： 避免过度设计，仅仅所有事情都完美的时候才加功能
- Rule of Transparency: Design for visibility to make inspection and debugging easier.
	+ 透明原则： 可见性设计，方便审查和调试
- Rule of Robustness: Robustness is the child of transparency and simplicity.
	+ 健壮原则： 源于透明与简洁
- Rule of Representation: Fold knowledge into data so program logic can be stupid and robust.
	+ 表示原则： 把知识叠入数据以求逻辑质朴而健壮
- Rule of Least Surprise: In interface design, always do the least surprising thing.
	+ 通俗原则： 接口设计避免标新立异（标准化，减少学习成本）
- Rule of Silence: When a program has nothing surprising to say, it should say nothing.
	+ 缄默原则： 没有意外就什么也别说
- Rule of Repair: When you must fail, fail noisily and as soon as possible.
	+ 修复原则：当你需要报错的时候，就尽快并且明显的报错
- Rule of Economy: Programmer time is expensive; conserve it in preference to machine time.
	+ 经济原则： 程序员的时候很宝贵，机器能做的就别自己做。
- Rule of Generation: Avoid hand-hacking; write programs to write programs when you can.
	+ 生成原则： 避免手动hack，让程序去做重复的事情。
- Rule of Optimization: Prototype before polishing. Get it working before you optimize it.
	+ 优化原则： 雕琢前先要有原型，优化之前先让其工作起来
	+ 在功能实现之前，不要考虑对它优化。最重要的是让一切先能够运行，其次才是效率。
- Rule of Diversity: Distrust all claims for “one true way”.
	+ 多样原则： 绝不相信任何唯一断言
- Rule of Extensibility: Design for the future, because it will be here sooner than you think.
	+ 扩展原则： 为未来设计，因为他来的比你想象得更快

> X Window -> 《The UNIX Philosophy》

- Small is beautiful.
	+ 小就是美
- Make each program do one thing well.
	+ 让程序只做好一件事。
- Build a prototype as soon as possible.
	+ 尽可能早地建立原型。
- Choose portability over efficiency.
	+ 可移植性比效率更重要。
- Store data in flat text files.
	+ 数据应该保存为文本文件。
- Use software leverage to your advantage.
	+ 尽可能地榨取软件的全部价值。
- Use shell scripts to increase leverage and portability.
	+ 使用shell脚本来提高效率和可移植性。
- Avoid captive user interfaces.
	+ 避免使用可定制性低下的用户界面。
- Make every program a filter.
	+ 所有程序都是数据的过滤器。