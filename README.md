# -
自动化测试基本概念
——————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
　自动化测试的几个准则：
　　并不是将测试用例代码化了，就可以称之为自动化测试了。这是现在很多公司宣称自己做AT的一个噱头。
　　AT的代码有很多的要求。
　　首先就是你的覆盖面要够广。个位数case的自动化完全没有意义。
　　第二就是你的case必须要能够复用：软件每天都在变，如果你的case要天天跟着软件变，那你的case是完全不合格的。
　　第三就是测试的规模要够大：要么时间长（case多或者是压力测试），要么测试产品多。这样才能体现出来自动化测试的优势。、
　　测试自动化的几个准则：
　　第一个就是要减少除工具研发部门外，其他所有测试部门的人力成本。这个是测试自动化追求的终极目标之一。、
　　第二个就是提高测试质量，不仅仅包括测试执行的质量，还包括测试的统计质量，数据回溯质量，等等等等。这些质量的提高可以帮助测试团队修正他们的测试方法，而不是每天将精力铺在无止境的数据收集和分析中。
　　第三个就是要抢出时间。某一项工作自动化后的时间，要么比人手做时间短，要么可以在非工作的16个小时中进行。通过让电脑OT的方法来解放工程师或者项目经理。
　　自动化的三大入手点：
　　自动化的三大入手点其实和三大准则是一样的。看哪个需求更加迫切：
　　1. 成本：自动化并不一定围绕测试执行，还可以包括测试的准备，log的提取，数据分析等等。将所有的与测试有关的工作逐一列出，然后找到重复的，可以被代码化的部分，评估现有工作成本和自动化成本，寻找到收益最大的工作块并顺序将之代码化。
　　2. 质量：和成本差不多，只是在评估的时候需要评估的是该工作块现有的质量状况和需求质量间的差异，寻找到差异最多的那个模块，并将所有质量差的模块逐一进行自动化。
　　3. 时间：和以上两点一样，都需要寻找到与测试有关的所有步骤和工作块，将其中关键路径上，动作最慢，耗时最大的部分进行自动化。

——————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
　
谈自动化测试框架思想与构建
2011-06-03 17:06 散步的SUN 51Testing软件测试博客 字号：T | T
 
自动化测试一般是指软件测试的自动化，软件测试就是在预设条件下运行系统或应用程序，评估运行结果，预先条件应包括正常条件和异常条件。本文介绍的是自动化测试框架思想与构建，一起来看。
AD：51CTO 网+ 第十二期沙龙：大话数据之美_如何用数据驱动用户体验
序言：也许到现在大家对所谓的“自动化测试框架”仍然觉得是一种神秘的东西，仍然觉得其与各位很远；其实不然，“自动化测试框架”从理念来说，并不复杂，但其之所以神秘，是因为其运用起来很是复杂，每个公司，每个部门其产品线，其运作流程都是不同的，所以就导致了在想运用“自动化测试框架”去完成自动化测试时产生了很多不定因素，导致了很多自动化测试项目的失败，让人对“自动化测试框架”开始敬而远之。
而自动化测试发展也有一段时间了，为什么到现在虽见其火热，但难见其规模，关键是大家对其的定位，很多公司以及很多人都知道做好自动化测试不简简单单的靠一个工具，而更需要一个框架，但其总是对“自动化测试框架”缺乏清晰的定位，很容易将其定位成了一个固定的框架，其实个人理解不然，自动化测试框架不是一个模式，而是一系列思想的集合，是将各种自动化测试框架思想集合应用去搭建成的一个分层组织。
一、简述自动化测试框架
也许很多人印象里的自动化测试框架就是一个能够进行自动化测试的程序似的。其实这不全面，真正的自动化测试框架可以不是一个程序，它仅仅是一种思想和方法的集合，说白了，就是一个架构，大家应该都知道操作系统其实也是一个架构吧，你可以把其理解成一个基础的自动化测试框架为一个简单的操作系统，它定义了几层架构，定义了各层互相通信的方式。通过这个架构我们才能在上面进行拓展我们的测试对象（核心体）、测试库（链接库）、测试用例集（各个windows进程）、测试用例（线程），而其之间的通过参数的传递进行通信（即相当于系统中的消息传递）。
二、自动化测试框架思想
接触过自动化测试的，一定不会对以下几种“自动化测试框架思想”陌生吧。
•	模块化思想
•	库思想
•	数据驱动思想
•	关键字驱动思想
很多人都将以上定义为“框架”，而我却觉得它们都只是代表了一种自动化测试的思想，不能以纯粹的框架定义。
首先，我们来看看自动化测试的一个发展，就能更加明白这些思想的真谛了。
a）第一代自动化测试，即自动化测试思想刚开始诞生时，依靠的是传统的“录制-回放”技术，这种技术与现在的工具的“录制-回放”思想不一样，其其实就是一个“模拟”的过程，即模拟你对PC的操作而形成的，其基于你对键盘的输入与对鼠标的操作，原理与按键精灵等类似，这种机制对环境的依赖性太强，对变化性太过于敏感，因此不可能发展成一种规模。
b）第二代自动化测试，即脚本化的自动化测试，利用脚本进行结构化的自动化测试，此可以应用于CLI与API的自动化测试，在其就开始集成了模块化与库思想。
c）第三代自动化测试，开始产生了各种自动化测试思想，包括数据驱动与关键字驱动思想，其伴随着对象化思想的产生，而且也造就了现在一系列的自动化测试软件，其实其中都集成了这些思想，从这时候开始，自动化就开始实现了一定的规模，开始运用在各个行业，并且发展趋势越来越快。
现在将一一根据自己的个人理解来介绍这些“自动化测试框架思想”：
1、所谓模块化思想，就是将一个测试用例中的几个不同的测试点拆分并且将其单个点的测试步骤进行了封装，形成了一个模块。
例如：一个测试用例要对一个登录程序进行测试，其中包括：用户名输入、密码输入、以及确定登录；
那么就可以将用户名输入、密码输入、确定登录、取消登录四个操作分别封装在四个不同的模块中。测试时，只需调用其模块即可。这样的话，当一个模块有变化，你只需单独维护那个模块即可，也可以根据模块的不同组合成不同的测试用例。
2、所谓测试库思想，就是模块化思想的升华，其为应用程序的测试创造了库文件（可以是APIs、DLLs等），这些库文件为一系列函数的集合。其与模块化思想不同的是，其拓展了接口思想，即可以通过接口去传递参数，而不是一个封死的模块，可以说是一个多了一个“门”的交互型模块。
例如：还是以上那个测试用例，只是将用户名输入、密码输入、确定登录、取消登录封装成一个库，这个库含有一个函数Login，这个函数Login接收两个参数“用户名、密码”，对输入不同的用户名和密码可以进行不同的测试用例。也可以另外一个函数Cancle。
3、所谓数据驱动思想，众说纷纭，很多人都觉仅仅依靠用EXCLE表进行不同数据的读取仅是一个高级的参数化，其实怎么理解并不重要，关键是其思想能够好的应用到你的框架中。而我的理解就是变量不变，数据驱动结果，不同的数据导致了不同的结果的产生。而对于数据的导入，可以通过很多方式，例如：EXCLE表、XML（用在WEB中）、数据库（DB）、CSV文件、TXT等都可以。
4、所谓关键字思想，这个思想，我曾经一直思考，它与面向对象的关系，与交互模块化思想的区别。后来个人理解，其实关键字驱动就是一种面向对象的思想，例如：QTP、RFT中，对象可以为一个数据或者一个关键字，对对象的抓取，可以将其测试对象封装为一个关键字（即可以将gui元素封装成了一个个关键字），这样可以对其关键对象进行各种操作了，不同的对象可以驱动不同的测试流向与结果。
简单的应用的方式可以用一个EXCEL表，里面包括“对象类型”“对象名称”“对象操作名称”“判断方式”“预期结果”。这样的话，可以通过导入不同的对象类型和名称、不同的对象操作来构建成了一个测试用例表了。
以上只是对这些思想的个人理解，做好自动化测试，不是说你掌握了一个框架，而是要掌握其自动化的思想，然后根据这些思想，结合你不同的测试环境和流程来构建你自己的自动化测试框架。
三、构建自动化测试框架的策略
1、永远记住，你的“自动化测试框架”是给测试人员用的，如果你真的想把自动化测试做成一个规模，那么你需要将测试工程师当做你的用户，你不能指望他们有耐心的去编写测试脚本或者指望他们能够像你一样对这些思想有良好的掌握。你要将他们当成什么都不懂的用户，因此你的框架必须是“一切简单化”的化身，简单的操作、简单的维护、简单的拓展。
2、做一个自动化测试框架主要是从分层上去考虑，而不是简简单单的应用一种思想，它是各种思想的集合体。
例如，做GUI自动化测试，简单的一般就将其分为三层，其框架如下图所示：
  
而其中，可以贯穿着自动化测试的各种思想，例如：对象层中有关键字的思想、可以将对象库标示在Excel表中进行管理，或者应用动态搜索的方式传递对象识别参数。tasks层中可以封装各种方法，形成一个大型的方法库，而每个方法中可以应用上数据驱动的思想。
3、真正的自动化测试框架是与流程上结合的，而不简简单单的靠技术实现，技术其实不是很复杂，关键就在于对其架构和流程的深刻把握，而这需要很长的一段时间，所以不要指望一口气能吃成胖子，只能一步一步按需求来，需求指导思想的应用。
四、自动化测试框架的发展趋势
个人认为，自动化测试从初始诞生到至今，已经经过了一段漫长的日子，而其仍处于上升期，特别是现在软件大爆炸、敏捷模式、云端的开始热门，测试难度和质量保证的难度开始越来越大，自动化测试的比重也会越来越大，而单存的自动化测试是无法实现规模化的，因此，自动化测试框架热门化的趋势化的必然的，那是，在各种框架思想的集合中，各种框架将散发出各自的璀璨，来帮助我们快速的完成各种测试。
以上仅仅是至今，个人对“自动化测试框架”的理解，也许在以后的日子，因为认识的加深而会有不同的火花蹦出，但至少觉得现在的框架对自己的项目能够进行应用，也许某一天，需求饱和时，那么，新一轮的远征探索就又要开始……
希望，我们大家在自动化测试的征程上能越走越远，也希望自动化测试能真正成为测试流程中“不可缺少”的一部分。共勉之。
版权声明：本文出自 散步的SUN 的51Testing软件测试博客：http://www.51testing.com/?382641

——————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
在做自动化测试之前你需要知道的 - 虫师
时间 2014-04-09 11:21:00  博客园_虫师
原文  http://www.cnblogs.com/fnng/p/3653793.html
主题 自动化测试
什么是自动化测？
做测试好几年了，真正学习和实践自动化测试一年，自我感觉这一个年中收获许多。一直想动笔写一篇文章分享自动化测试实践中的一些经验。终于决定花点时间来做这件事儿。
首先理清自动化测试的概念，广义上来讲，自动化包括一切通过工具（程序）的方式来代替或辅助手工测试的行为都可以看做自动化，包括性能测试工具（ loadrunner、 jmeter ） , 或自己所写的一段程序，用于生成 1 到 100 个测试数据。狭义上来讲，通工具记录或编写脚本的方式模拟手工测试的过程，通过回放或运行脚本来执行测试用例，从而代替人工对系统的功能进行验证。
当然，我们更普遍的认识把“自动化测试”看做“ 基于产品或项目 UI 层的自动化测试”。
分层的自动化测试
这个概念最近曝光度比较高，传统的自动化测试更关注的产品 UI 层的自动化测试，而分层的自动化测试倡导产品的不同阶段（层次）都需要自动化测试。
 
相信测试同学对上面的金字塔并不陌生，这不就是对产品开发不同阶段所对应的测试么！我们需要规范的来做单元测试同样需要相应的单元测试框架，如 java 的 Junit、 testNG ， C# 的 NUnit  ， python  的 unittest 、 pytest  等，几乎所有的主流语言，都会有其对应的单元测试框架。
集成、接口测试对于不少测试新手来说不太容易理解，单元测试关注代码的实现逻辑，例如一个 if  分支或一个 for 循环的实现；那么集成、接口测试关注的一是个函数、类（方法）所提供的接口是否可靠。例如，我定义一个 add() 函数用于计算两个参数的结果并返回，那么我需要调用 add() 并传参，并比较返回值是否两个参数相加。当然，接口测试也可以是 url 的形式进行传递。例如，我们通过 get 方式向服务器发送请求，那么我们发送的内容做为 URL 的一部分传递到服务器端。但比如  Web service 技术对外提供的一个公共接口，需要通过soapUI 等工具对其进行测试。 
UI 层的自动化测试，这个大家应该再熟悉不过了，大部分测试人员的大部分工作都是对 UI 层的功能进行测试。例如，我们不断重复的对一个表单提交，结果查询等功能进行测试，我们可以通过相应的自动化测试工具来模拟这些操作，从而解放重复的劳动。 UI 层的自动化测试工具非常多，比较主流的是 QTP ， Robot Framework 、 watir 、 selenium  等。
为什么要画成一个金字塔形，则不是长方形 或倒三角形呢？ 这是为了表示不同阶段所投入自动化测试的比例。如果一个产品从没有做单元测试与接口测试，只做 UI 层的自动化测试是不科学的，从而很难从本质上保证产品的质量。如果你妄图实现全面的UI 层的自动化测试，那更是一个劳民伤财的举动，投入了大量人力时间，最终获得的收益可能会远远低于所支付的成本。因为越往上层，其维护成本越高。尤其是 UI 层的元素会时常的发生改变。所以，我们应该把更多的自动化测试放在单元测试与接口测试阶段进行。
既然 UI 层的自动化测试这么劳民伤财，那我们只做单元测试与接口测试好了。 NO! 因为不管什么样的产品，最终呈现给用户的是 UI 层。所以，测试人员应该更多的精力放在 UI 层。那么也正是因为测试人员在 UI 层投入大量的精力，所以，我们有必要通过自动化的方式帮助我们“部分解放”重复的劳动。
在自动化测试中最怕的是变化，因为变化的直接结果就是导致测试用例的运行失败，那么就需要对自动化脚本进行维护；如何控制失败，降低维护成本对自化的成败至关重要。反过来讲，一份永远都运行成功的自动化测试用例是没有价值。  
至于在金字塔中三种测试的比例要根据实际的项目需求来划分。在《 google  测试之道》一书，对于 google 产品， 70% 的投入为单元测试， 20% 为集成、接口测试， 10%  为 UI 层的自动化测试。
我为什么要做自动化测试？
根据51testing的《中国软件测试从业人员调查报告》，手工测试占到的89% ，相对开发来说，测试的门槛底，薪资普遍较底，所要求的知识面虽然有一定广度，但缺乏深度。这是测试的普遍现状。
正因为手功测试人门槛不高，使大量的毕业生，甚至是非专业人员涌入这个行业。从而增加了这个行业的激烈竞争。对于工作几年扔处于手工测试的人员来说都会有强列的危机感。由于工作的技术含量不高，薪资的涨幅遇到瓶颈，另一方面受到新进入者的威胁，同样的工作公司花 5K 招来的人就可以做，那么就不会花 8K  的招。
好吧，这个问题不应该出现讨论技术的话题中，但他的确是大多测试人员不得不面对的一个问题。所以，从测试人员自身的发展来说，我其实非常需要通过自动化技术来增加自己有竞争力。当然，做到一定年限测试人员会选择转管理或其它岗位，这又是另一个话题了。
从测试行业的发展来说，国内产品由于产品特点，世界级的产品不多，技术含量相对不高，质量要求相对要求不高，外包国外项目，测试人力成本低廉，所以需要大量的手工测试人员。
所以，在不远的未来，我认为纯的工手测试人员的需求是递减，公司更需要更高技术能力的测试。质量需要测试，测试行为永远不会消失，但纯的手工测试人员是否消失是有可能的。
好吧，你可以说测试多朝阳的行业，我纯属在危言耸听。不管未来如何，我们都需要提升自身的技能对吧！
什么项目适合做自动化测试？
假如你已经决定要学习自动化测试了，如何学习是要面临的下一个问题？这个问题以被测试产品为出发点进行分析，假如你所学的技术不能得到应用（验证），将会使你的学习过程寸步难行。
首先考考虑产品是否适合做自动化测试。这方法比较普遍的共识是从三个方面进行权衡。
软件需求变动不频繁
测试脚本的稳定性决定了自动化测试的维护成本。如果软件需求变动过于频繁，测试人员需要根据变动的需求来更新测试用例以及相关的测试脚本，而脚本的维护本身就是一个代码开发的过程，需要修改、调试，必要的时候还要修改自动化测试的框架，如果所花费的成本不低于利用其节省的测试成本，那么自动化测试便是失败的。
项目中的某些模块相对稳定，而某些模块需求变动性很大。我们便可对相对稳定的模块进行自动化测试，而变动较大的仍是用手工测试。
项目周期较长
由于自动化测试需求的确定、自动化测试框架的设计、测试脚本的编写与调试均需要相当长的时间来完成。这样的过程本身就是一个测试软件的开发过程，需要较长的时间来完成。如果项目的周期比较短，没有足够的时间去支持这样一个过程，那么自动化测试便成为笑谈。
自动化测试脚本可重复使用
自动化测试脚本的重复使用要从三个方面来考量，一方面所测试的项目之间是否很大的差异性（如C/S 系统和 B/S 系统的差异 ）；所选择的测试工具是否适应这种差异；最后，测试人员是否有能力开发出适应这种差异的自动化测试框架。
选择什么工具进行自动化测试
假如你已经确认了XX  项目适合做自动化测试，那么接下来你要做的就是选测试工具了。
首先要先确认你所测试的产品是桌面程序（ C/S ）还是 web 应用（ B/S ）。
桌面程序的工具有： QTP 、  AutoRunner
web 应用的工具有： QTP 、 AutoRunner、 Robot Framework 、 watir 、 selenium
由于 B/S 架构的诸多优势，早几年前大量 C/S 架构的应用转为 B/S 结构。从而也推动了 web 开发与测试技术的发展。假如，被测试有产品是 C/S 架构的，那么推荐 QTP  ， QTP 在 UI 自动化测试领域占到了一半的试用率。所以，足以说明 QTP 在自动化领域强大，易用性等。学习主流的工具也可以使你获得更多的机会。市面上关于 QTP 的书籍也非常丰富。当然，要想学好 QTP  ，你必须要掌握 VBS 脚本语言。
如果，被测产品是 B/S  结构，那么推荐 selenium  ，为什么不是 QTP  或其它工具？因为 selenium  对 B/S 应用支持很好，更重要的一点，它支持多语言的开发，真正的试用 selenium  ，你所要掌握的不仅仅是一个工具而已，你还需要学习一门语言。我为什么要选择 selenium ？还要学一门语言，这无疑增加了我的学习成本。增加成本的同时，也增加的你的竞争力，而且，在这个过程中你不单单只是学会了一个自动化工具而已，你完全可以使用所学的语言去做更多的事情。
好吧！假如你决定试用 selenium  了之后，你又面临了一个新的问题，选择一门语言。 selenium  是支持 java、python、ruby、php、C#、JavaScript 。
从语言易学性来讲，首选 ruby  ， python
从语言应用广度来讲，首选java、C#、php、
从语言相关测试技术成度（及 资料）来讲：ruby ,python ,java
或者你可以考虑整个技术团队主流用什么语言，然后选择相应的语言。
selenium 用前须知
OK！经过上的过程，我相信你一定做出的相应的选择，如果你选择的是selenium 工具，那么接着往下阅读。
首选你在开始selenium之前，需要花一到两个月时间去学一门语言，这里是根据没有语言基础的同学而定的。我推荐ruby ,python ,java 任意一门语言来进行学习。
当然，已经如果有很好的语言基础略过这个环节，或者你的丰富的java编程能力，那么学习python 可能只需要几天时间或更短。
假如，你已经搞定了一门语言的基础，接下来你需要先了解selenium ，selenium 并不是单纯的一个工具，他是一组工具的集合，而且，他还有1.0与2.0之分，当然3.0也已经到来。
selenium 也不是简单一个工具，而是由几个工具组成，每个工具都有其特点和应用场景。
 
selenium IDE
selenium IDE 是嵌入到Firefox浏览器中的一个插件，实现简单的浏览器操作的录制与回放功能。那么什么情况下用到它呢？
快速的创建bug重现脚本，在测试人员的测试过程中，发现了bug之后可以通过IDE将重现的步骤录制下来，以帮助开发人员更容易的重现bug。
IDE录制的脚本可以可以转换成多种语言，从而帮助我们快速的开发脚本，关于这个功能后而用到时再详细介绍。
selenium Grid
Selenium Grid是一种自动化的测试辅助工具，Grid通过利用现有的计算机基础设施，能加快Web-app的功能测试。利用Grid，可以很方便地同时在多台机器上和异构环境中并行运行多个测试事例。其特点为：
· 并行执行
· 通过一个主机统一控制用例在不同环境、不同浏览器下运行。
· 灵活添加变动测试机
selenium RC
selenium RC 是selenium 家族的核心工具，selenium RC 支持多种不同的语言编写自动化测试脚本，通过selenium RC 的服务器作为代理服务器去访问应用从而达到测试的目的。
selenium RC 使用分Client Libraries和selenium Server，Client Libraries库主要主要用于编写测试脚本，用来控制selenium Server的库。
Selenium Server负责控制浏览器行为，总的来说，Selenium Server主要包括3个部分：Launcher、Http Proxy、Core。其中Selenium Core是被Selenium Server嵌入到浏览器页面中的。其实Selenium Core就是一堆JS函数的集合，就是通过这些JS函数，我们才可以实现用程序对浏览器进行操作。Launcher用于启动浏览器，把selnium Core加载到浏览器页面当中，并把浏览器的代理设置为Selenium Server 的Http Proxy。
selenium 2.0
搞清了selenium 1.0 的家族关系，selenium 2.0 是把WebDriver 加入到了这个家族中；简单用公式表示为：
selenium 2.0 = selenium 1.0 + WebDriver 
需要强调的是，在selenium 2.0 中主推的是WebDriver ，WebDriver 是selenium RC 的替代品，因为 selenium 为了向下兼容性，所以selenium RC 并没有彻底抛弃，如果你使用selenium开发一个新自动化测试项目，强列推荐使用WebDriver 。那么selenium RC 与webdriver 主要有什么区别呢？
selenium RC 在浏览器中运行JavaScript应用，使用浏览器内置的JavaScript 翻译器来翻译和执行selenese命令（selenese 是selenium命令集合）。
WebDriver通过原生浏览器支持或者浏览器扩展直接控制浏览器。WebDriver针对各个浏览器而开发，取代了嵌入到被测Web应用中的JavaScript。与浏览器的紧密集成支持创建更高级的测试，避免了JavaScript安全模型导致的限制。除了来自浏览器厂商的支持，WebDriver还利用操作系统级的调用模拟用户输入。
如果是新项目直接学习webdriver 就OK了，RC是过时技术。
selenium学习路线
配置你的测试环境，真对你所学习语言，来配置你相应的selenium 测试环境。selenium 好比定义的语义---“问好”，假如你使用的是中文，为了表术问好，你的写法是“你好”，假如你使用的是英语，你的写法是“hello”。 所以，同样有语义在不同的语言下会有不同的写法（语法）。
  接着你需要熟悉webdriver API ，API就是selenium 所定义一方法，用于定位，操作页面上的各种元素。
先学习元素的定位，selenium 提供了id、name、class name、 tag name、link text、partial link text、 xpath、css、等定位方法。xpath 和 css  功能强大语法稍微复杂，在这其间你可能还需要了解更多的前端知识。 xml ,javascript  等。
定位元素的目的是为了操作元素，接就要学习各种元素有操作，输入框，下拉框，按钮点击，文件上传、下载，分页，对话框，警告框 ... 等等。
经过一段时间的学习，你可以游刃有余的模拟手工测试来操作页面上的各种元素了。接着你需要做的就是把这些“用例”组织起来，统一来跑。
那么你需要做的就是学习并使用单元测试框架，单元测试框架本身就解决了用例的组织与运行。
当你写了一些“测试用例” 之后，你会发现用例中有大量重复的操作，能不能写到一个单独的文件中，需要的时候调用这些操作？当然可以，运用你的编程能力来实现这一点将非常简单。然后，你又发现每个用例中都有一些数据，这些数据也是一样的，但如果变化了修改起来非常麻烦，你也可以把他写到一个单独的文件中进行读取。
接着你又遇到了新的疑问，我写的脚本（用例）都是流水式的，我怎么知道用例运行失败还是成功。那么就需要在脚本中加一些验证与断言。
接着你又有了更多的想法，单元测试框架的 log 太简陋了，能不能生成一张漂亮的测试报告出来。我能不能定时的来跑这个脚本。能不能把每一次跑脚本的测试结果直接发到我的邮箱。能不能 ......
为解决这些问题，你不得不学习更多的编程技术，然后你的“测试结构”会功能越来越强大，越来越灵活。产生了一定的通用性和移植性。一个有模有样的自动化测试框架诞生了。
  假如，有一天你不再做 UI 的自动化测试了，你会发现你去做单元测试 或接口测试基本没什么难度。开发个测试工具之类的也不在话下，感谢 selenium  吧！顺便也感谢一下我吧！
——————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
　
csv是(逗号分隔值)的英文缩写,csv文件是一种用来存储数据的纯文本文件,通常都是用于存放电子表格或数据的一种文件格式。用office excel 2007软件也可以打开csv文件。
——————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
　
1、首先你得先定位自己需要开放自动化测试工具为哪类，例如：自动化测试用例管理工具，自动化测试框架类、界面自动化测试工具等
　　2、根据你所需的自动化测试工具类，对应相应的技能
　　1）基本技能；编程知识（其实哪一种语言都能进行自动化测试工具开发，不过我推荐：想掌握一些软件开发方面高级一些的思想，可以学学java的OO，一般而言，自动化测试需要开发效率比较高，则可以应用一些脚本方面的语言，例如：tcl、python等。所以，首先，先掌握基本的编程语法以及编程思想
　　另外的一个基本技能：你需要简单的去了解一下自动化测试以及其中的一些应用模式，而且需要对测试流程以及基本的测试方法进行学习，就像你做软件工程一样，你也需要适应一定的软件流程，例如：瀑布式、RUP式。
　　3）之后，对应不同的工具学习不同的东西，例如：你开发一个简易的界面自动化测试工具，WIN32、web以及java都是用不同的语言，像win32的话，就需要去掌握MS的一些基本知识，例如:句柄的概念、MSAA接口概念等。java的swing界面的话，就得去看java底层的事件机制，web的话，就去好好了解一下web中的html节点元素，还有js等。或者你想做一个手机自动化测试工具，andriod，则需要对android的开发进行一些了解了。这就叫应用不同的技能满足不同的需求阶段。当然，还有各种不同的自动化测试工具，例如：测试管理以及连接类、CLI命令行控制类，日志生成类等 ，这都是在平时工作中进行总结出来的一些经验，总之，先打好基础。如果，有什么问题或者想法的话，可以发邮件于我：test_sunny@hotmail.com(散步的SUN），ok，祝你学习愉快~欢迎来到自动化测试的小世界
——————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
　
Chrome于12月全面转向HTML5(图片来自新浪)

　　谷歌表示，从2016年9月的Chrome 53开始，Chrome浏览器将屏蔽在后台加载的Flash内容，估计这类内容占互联网上Flash内容的90%。今年12月，Chrome将把HTML5设为网页核心内容，例如游戏和视频的默认选项，但仅支持Flash的网站除外。
　　Flash一直被默认集成在谷歌浏览器中，但其重要性正逐渐被削弱。这主要是由于Flash所带来了信息安全问题。2015年9月，Chrome 45开始暂停自动加载不重要的Flash内容，包括广告、动画，以及任何“非网页中心”的内容。
　　除此之外，Mozilla和微软也在逐步舍弃Flash。这些浏览器厂商的最终目标是让尽可能多的网站转向HTML5。HTML5技术能带来更好的性能(降低内存消耗和CPU占用率，提升电池续航时间)，同时也更符合网页标准(这将使开发者的工作更简单)。而考虑到Flash的信息安全问题，转向HTML5也将优化安全性。

