# 代码整洁的-Java

## 目录

  1. [简介](#简介)
  2. [变量](#变量)
  3. [函数](#函数)
  4. [对象和数据结构](#对象和数据结构)
  5. [类](#类)
  6. [SOLID](#solid)
  7. [测试](#测试)
  8. [错误处理](#错误处理)
  9. [格式化](#格式化)
  10. [注释](#注释)
  11. [工具](#工具)


## 简介

![一张用你阅读代码时吐槽的数量来评估软件质量的搞笑图片](https://user-gold-cdn.xitu.io/2020/3/1/17093d303918263e?w=500&h=471&f=jpeg&s=45533)

将源自 Robert C. Martin 的 [*Clean Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)
的软件工程原则适配到 Java 。 这不是一个代码风格指南， 它是一个使用 Java 来生产
可读的, 可重用的, 以及可重构的软件的指南。

这里的每一项原则都不是必须遵守的， 甚至只有更少的能够被广泛认可。 这些仅仅是指南而已， 但是却是
*Clean Code* 作者多年经验的结晶。

我们的软件工程行业只有短短的 50 年， 依然有很多要我们去学习。 当软件架构与建筑架构一样古老时，
也许我们将会有硬性的规则去遵守。 而现在， 让这些指南做为你和你的团队生产的 Java 代码的
质量的标准。

还有一件事： 知道这些指南并不能马上让你成为一个更加出色的软件开发者， 并且使用它们工作多年也并
不意味着你不再会犯错误。 每一段代码最开始都是草稿， 像湿粘土一样被打造成最终的形态。 最后当我们
和搭档们一起审查代码时清除那些不完善之处, 不要因为最初需要改善的草稿代码而自责， 而是对那些代
码下手。

## **变量**

### 使用有意义并且可读的变量名称

**不好的：**
```
 String yyyymmdstr = new SimpleDateFormat("YYYY/MM/DD").format(new Date());
```

**好的：**
```
 String currentDate = new SimpleDateFormat("YYYY/MM/DD").format(new Date());
```
**[⬆ 返回顶部](#代码整洁的-Java)**

### 为相同类型的变量使用相同的词汇

**不好的：**
```
  getUserInfo();
  getClientData();
  getCustomerRecord();
```

**好的：**
```
  getUser();
```
**[⬆ 返回顶部](#代码整洁的-Java)**
### 使用可搜索的名称

我们要阅读的代码比要写的代码多得多， 所以我们写出的代码的可读性和可搜索性是很重要的。 使用没有
意义的变量名将会导致我们的程序难于理解， 将会伤害我们的读者， 所以请使用可搜索的变量名。 类似
[buddy.js](https://github.com/danielstjules/buddy.js) 和 [ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md)
的工具可以帮助我们找到未命名的常量。

**不好的：**
```
// 艹， 86400000 是什么鬼？
 setTimeout(blastOff, 86400000);

```

**好的：**
```
// 将它们声明为全局常量。
 public static final int MILLISECONDS_IN_A_DAY = 86400000;
 setTimeout(blastOff, MILLISECONDS_IN_A_DAY);

```
**[⬆ 返回顶部](#代码整洁的-Java)**

### 使用解释性的变量
**不好的：**
```
 String address = "One Infinite Loop, Cupertino 95014";
 String cityZipCodeRegex = "/^[^,\\\\]+[,\\\\\\s]+(.+?)\\s*(\\d{5})?$/";

 saveCityZipCode(address.split(cityZipCodeRegex)[0],
 address.split(cityZipCodeRegex)[1]);
```

**好的：**
```
  String address = "One Infinite Loop, Cupertino 95014";
  String cityZipCodeRegex = "/^[^,\\\\]+[,\\\\\\s]+(.+?)\\s*(\\d{5})?$/";

  String city = address.split(cityZipCodeRegex)[0];
  String zipCode = address.split(cityZipCodeRegex)[1];

  saveCityZipCode(city, zipCode);
```
**[⬆ 返回顶部](#代码整洁的-Java)**

### 避免心理映射
显示比隐式更好

**不好的：**
```
 String [] l = {"Austin", "New York", "San Francisco"};

        for (int i = 0; i < l.length; i++) {
            String li = l[i];
            doStuff();
            doSomeOtherStuff();
            // ...
            // ...
            // ...
            // Wait, what is `$li` for again?
            dispatch(li);
        }
```

**好的：**
```
 String[] locations = {"Austin", "New York", "San Francisco"};

        for (String location : locations) {
            doStuff();
            doSomeOtherStuff();
            // ...
            // ...
            // ...
            dispatch(location);
        }
```
**[⬆ 返回顶部](#代码整洁的-Java)**

### 不添加不必要的上下文

如果你的类名/对象名有意义， 不要在变量名上再重复。

**不好的：**
```
  class Car {
            public String carMake = "Honda";
            public String carModel = "Accord";
            public String carColor = "Blue";
        }

        void paintCar(Car car) {
            car.carColor = "Red";
        }
```

**好的：**
``` 
  class Car {
            public String make = "Honda";
            public String model = "Accord";
            public String color = "Blue";
        }

        void paintCar(Car car) {
            car.color = "Red";
        }
```
**[⬆ 返回顶部](#代码整洁的-Java)**

## **函数**

### 函数参数 (两个以下最理想)

限制函数参数的个数是非常重要的， 因为这样将使你的函数容易进行测试。 一旦超过三个参数将会导致组
合爆炸， 因为你不得不编写大量针对每个参数的测试用例。

没有参数是最理想的， 一个或者两个参数也是可以的， 三个参数应该避免， 超过三个应该被重构。 通常，
如果你有一个超过两个函数的参数， 那就意味着你的函数尝试做太多的事情。 如果不是， 多数情况下一个
更高级对象可能会满足需求。

当你发现你自己需要大量的参数时， 你可以使用一个对象。

**不好的：**
```
void createMenu(String title,String body,String buttonText,boolean cancellable){}
```

**好的：
```
 class MenuConfig{
            String title;
            String body;
            String buttonText;
            boolean cancellable;
        }
        void  createMenu(MenuConfig menuConfig){}
```
**[⬆ 返回顶部](#代码整洁的-Java)**


### 函数应当只做一件事情

这是软件工程中最重要的一条规则， 当函数需要做更多的事情时， 它们将会更难进行编写、 测试和推理。
当你能将一个函数隔离到只有一个动作， 他们将能够被容易的进行重构并且你的代码将会更容易阅读。 如
果你严格遵守本指南中的这一条， 你将会领先于许多开发者。

**不好的：**
```
 public void emailClients(List<Client> clients) {
            for (Client client : clients) {
                Client clientRecord = repository.findOne(client.getId());
                if (clientRecord.isActive()){
                    email(client);
                }
            }
        }
```

**好的：**
```
 public void emailClients(List<Client> clients) {
            for (Client client : clients) {
                if (isActiveClient(client)) {
                    email(client);
                }
            }
        }
 private boolean isActiveClient(Client client) {
            Client clientRecord = repository.findOne(client.getId());
            return clientRecord.isActive();
        }
```
**[⬆ 返回顶部](#代码整洁的-Java)**

### 函数名称应该说明它要做什么

**不好的：**
```
 private void addToDate(Date date, int month){
            //..
        }

        Date date = new Date();

        // It's hard to to tell from the method name what is added
        addToDate(date, 1);
```

**好的：**
```
  private void addMonthToDate(Date date, int month){
            //..
        }

        Date date = new Date();
        addMonthToDate(1, date);
```
**[⬆ 返回顶部](#代码整洁的-Java)**

### 函数应该只有一个抽象级别

当在你的函数中有多于一个抽象级别时， 你的函数通常做了太多事情。 拆分函数将会提升重用性和测试性。

**不好的：**
```
 void parseBetterJSAlternative(String code){
        String[] REGECES={};
        String[] statements=code.split(" ");
        String[] tokens={};
        for(String regex: Arrays.asList(REGECES)){
               for(String statement:Arrays.asList(statements)){
                   //...
                }
            }
        String[] ast={};
        for(String token:Arrays.asList(tokens)){
                //lex ...
            }

        for(String node:Arrays.asList(ast)){
                //parse ...
            }
        }
```

**好的：**
```
 String[] tokenize(String code){
        String[] REGECES={};
        String[] statements=code.split(" ");
        String[] tokens={};
        for(String regex: Arrays.asList(REGECES)){
            for(String statement:Arrays.asList(statements)){
                    //tokens push
                }
            }
        return tokens;
        }

        String[] lexer(String[] tokens){
        String[] ast={};
        for(String token:Arrays.asList(tokens)){
                //ast push
        }
        return ast;
        }

        void parseBetterJSAlternative(String code){
        String[] tokens=tokenize(code);
        String[] ast=lexer(tokens);
        for(String node:Arrays.asList(ast)){
                //parse ...
            }
        }
```
**[⬆ 返回顶部](#代码整洁的-Java)**

### 移除冗余代码

竭尽你的全力去避免冗余代码。 冗余代码是不好的， 因为它意味着当你需要修改一些逻辑时会有多个地方
需要修改。

想象一下你在经营一家餐馆， 你需要记录所有的库存西红柿， 洋葱， 大蒜， 各种香料等等。 如果你有多
个记录列表， 当你用西红柿做一道菜时你得更新多个列表。 如果你只有一个列表， 就只有一个地方需要更
新！

你有冗余代码通常是因为你有两个或多个稍微不同的东西， 它们共享大部分， 但是它们的不同之处迫使你使
用两个或更多独立的函数来处理大部分相同的东西。 移除冗余代码意味着创建一个可以处理这些不同之处的
抽象的函数/模块/类。

让这个抽象正确是关键的， 这是为什么要你遵循 *Classes* 那一章的 SOLID 的原因。 不好的抽象比冗
余代码更差， 所以要谨慎行事。 既然已经这么说了， 如果你能够做出一个好的抽象， 才去做。 不要重复
你自己， 否则你会发现当你要修改一个东西时时刻需要修改多个地方。

**不好的：**
```
 void showDeveloperList(List<Developer> developers){
            for(Developer developer:developers){
                render(new Data(developer.expectedSalary,developer.experience,developer.githubLink));
            }
        }

 void showManagerrList(List<Manager> managers){
            for(Manager manager:managers){
                render(new Data(manager.expectedSalary,manager.experience,manager.portfolio));
            }
        }
```

**好的：**
```
 void showList(List<Employee> employees){
            for(Employee employee:employees){
                Data data=new Data(employee.expectedSalary,employee.experience,employee.githubLink);
                String portfolio=employee.portfolio;
                if("manager".equals(employee)){
                    portfolio=employee.portfolio;
                }
                data.portfolio=portfolio;
                render(data);
            }
        }
```
**[⬆ 返回顶部](#代码整洁的-Java)**

### 不要使用标记位做为函数参数

标记位是告诉你的用户这个函数做了不只一件事情。 函数应该只做一件事情。 如果你的函数因为一个布尔值
出现不同的代码路径， 请拆分它们。

**不好的：**
```
void createFile(String name,boolean temp){
        if(temp){
            new File("./temp"+name);
        }else{
           new File(name);
        }
    }
```

**好的：**
```
void createFile(String name){
        new File(name);
    }

void createTempFile(String name){
        new File("./temp"+name);
    }
```
**[⬆ 返回顶部](#代码整洁的-Java)**

### 避免副作用

如果一个函数做了除接受一个值然后返回一个值或多个值之外的任何事情， 它将会产生副作用， 它可能是
写入一个文件， 修改一个全局变量， 或者意外的把你所有的钱连接到一个陌生人那里。

现在在你的程序中确实偶尔需要副作用， 就像上面的代码， 你也许需要写入到一个文件， 你需要做的是集
中化你要做的事情， 不要让多个函数或者类写入一个特定的文件， 用一个服务来实现它， 一个并且只有一
个。

重点是避免这些常见的易犯的错误， 比如在对象之间共享状态而不使用任何结构， 使用任何地方都可以写入
的可变的数据类型， 没有集中化导致副作用。 如果你能做到这些， 那么你将会比其它的码农大军更加幸福。

**不好的：**
```
    String name="Ryan McDermott";
    void splitIntoFirstAndLastName(){
            name=name.split(" ").toString();
    }
    splitIntoFirstAndLastName();
    System.out.println(name);
```

**好的：**
```
    String name="Ryan McDermott";
    String splitIntoFirstAndLastName(){
            return name.split(" ").toString();
        }
    String newName=splitIntoFirstAndLastName();
    System.out.println(name);
    System.out.println(newName);
```
**[⬆ 返回顶部](#代码整洁的-Java)**


### 函数式编程优于指令式编程

函数式语言更加简洁
并且更容易进行测试， 当你可以使用函数式编程风格时请尽情使用。

**不好的：**
```
    List<Integer> programmerOutput=new ArrayList<>();
    programmerOutput.add(500);
    programmerOutput.add(1500);
    programmerOutput.add(150);
    programmerOutput.add(1000);
    int totalOutput=0;
    for(int i=0;i<programmerOutput.size();i++){
        totalOutput+=programmerOutput.get(i);
    }
```

**好的：**
```
    List<Integer> programmerOutput=new ArrayList<>();
    programmerOutput.add(500);
    programmerOutput.add(1500);
    programmerOutput.add(150);
    programmerOutput.add(1000);
    int totalOutput= programmerOutput.stream().filter(programmer -> programmer > 500).mapToInt(programmer -> programmer).sum();
```
**[⬆ 返回顶部](#代码整洁的-Java)**

### 封装条件语句

**不好的：**
```
    if(fsm.state.equals("fetching")&&listNode.isEmpty(){
            //...
        }
```

**好的：**
```
    void shouldShowSpinner(Fsm fsm, String listNode) {
            return fsm.state.equals("fetching")&&listNode.isEmpty();
        }

    if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
            // ...
        }
```
**[⬆ 返回顶部](#代码整洁的-Java)**

### 避免负面条件

**不好的：**
```
    void isDOMNodeNotPresent(Node node) {
            // ...
        }

    if (!isDOMNodeNotPresent(node)) {
            // ...
        }
```

**好的：**
```
    void isDOMNodePresent(Node node) {
                // ...
        }

    if (isDOMNodePresent(node)) {
            // ...
        }
```
**[⬆ 返回顶部](#代码整洁的-Java)**

### 避免条件语句

这看起来似乎是一个不可能的任务。 第一次听到这个时， 多数人会说： “没有 `if` 语句还能期望我干
啥呢”， 答案是多数情况下你可以使用多态来完成同样的任务。 第二个问题通常是 “好了， 那么做很棒，
但是我为什么想要那样做呢”， 答案是我们学到的上一条代码整洁之道的理念： 一个函数应当只做一件事情。
当你有使用 `if` 语句的类/函数是， 你在告诉你的用户你的函数做了不止一件事情。 记住： 只做一件
事情。

**不好的：**
```
 class Airplane{
        int getCurisingAltitude(){
            switch(this.type){
                case "777":
                    return this.getMaxAltitude()-this.getPassengerCount();
                case "Air Force One":
                    return this.getMaxAltitude();
                case "Cessna":
                    return this.getMaxAltitude() - this.getFuelExpenditure();
                }
            }
        }
```

**好的：**
```
    class Airplane {
            // ...
        }

    class Boeing777 extends Airplane {
            // ...
        int getCruisingAltitude() {
            return this.getMaxAltitude() - this.getPassengerCount();
        }
    }

    class AirForceOne extends Airplane {
        // ...
        int getCruisingAltitude() {
            return this.getMaxAltitude();
           }
    }

    class Cessna extends Airplane {
        // ...
        int getCruisingAltitude() {
            return this.getMaxAltitude() - this.getFuelExpenditure();
        }
    }
```
**[⬆ 返回顶部](#代码整洁的-Java)**

### 移除僵尸代码

僵死代码和冗余代码同样糟糕。 没有理由在代码库中保存它。 如果它不会被调用， 就删掉它。 当你需要
它时， 它依然保存在版本历史记录中。

**不好的：**
```
    void  oldRequestModule(String url) {
        // ...
    }

    void  newRequestModule(String url) {
        // ...
    }

    String  req = newRequestModule;
    inventoryTracker("apples", req, "www.inventory-awesome.io");

```

**好的：**
```
    void  newRequestModule(String url) {
        // ...
    }

    String  req = newRequestModule;
    inventoryTracker("apples", req, "www.inventory-awesome.io");
```
**[⬆ 返回顶部](#代码整洁的-Java)**

## **对象和数据结构**

### 使用 getters 和 setters

 使用 getters 和 setters 来访问对象上的数据比简单的在一个对象上查找属性
要好得多。 “为什么？” 你可能会问， 好吧， 原因请看下面的列表：

* 当你想在获取一个对象属性的背后做更多的事情时， 你不需要在代码库中查找和修改每一处访问；
* 使用 `set` 可以让添加验证变得容易；
* 封装内部实现；
* 使用 getting 和 setting 时， 容易添加日志和错误处理；
* 继承这个类， 你可以重写默认功能；
* 你可以延迟加载对象的属性， 比如说从服务器获取。

**不好的：**
```
    class BankAccount{
            public int balance=1000;
        }
    BankAccount bankAccount=new BankAccount();
    bankAccount.balance-=100;
```

**好的：**
```
class BankAccount{
    private int blance=1000;
    public int getBlance() {
         return blance;
    }

    public void setBlance(int blance) {
        if(verifyIfAmountCanBeSetted(blance)){
                this.blance = blance;
            }
        }

        void verifyIfAmountCanBeSetted(int amount){
            //...
        }
    }
    BankAccount bankAccount=new BankAccount();
    bankAccount.setBlance(2000);
    int balance=bankAccount.getBlance();
```
**[⬆ 返回顶部](#代码整洁的-Java)**

## **类**

### 使用方法链

这个模式在 Java 中是非常有用的， 并且你可以在许多类库比如 Glide 和 OkHttp 中见到。
它使你的代码变得富有表现力， 并减少啰嗦。 因为这个原因， 我说， 使用方法链然后再看看你的代码
会变得多么简洁。 在你的类／方法中， 简单的在每个方法的最后返回 `this` ， 然后你就能把这个类的
其它方法链在一起。

**不好的：**
```
    class Car{
        private String make;
        private String model;
        private String color;

        public void setMake(String make) {
            this.make = make;
         }

         public void setModel(String model) {
            this.model = model;
        }

        public void setColor(String color) {
            this.color = color;
        }

        public void save(){
            console.log(this.make, this.model, this.color);
        }
    }

    Car car=new Car();
    car.setColor("pink");
    car.setMake("Ford");
    car.setModel("F-150");
    car.save();
```

**好的：**
```
    class Car{
        private String make;
        private String model;
        private String color;

        public Car setMake(String make) {
            this.make = make;
            return this;
        }

        public Car setModel(String model) {
            this.model = model;
            return this;
        }

        public Car setColor(String color) {
            this.color = color;
            return this;
        }

        public Car save(){
            console.log(this.make, this.model, this.color);
            return this;
        }
    }

    Car car=new Car()
    .setColor("pink")
    .setMake("Ford")
    .setModel("F-150")
    .save();
```
**[⬆ 返回顶部](#代码整洁的-Java)**

### 组合优先于继承

正如[*设计模式四人帮*](https://en.wikipedia.org/wiki/Design_Patterns)所述， 如果可能，
你应该优先使用组合而不是继承。 有许多好的理由去使用继承， 也有许多好的理由去使用组合。这个格言
的重点是， 如果你本能的观点是继承， 那么请想一下组合能否更好的为你的问题建模。 很多情况下它真的
可以。

那么你也许会这样想， “我什么时候改使用继承？” 这取决于你手上的问题， 不过这儿有一个像样的列表说
明什么时候继承比组合更好用：

1. 你的继承表示"是一个"的关系而不是"有一个"的关系（人类->动物 vs 用户->用户详情）；
2. 你可以重用来自基类的代码（人可以像所有动物一样行动）；
3. 你想通过基类对子类进行全局的修改（改变所有动物行动时的热量消耗）；

**不好的：**
```
    class Employee{
        private String name;
        private String email;
    }
    // 不好是因为雇员“有”税率数据， EmployeeTaxData 不是一个 Employee 类型。
    class EmployeeTaxData extends Employee{
        private String ssn;
        private String salary;
    }

```

**好的：**
```
 class EmployeeTaxData{
        private String ssn;
        private String salary;

        public EmployeeTaxData(String ssn, String salary) {
            this.ssn = ssn;
            this.salary = salary;
        }
    }
class Employee{
        private String name;
        private String email;
        private EmployeeTaxData taxData;
        void setTaxData(String ssn,String salary){
            this.taxData=new EmployeeTaxData(ssn,salary);
        }
    }
```
**[⬆ 返回顶部](#代码整洁的-Java)**

## **SOLID**

### 单一职责原则 (SRP)

正如代码整洁之道所述， “永远不要有超过一个理由来修改一个类”。 给一个类塞满许多功能， 就像你在航
班上只能带一个行李箱一样， 这样做的问题你的类不会有理想的内聚性， 将会有太多的理由来对它进行修改。
最小化需要修改一个类的次数时很重要的， 因为如果一个类拥有太多的功能， 一旦你修改它的一小部分，
将会很难弄清楚会对代码库中的其它模块造成什么影响。

**不好的：**
```
 class UserSettings {
        User user;

        void changeSettings(UserSettings settings) {
            if (this.verifyCredentials()) {
                // ...
            }
        }

        void verifyCredentials() {
            // ...
        }
    }
```

**好的：**
```
    User user;
    UserAuth auth;

    public UserSettings(User user) {
        this.user = user;
        this.auth = new UserAuth(user);
    }

    void changeSettings(UserSettings settings) {
        if (this.auth.verifyCredentials()) {
            // ...
        }
    }

```
**[⬆ 返回顶部](#代码整洁的-Java)**

### 开闭原则 (OCP)

Bertrand Meyer 说过， “软件实体 (类， 模块， 函数等) 应该为扩展开放， 但是为修改关闭。” 这
是什么意思呢？ 这个原则基本上说明了你应该允许用户添加功能而不必修改现有的代码。

**不好的：**
```
    class AjaxAdapter extends Adapter {
        private String name;
        public  AjaxAdapter() {
            this.name = "ajaxAdapter";
        }
    }

    class NodeAdapter extends Adapter {
        private String name;
        public NodeAdapter() {
            this.name = "nodeAdapter";
        }
    }

    class HttpRequester {
        public HttpRequester(Adapter adapter) {
            this.adapter = adapter;
        }

        void fetch(String url) {
            if ("ajaxAdapter".equals(this.adapter.name)) {
                makeAjaxCall(url);
            } else if ("httpNodeAdapter".equals(this.adapter.name)) {
                makeHttpCall(url);
            }
        }
    }

        void  makeAjaxCall(String url) {
        // request and return promise
        }

        void  makeHttpCall(String url) {
        // request and return promise
        }
```

**好的：**
```
    class AjaxAdapter extends Adapter {
        private String name;
        public  AjaxAdapter() {
            this.name = "ajaxAdapter";
        }
        void request(String url){

        }
    }

    class NodeAdapter extends Adapter {
        private String name;
        public NodeAdapter() {
            this.name = "nodeAdapter";
        }
        void request(String url){

        }
    }

    class HttpRequester {
        public  HttpRequester(Adapter adapter) {
            this.adapter = adapter;
        }

        void fetch(String url) {
            this.adapter.request(url);
        }
    }
```
**[⬆ 返回顶部](#代码整洁的-Java)**

### 里氏代换原则 (LSP)

这是针对一个非常简单的里面的一个恐怖意图， 它的正式定义是： “如果 S 是 T 的一个子类型， 那么类
型为 T 的对象可以被类型为 S 的对象替换（例如， 类型为 S 的对象可作为类型为 T 的替代品）儿不需
要修改目标程序的期望性质 （正确性、 任务执行性等）。” 这甚至是个恐怖的定义。

最好的解释是， 如果你又一个基类和一个子类， 那个基类和字类可以互换而不会产生不正确的结果。 这可
能还有有些疑惑， 让我们来看一下这个经典的正方形与矩形的例子。 从数学上说， 一个正方形是一个矩形，
但是你用 "is-a" 的关系用继承来实现， 你将很快遇到麻烦。

**不好的：**
```
    class Rectangle {
        protected int width;
        protected int height;
        public Rectangle() {
            this.width = 0;
            this.height = 0;
        }

        void setColor(String color) {
           // ...
        }

        void render(int  area) {
            // ...
        }

        void setWidth(int width) {
            this.width = width;
        }

        void setHeight(int height) {
            this.height = height;
        }

        int getArea() {
            return this.width * this.height;
        }
    }

    class Square extends Rectangle {
        void setWidth(int width) {
            this.width = width;
            this.height = width;
        }

        void setHeight(int height) {
            this.width = height;
            this.height = height;
        }
    }

    void  renderLargeRectangles(List<Rectangle> rectangles) {
        for(Rectangle rectangle:rectangles) {
            rectangle.setWidth(4);
            rectangle.setHeight(5);
            int area = rectangle.getArea(); // BAD: Will return 25 for Square. Should be 20.
            rectangle.render(area);
        }
    }

    List<Rectangle> rectangles=new ArrayList<>();
    rectangles.add(new Rectangle());
    rectangles.add(new Rectangle());
    rectangles.add(new Square());
    renderLargeRectangles(rectangles);
```

**好的：**
```
    class Shape {
        void setColor(String color) {
            // ...
        }

        int getArea() {
        }

        void render(int area) {
            // ...
            }
    }

    class Rectangle extends Shape {
        private int width;
        private int height;
        public Rectangle(int width, int height) {
            super();
            this.width = width;
            this.height = height;
        }

        int getArea() {
            return this.width * this.height;
            }
        }

    class Square extends Shape {
        private int length;
        public Square(int length) {
            super();
            this.length = length;
        }

        int getArea() {
            return this.length * this.length;
        }
    }

    void  renderLargeShapes(List<Shape> shapes) {
        for(Shape shape:shapes) {
            int area = shape.getArea();
            shape.render(area);
        }
    }

    List<Shape> shapes=new ArrayList<>();
    shapes.add(new Rectangle(4,5));
    shapes.add(new Rectangle(4,5));
    shapes.add(new Square(5));
    renderLargeShapes(shapes);
```
**[⬆ 返回顶部](#代码整洁的-Java)**

### 接口隔离原则 (ISP)

接口隔离原则说的是 “客户端不应该强制依赖他们不需要的接口。”

**不好的：**
```
  interface I {
            public void method1();
            public void method2();
            public void method3();
            public void method4();
            public void method5();
        }

        class A{
            public void depend1(I i){
                i.method1();
            }
            public void depend2(I i){
                i.method2();
            }
            public void depend3(I i){
                i.method3();
            }
        }



        class B{
            public void depend1(I i){
                i.method1();
            }
            public void depend2(I i){
                i.method4();
            }
            public void depend3(I i){
                i.method5();
            }
        }

        class C implements I{
            public void method1() {
                System.out.println("类B实现接口I的方法1");
            }
            public void method2() {
                System.out.println("类B实现接口I的方法2");
            }
            public void method3() {
                System.out.println("类B实现接口I的方法3");
            }
            //对于类B来说，method4和method5不是必需的，但是由于接口A中有这两个方法，
            //所以在实现过程中即使这两个方法的方法体为空，也要将这两个没有作用的方法进行实现。
            public void method4() {}
            public void method5() {}
        }

        class D implements I{
            public void method1() {
                System.out.println("类D实现接口I的方法1");
            }
            //对于类D来说，method2和method3不是必需的，但是由于接口A中有这两个方法，
            //所以在实现过程中即使这两个方法的方法体为空，也要将这两个没有作用的方法进行实现。
            public void method2() {}
            public void method3() {}

            public void method4() {
                System.out.println("类D实现接口I的方法4");
            }
            public void method5() {
                System.out.println("类D实现接口I的方法5");
            }
        }
```

**好的：**
```
   interface I1 {
            public void method1();
        }

        interface I2 {
            public void method2();
            public void method3();
        }

        interface I3 {
            public void method4();
            public void method5();
        }

        class A{
            public void depend1(I1 i){
                i.method1();
            }
            public void depend2(I2 i){
                i.method2();
            }
            public void depend3(I2 i){
                i.method3();
            }
        }

        class B{
            public void depend1(I1 i){
                i.method1();
            }
            public void depend2(I3 i){
                i.method4();
            }
            public void depend3(I3 i){
                i.method5();
            }
        }


        class C implements I1, I2{
            public void method1() {
                System.out.println("类B实现接口I1的方法1");
            }
            public void method2() {
                System.out.println("类B实现接口I2的方法2");
            }
            public void method3() {
                System.out.println("类B实现接口I2的方法3");
            }
        }


        class D implements I1, I3{
            public void method1() {
                System.out.println("类D实现接口I1的方法1");
            }
            public void method4() {
                System.out.println("类D实现接口I3的方法4");
            }
            public void method5() {
                System.out.println("类D实现接口I3的方法5");
            }
```
**[⬆ 返回顶部](#代码整洁的-Java)**

### 依赖反转原则 (DIP)

这个原则阐述了两个重要的事情：

1. 高级模块不应该依赖于低级模块， 两者都应该依赖与抽象；
2. 抽象不应当依赖于具体实现， 具体实现应当依赖于抽象。

这个一开始会很难理解， 但是如果你使用过 Angular.js ， 你应该已经看到过通过依赖注入来实现的这
个原则， 虽然他们不是相同的概念， 依赖反转原则让高级模块远离低级模块的细节和创建， 可以通过 DI
来实现。 这样做的巨大益处是降低模块间的耦合。 耦合是一个非常糟糕的开发模式， 因为会导致代码难于
重构。

如上所述， JavaScript 没有接口， 所以被依赖的抽象是隐式契约。 也就是说， 一个对象/类的方法和
属性直接暴露给另外一个对象/类。 在下面的例子中， 任何一个 Request 模块的隐式契约 `InventoryTracker`
将有一个 `requestItems` 方法。

**不好的：**
```
  class InventoryRequester {
        private String REQ_METHODS;
        public InventoryRequester() {
            this.REQ_METHODS = "HTTP";
        }

        void requestItem(String item) {
            // ...
        }
    }

    class InventoryTracker {
        private List<String> items;
        private InventoryRequester requester;
        public InventoryTracker(List<String> items) {
            this.items = items;

            // 不好的： 我们已经创建了一个对请求的具体实现的依赖， 我们只有一个 requestItems 方法依
            // 赖一个请求方法 'request'
            this.requester = new InventoryRequester();
        }

        void requestItems() {
            this.items.stream().forEach(item->this.requester.requestItem(item));
        }
    }

    List<String> items=new ArrayList<>();
    items.add("apples");
    items.add("bananas");
    InventoryTracker inventoryTracker = new InventoryTracker(items);
    inventoryTracker.requestItems();
```

**好的：**
```
  interface Requester{
            void requestItem(String item);
        }


    class InventoryTracker {
        List<String> items;
        Requester requester;
        public InventoryTracker(List<String> items, Requester requester) {
            this.items = items;
            this.requester = requester;
        }

        void requestItems() {
            this.items.stream().forEach(item->requester.requestItem(item));
        }
    }

    class InventoryRequesterV1 implements Requester{
        String REQ_METHODS;
        public InventoryRequesterV1() {
            this.REQ_METHODS="HTTP";
        }

        @Override
        public void requestItem(String item) {
            // ...
        }
    }

    class InventoryRequesterV2 implements Requester{
        String REQ_METHODS;
        public InventoryRequesterV2() {
            this.REQ_METHODS="WS";
        }

        @Override
        public void requestItem(String item) {
            // ...
        }
    }
    // 通过外部创建依赖项并将它们注入， 我们可以轻松的用一个崭新的使用 WebSockets 的请求模块进行替换。
    List<String> items=new ArrayList<>();
    items.add("apples");
    items.add("bananas");
    InventoryTracker inventoryTracker = new InventoryTracker(items, new InventoryRequesterV2());
    inventoryTracker.requestItems();
```
**[⬆ 返回顶部](#代码整洁的-Java)**

## **测试**

测试比发布更加重要。 如果你没有测试或者测试不够充分， 每次发布时你就不能确认没有破坏任何事情。
测试的量由你的团队决定， 但是拥有 100% 的覆盖率(包括所有的语句和分支)是你为什么能达到高度自信
和内心的平静。 这意味着需要一个额外的伟大的测试框架， 也需要一个好的[覆盖率工具](http://gotwarlost.github.io/istanbul/)。

没有理由不写测试。 这里有[大量的优秀的 JS 测试框架](http://jstherightway.org/#testing-tools)，
选一个适合你的团队的即可。 当为团队选择了测试框架之后， 接下来的目标是为生产的每一个新的功能／模
块编写测试。 如果你倾向于测试驱动开发(TDD)， 那就太棒了， 但是要点是确认你在上线任何功能或者重
构一个现有功能之前， 达到了需要的目标覆盖率。

### 一个测试一个概念

**不好的：**
```
    void testMakeMomentJSGreatAgain(){
            Date date;
            date = new MakeMomentJSGreatAgain("1/1/2015");
            date.addDays(30);
            Assert.equal(date.getString(),"1/31/2015").

            date = new MakeMomentJSGreatAgain("2/1/2016");
            date.addDays(28);
            Assert.equal(date.getString(),"02/29/2016");

            date = new MakeMomentJSGreatAgain("2/1/2015");
            date.addDays(28);
            Assert.equal(date.getString(),"03/01/2015");
        }
```

**好的：**
```
    void testThirtyDayMonths(){
            Date date = new MakeMomentJSGreatAgain("1/1/2015");
            date.addDays(30);
            Assert.equal(date.getString(),"1/31/2015");
        }

    void testLeapYear(){
            Date date = new MakeMomentJSGreatAgain("2/1/2016");
            date.addDays(28);
            Assert.equal(date.getString(),"02/29/2016");
        }

    void testNonLeapYear(){
            Date date = new MakeMomentJSGreatAgain("2/1/2015");
            date.addDays(28);
            Assert.equal(date.getString(),"03/01/2015");
        }
```
**[⬆ 返回顶部](#代码整洁的-Java)**


## **错误处理**

抛出错误是一件好事情！ 他们意味着当你的程序有错时运行时可以成功确认， 并且通过停止执行当前堆栈
上的函数来让你知道， 结束当前进程（在 Node 中）， 在控制台中用一个堆栈跟踪提示你。

### 不要忽略捕捉到的错误

对捕捉到的错误不做任何处理不能给你修复错误或者响应错误的能力。 向控制台记录错误 (`console.log`)
也不怎么好， 因为往往会丢失在海量的控制台输出中。 如果你把任意一段代码用 `try/catch` 包装那就
意味着你想到这里可能会错， 因此你应该有个修复计划， 或者当错误发生时有一个代码路径。

**不好的：**
```
  try {
        functionThatMightThrow();
    } catch (Exception error) {
        console.log(error);
    }
```

**好的：**
```
 try {
        functionThatMightThrow();
    } catch (Exception error) {
        // One option (more noisy than console.log):
        console.error(error);
        // Another option:
        notifyUserOfError(error);
        // Another option:
        reportErrorToService(error);
        // OR do all three!
    }
```


## **格式化**

格式化是主观的。 就像其它规则一样， 没有必须让你遵守的硬性规则。 重点是不要因为格式去争论， 这
里有[大量的工具](http://standardjs.com/rules.html)来自动格式化， 使用其中的一个即可！ 因
为做为工程师去争论格式化就是在浪费时间和金钱。

针对自动格式化工具不能涵盖的问题（缩进、 制表符还是空格、 双引号还是单引号等）， 这里有一些指南。

### 使用一致的大小写

JavaScript 是无类型的， 所以大小写告诉你关于你的变量、 函数等的很多事情。 这些规则是主观的，
所以你的团队可以选择他们想要的。 重点是， 不管你们选择了什么， 要保持一致。

**不好的：**
```
    int DAYS_IN_WEEK = 7;
    int  daysInMonth = 30;

    String[] songs = {"Back In Black", "Stairway to Heaven", "Hey Jude"};
    String[] Artists = {"ACDC", "Led Zeppelin", "The Beatles"};

    void eraseDatabase() {}
    void restore_database() {}

    class animal {}
    class Alpaca {}
```

**好的：**
```
    int DAYS_IN_WEEK = 7;
    int DAYS_IN_MONTH = 30;

    String[] songs = {"Back In Black", "Stairway to Heaven", "Hey Jude"};
    String[] artists = {"ACDC", "Led Zeppelin", "The Beatles"};

    void eraseDatabase() {}
    void restoreDatabase() {}

    class Animal {}
    class Alpaca {}
```
**[⬆ 返回顶部](#代码整洁的-Java)**


### 函数的调用方与被调用方应该靠近

如果一个函数调用另一个， 则在代码中这两个函数的竖直位置应该靠近。 理想情况下，保持被调用函数在被
调用函数的正上方。 我们倾向于从上到下阅读代码， 就像读一章报纸。 由于这个原因， 保持你的代码可
以按照这种方式阅读。

**不好的：**
```
 class PerformanceReview {
        String employee;
        public PerformanceReview(String employee) {
            this.employee = employee;
        }

        String lookupPeers() {
            return db.lookup(this.employee, "peers");
        }

        String lookupManager() {
            return db.lookup(this.employee, "manager");
        }

        void getPeerReviews() {
            String  peers = this.lookupPeers();
            // ...
        }

        public void perfReview() {
            this.getPeerReviews();
            this.getManagerReview();
            this.getSelfReview();
        }

        void getManagerReview() {
            String  manager = this.lookupManager();
        }

        void getSelfReview() {
            // ...
        }
    }
    
    PerformanceReview  review = new PerformanceReview("user");
    review.perfReview();
```

**好的：**
```
class PerformanceReview {
        String employee;
        public PerformanceReview(String employee) {
            this.employee = employee;
        }

        void perfReview() {
            this.getPeerReviews();
            this.getManagerReview();
            this.getSelfReview();
        }

        void getPeerReviews() {
            String  peers = this.lookupPeers();
            // ...
        }

        String lookupPeers() {
            return db.lookup(this.employee, "peers");
        }

        void getManagerReview() {
            String  manager = this.lookupManager();
        }

        String lookupManager() {
            return db.lookup(this.employee, "manager");
        }

        public void getSelfReview() {
            // ...
        }
    }
    
    PerformanceReview review = new PerformanceReview("user");
    review.perfReview();
```

**[⬆ 返回顶部](#代码整洁的-Java)**

## **注释**

### 仅仅对包含复杂业务逻辑的东西进行注释

注释是代码的辩解， 不是要求。 多数情况下， 好的代码就是文档。

**不好的：**
```
void hashIt(String data) {
        // The hash
        long hash = 0;

        // Length of string
        int length = data.length();

        // Loop through every character in data
        for (int i = 0; i < length; i++) {
            // Get character code.
            char mChar = data.charAt(i);
            // Make the hash
            hash = ((hash << 5) - hash) + mChar;
            // Convert to 32-bit integer
            hash &= hash;
        }
    }
```

**好的：**
```
 void hashIt(String data) {
        long hash = 0;
        int length = data.length();

        for (int i = 0; i < length; i++) {
            char mchar = data.charAt(i);
            hash = ((hash << 5) - hash) + mchar;

            // Convert to 32-bit integer
            hash &= hash;
        }
    }


```
**[⬆ 返回顶部](#代码整洁的-Java)**

### 不要在代码库中保存注释掉的代码

因为有版本控制， 把旧的代码留在历史记录即可。

**不好的：**
```
    doStuff();
    // doOtherStuff();
    // doSomeMoreStuff();
    // doSoMuchStuff();
```

**好的：**
```
    doStuff();
```
**[⬆ 返回顶部](#代码整洁的-Java)**

### 不要有日志式的注释

记住， 使用版本控制！ 不需要僵尸代码， 注释掉的代码， 尤其是日志式的注释。 使用 `git log` 来
获取历史记录。

**不好的：**
```
    /**
     * 2016-12-20: Removed monads, didn't understand them (RM)
     * 2016-10-01: Improved using special monads (JP)
     * 2016-02-03: Removed type-checking (LI)
     * 2015-03-14: Added combine with type-checking (JR)
     */
     
    void combine(String a, String b) {
        return a + b;
    }
```

**好的：**
```
    void combine(String a, String b) {
            return a + b;
    }
```
**[⬆ 返回顶部](#代码整洁的-Java)**

### 避免占位符

它们仅仅添加了干扰。 让函数和变量名称与合适的缩进和格式化为你的代码提供视觉结构。

**不好的：**
```
    ////////////////////////////////////////////////////////////////////////////////
    // Scope Model Instantiation
    ////////////////////////////////////////////////////////////////////////////////
    String[] model = {"foo","bar"};

    ////////////////////////////////////////////////////////////////////////////////
    // Action setup
    ////////////////////////////////////////////////////////////////////////////////
    void action(){
        //...
    }

```

**好的：**
```
  String[] model = {"foo","bar"};
        void action(){
            //...
        }
```
 
**[⬆ 返回顶部](#代码整洁的-Java)**
--

## **工具**

### [Lint](https://developer.android.google.cn/studio/write/lint)

Android Studio 提供了一个名为 Lint 的代码扫描工具，可帮助您发现并更正代码结构质量的问题，而无需您实际执行应用，也不必编写测试用例。系统会报告该工具检测到的每个问题并提供问题的描述消息和严重级别，以便您可以快速确定需要优先进行的关键改进。此外，您还可以降低问题的严重级别以忽略与项目无关的问题，或者提高严重级别以突出特定问题。

Lint 工具可以检查您的 Android 项目源文件是否有潜在的错误，以及在正确性、安全性、性能、易用性、无障碍性和国际化方面是否需要优化改进。

![](https://user-gold-cdn.xitu.io/2020/3/4/170a34230f1219ec?w=604&h=287&f=png&s=29918)

Lint不仅支持Java语言，同是支持Kotlin、C/C++等的规范性检查，Android官方推荐的代码扫描工具。

### [CheckStyle](https://checkstyle.org/)

CheckStyle作为检验代码规范的插件，除了可以使用配置默认给定的开发规范，如Sun的，Google的开发规范啊，也可以导入像阿里的开发规范的插件。事实上，每一个公司都存在不同的开发规范要求，所以大部分公司会给定自己的check规范，一般导入给定的checkstyle.xml文件即可实现。CheckStyle来辅助判断代码格式是否满足规范，保证团队成员开发的code风格一致。
 
CheckStyle检验的主要内容
- Javadoc注释
- 命名约定
- 标题
- Import语句
- 体积大小
- 空白
- 修饰符
- 块
- 代码问题
- 类设计
- 混合检查（包括一些有用的比如非必须的System.out和printstackTrace）

从上面可以看出，CheckStyle提供了大部分功能都是对于代码规范的检查。但CheckStyle目前只支持检查Java语言。


![](https://user-gold-cdn.xitu.io/2020/3/4/170a3a215261376c?w=1510&h=778&f=png&s=223360)

### [SonarLint](https://www.sonarlint.org/)

SonarLint是一个IDE扩展，可帮助您在编写代码时检测和修复质量问题，在提交代码之前进行修复。

- 错误检测

受益于已有的数千条规则 ; 可以检测常见错误，棘手错误和已知漏洞

- 即时反馈

就像拼写检查器一样，在编码时会检测到并报告问题

- 丰富的文档

精确地指出了问题所在，并为您提供了解决方法的建议
 
![](https://user-gold-cdn.xitu.io/2020/3/4/170a47f863d2a8ef?w=2746&h=1306&f=png&s=389802)

### [Alibaba Java Coding Guidelines](https://github.com/alibaba/p3c)

阿里巴巴于10月14号在杭州云栖大会上，正式发布《阿里巴巴Java开发规约》的扫描插件。该插件在扫描代码后，将不符合规约的代码按Blocker/Critical/Major三个等级显示在下方，甚至在IDEA上，该插件还基于Inspection机制提供了实时检测功能，编写代码的同时也能快速发现问题所在。对于历史代码，部分规则实现了批量一键修复的功能。

- Blocker

即系统无法执行、崩溃或严重资源不足、应用模块无法启动或异常退出、无法测试、造成系统不稳定。
严重花屏、内存泄漏、用户数据丢失或破坏、系统崩溃/死机/冻结、模块无法启动或异常退出、严重的数值计算错误、功能设计与需求严重不符、其它导致无法测试的错误, 如服务器500错误

- Critical

即影响系统功能或操作，主要功能存在严重缺陷，但不会影响到系统稳定性。
功能未实现、功能错误、系统刷新错误、数据通讯错误、轻微的数值计算错误、影响功能及界面的错误字或拼写错误、安全性问题

- Major

即界面、性能缺陷、兼容性。操作界面错误（包括数据窗口内列名定义、含义是否一致）、边界条件下错误、提示信息错误（包括未给出信息、信息提示错误等）、长时间操作无进度提示、系统未优化（性能问题）、光标跳转设置不好，鼠标（光标）定位错误、兼容性问题

![](https://user-gold-cdn.xitu.io/2020/3/4/170a39d986563f1b?w=635&h=308&f=png&s=13685)

**不同的代码检查工具原理相同，但侧重点不一样，在实际的项目开发中，可以根据自己的团队及项目情况进行选择及组合，定义自己的团队规范。**

**[⬆ 返回顶部](#代码整洁的-Java)**
--
