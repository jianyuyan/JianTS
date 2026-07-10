

# JianTS 语法规范 v1.5（公开版·修正）

> **一句话定义：** JianTS 是 TypeScript 的方言——用拼音关键词替换声明层，用 `[T]` 替换泛型尖括号，流程控制部分拼音化，语义 100% 兼容 TypeScript。
>
> **核心理念：** 让中文开发者用更少的「思维空转」写 TypeScript。

---

## 一、5 秒看懂 JianTS

```jian
// 这是 JianTS 代码
gn greet(name: string)string {
    fanhui `你好，${name}！`;
}

gn log(message: string) {    // 无返回值，不加 :void
    console.log(message);
}

gn 入口() {
    log("程序启动");
    chang users: string[] = ["张三", "李四"];
    zhuge (chang name of users) {
        log(greet(name));
    }
}
```

编译后就是标准 TypeScript：

```typescript
function greet(name: string)string {
    return `你好，${name}！`;
}

function log(message: string) {    // 无返回值，不加 :void
    console.log(message);
}

function main() {
    log("程序启动");
    const users: string[] = ["张三", "李四"];
    for (const name of users) {
        log(greet(name));
    }
}
main();
```

---

## 二、核心关键词映射表

### 2.1 声明层关键词（拼音替换）

| TypeScript | JianTS | 说明 |
| :--- | :--- | :--- |
| `function` | `gn` | 函数定义 |
| `let` / `var` | `shu` | 可变变量 |
| `const` | `chang` | 常量 |
| `class` | `sjgnbao` | 类定义 |
| `interface` | `gnlantu` | 接口定义 |
| `enum` | `xuanxiangdan` | 枚举定义 |
| `constructor` | `入口` | 构造函数 |
| `public` | `kaif` | 公开可见性 |
| `private` | `shiyou` | 私有可见性 |
| `protected` | `neibu` | 受保护可见性 |
| `export` | `引出` | 模块导出 |
| `import` | `引入` | 模块导入 |
| `from` | `自` | 引入来源 |
| `as` | `zuo` | 别名/类型断言 |
| `this` | `ben` | 当前实例 |
| `super` | `fuji` | 父类引用 |
| `async` | `后台` | 异步函数 |
| `await` | `等` | 等待异步 |
| `yield` | `产出` | 生成器产出 |
| `return` | `fanhui` | 函数返回 |
| `default`（switch 中） | `moren` | 默认分支 |
| `namespace` | `mok` | 命名空间 |
| `function*` | `生成器 gn` | 生成器函数 |

### 2.2 函数返回类型规则（重要！）

| 场景 | JianTS 写法 | 说明 |
| :--- | :--- | :--- |
| **无返回值** | `gn log(msg: string) { ... }` | 直接省略，不加 `:void` |
| **有返回值** | `gn add(a: number, b: number) number { ... }` | 返回类型直接跟在 `)` 后面，加 `:` |

### 2.3 流程控制（部分拼音化）

| TypeScript | JianTS | 说明 |
| :--- | :--- | :--- |
| `if` / `else` | `if` / `else` | ✅ 保持英文 |
| `switch` / `case` | `switch` / `case` | ✅ 保持英文 |
| `default` | `moren` | ⚠️ 拼音替换 |
| `for` | `zhuge` | ⚠️ 拼音替换（逐个） |
| `while` | `zhiyao` | ⚠️ 拼音替换（只要） |
| `do...while` | `do...zhiyao` | ⚠️ 拼音替换 |
| `break` | `break` | ✅ 保持英文 |
| `continue` | `continue` | ✅ 保持英文 |
| `try` / `catch` | `try` / `catch` | ✅ 保持英文 |

### 2.4 类型系统（完全保留英文）

```
string, number, boolean, bigint, symbol, object,
any, unknown, void, never, null, undefined,
type, extends, implements, readonly, static,
typeof, instanceof, keyof, infer, satisfies
```

### 2.5 保留不变的语法

以下内容**完全保持 TypeScript 原样**：

| 类别 | 示例 |
| :--- | :--- |
| 注释 | `// 注释` `/* 注释 */` |
| 字符串模板 | `` `Hello ${name}` `` |
| 箭头函数 | `(x) => x * x` |
| 解构赋值 | `{ a, b } = obj` |
| 展开运算符 | `...arr` |
| 可选链 | `obj?.prop` |
| 空值合并 | `x ?? y` |
| 装饰器 | `@decorator` |
| 布尔字面量 | `true` / `false` |
| 数字/字符串字面量 | `123` / `"hello"` |

---

## 三、泛型语法（核心差异）

**TypeScript 用 `<T>`，JianTS 用 `[T]`。**

### 3.1 泛型对照表

| 场景 | TypeScript | JianTS |
| :--- | :--- | :--- |
| 泛型函数 | `function f<T>(x: T)  T` | `gn f[T](x: T)  T` |
| 泛型箭头函数 | `<T>(x: T) => T` | `[T](x: T) => T` |
| 泛型接口 | `interface Box<T>` | `gnlantu Box[T]` |
| 泛型类 | `class Container<T>` | `sjgnbao Container[T]` |
| 泛型类型别名 | `type Result<T>` | `type Result[T]` |
| 多泛型参数 | `<T, U>` | `[T, U]` |
| 泛型约束 | `<T extends Clone>` | `[T extends Clone]` |
| 泛型默认值 | `<T = string>` | `[T = string]` |
| 泛型调用 | `identity<string>("x")` | `identity[ string ]("x")` |

### 3.2 泛型示例

```jian
// 泛型函数
gn identity[T](x: T) T {
    fanhui x;
}

// 泛型接口
gnlantu Box[T] {
    value: T;
    getValue()  T;
}

// 泛型类
sjgnbao Stack[T] {
    shiyou items: T[] = [];

    push(item: T) {
        ben.items.push(item);
    }

    pop()  T | undefined {
        fanhui ben.items.pop();
    }
}

// 调用泛型函数
chang num = identity[ number ](42);
chang str = identity[ string ]("hello");
```

### 3.3 泛型与数组的区分

```jian
// 泛型参数（出现在类型名后，紧跟 [类型名]）
gnlantu Box[T] { item: T }

// 数组类型（出现在类型注解位置，类型后跟 []）
chang items: string[] = ["a", "b"];

// 泛型实例化
chang box: Box[ number ] = { item: 42 };
```

---

## 四、入口函数

```jian
// 同步入口
gn 入口() {
    console.log("Hello, JianTS!");
}
```

```jian
// 异步入口
后台 gn 入口() {
    chang data = 等 fetchData();
    console.log(data);
}
```

编译为 TypeScript：

```typescript
async function main() {
    const data = await fetchData();
    console.log(data);
}
main();
```

---

## 五、变量与常量

```jian
shu count = 0;              // 可变变量
chang MAX = 100;            // 不可变常量
shu name: string = "张三";   // 带类型注解
chang users: string[] = ["张三", "李四"];

shu kegai counter = 0;      // 可变绑定（类似 let）
counter++;
```

---

## 六、函数

```jian
// 有返回值：返回类型直接跟在 ) 后面，不加 :
gn add(a: number, b: number) number {
    fanhui a + b;
}

// 无返回值：直接省略，不加 :void
gn log(message: string) {
    console.log(message);
}

// 箭头函数（箭头符号保持不变）
chang square = (x: number)  number => x * x;

// 异步函数
后台 gn fetchData(url: string)  Promise[string] {
    chang response = 等 fetch(url);
    fanhui response.text();
}

// 生成器函数
生成器 gn counter(start: number, end: number) {
    zhiyao (start <= end) {
        产出 start;
        start++;
    }
}

// 参数特性（可选、默认、剩余）
gn greet(name: string, suffix?: string)string {
    fanhui `Hello, ${name}${suffix || ""}`;
}

gn createUser(name: string, age: number = 18)  User {
    fanhui { name, age };
}

gn sum(...numbers: number[]) number {
    fanhui numbers.reduce((a, b) => a + b, 0);
}
```

### 6.1 函数返回类型速查

| 场景 | JianTS | 对应 TypeScript |
| :--- | :--- | :--- |
| 无返回值 | `gn log(msg: string) { ... }` | `function log(msg: string) { ... }` |
| 返回 `string` | `gn getName()string { ... }` | `function getName()string { ... }` |
| 返回 `Promise` | `gn fetch()  Promise[Data] { ... }` | `function fetch()  Promise<Data> { ... }` |
| 返回 `void` | **不写，直接省略** | `function f()  void { ... }` |

---

## 七、类型系统

### 7.1 接口（gnlantu）

```jian
gnlantu User {
    readonly id: number;
    name: string;
    age: number;
    email?: string;
}

gnlantu Admin extends User {
    permissions: string[];
}
```

### 7.2 类（sjgnbao）

```jian
abstract sjgnbao Animal {
    入口(kaif name: string) {}

    abstract makeSound();

    kaif move() {
        console.log(`${ben.name} is moving`);
    }
}

sjgnbao Dog extends Animal {
    入口(name: string, shiyou breed: string) {
        fuji(name);
    }

    kaif makeSound() {
        console.log("Woof!");
    }

    kaif getBreed()string {
        fanhui ben.breed;
    }
}
```

### 7.3 类型别名（type 保持英文）

```jian
type UserID = string | number;
type UserList = User[];
type Callback = (data: string) => void;
type Result[T] = T | Error;
```

### 7.4 枚举（xuanxiangdan）

```jian
xuanxiangdan Status {
    Active,
    Inactive,
    Pending,
}

xuanxiangdan ResponseStatus {
    Success = 200,
    NotFound = 404,
    Error = 500,
}
```

---

## 八、控制流

```jian
// if / else（保持英文）
if (age >= 18) {
    console.log("Adult");
} else if (age >= 13) {
    console.log("Teenager");
} else {
    console.log("Child");
}

// switch / case / moren（moren 拼音替换）
switch (status) {
    case "success":
        console.log("Success");
        break;
    case "failed":
        console.log("Failed");
        break;
    moren:
        console.log("Unknown");
}

// zhuge 循环（逐个）
zhuge (let i = 0; i < 10; i++) {
    console.log(i);
}

zhuge (chang item of items) {
    console.log(item);
}

zhuge (chang key in obj) {
    console.log(key, obj[key]);
}

// zhiyao 循环（只要）
zhiyao (count > 0) {
    console.log(count);
    count--;
}

do {
    console.log("At least once");
} zhiyao (condition);

// break / continue（保持英文）
zhuge (let i = 0; i < 10; i++) {
    if (i === 5) continue;
    if (i === 8) break;
    console.log(i);
}
```

---

## 九、模块系统

```jian
// 引出
引出 gnlantu User { name: string }
引出 chang API_URL = "https://api.example.com";
引出 moren sjgnbao App {}

// 引入
自 "./types" 引入 { User, API_URL };
自 "./App" 引入 App;
自 "./types" 引入 type { Response };
自 "./utils" 引入 * zuo utils;

// 重命名
自 "./types" 引入 { User zuo UserType };

// 类型引出
引出 type { User, Response };
```

---

## 十、异步编程

```jian
// 异步函数
后台 gn fetchUser(id: number)  Promise[User] {
    chang response = 等 fetch(`/api/user/${id}`);
    fanhui 等 response.json();
}

// 使用
后台 gn main() {
    try {
        chang user = 等 fetchUser(123);
        console.log(user);
    } catch (error) {
        console.error("Error:", error);
    }
}

// Promise 静态方法
chang results = 等 Promise.all([task1, task2]);
```

---

## 十一、完整示例

```jian
// 类型定义
gnlantu User {
    readonly id: number;
    name: string;
    age: number;
    email?: string;
}

type UserID = number | string;

xuanxiangdan Status {
    Active,
    Inactive,
    Pending,
}

// 泛型容器
sjgnbao Container[T] {
    shiyou items: T[] = [];

    add(item: T) {
        ben.items.push(item);
    }

    get(index: number) T | undefined {
        fanhui ben.items[index];
    }
}

// 服务类
sjgnbao UserService {
    shiyou static instance: UserService | null = null;

    shiyou 入口() {}

    kaif static getInstance()  UserService {
        if (!UserService.instance) {
            UserService.instance = new UserService();
        }
        fanhui UserService.instance;
    }

    后台 gn fetchUser(id: UserID)  Promise[User] {
        chang response = 等 fetch(`/api/users/${id}`);
        fanhui 等 response.json();
    }
}

// 工具函数
gn delay(ms: number)  Promise[void] {
    fanhui new Promise(resolve => setTimeout(resolve, ms));
}

// 入口函数
后台 gn 入口() {
    chang service = UserService.getInstance();
    chang user = 等 service.fetchUser(123);
    console.log("User:", user);
}
```

---

## 十二、与 TypeScript 对照速查表

| TypeScript | JianTS |
| :--- | :--- |
| `function f()  T` | `gn f()  T` |
| `function f()`（无返回值） | `gn f()` |
| `let x =` | `shu x =` |
| `const x =` | `chang x =` |
| `class X` | `sjgnbao X` |
| `interface X` | `gnlantu X` |
| `enum X` | `xuanxiangdan X` |
| `constructor()` | `入口()` |
| `public` / `private` | `kaif` / `shiyou` |
| `export` / `import` | `引出` / `引入` |
| `from` | `自` |
| `as` | `zuo` |
| `this` / `super` | `ben` / `fuji` |
| `async` / `await` | `后台` / `等` |
| `return` | `fanhui` |
| `for` | `zhuge` |
| `while` | `zhiyao` |
| `default`（switch） | `moren` |
| `<T>` | `[T]` |

**其余所有语法（if/else/switch/case/try/catch/break/continue/箭头函数/解构/展开/类型注解等）完全保持不变。**

---

## 十三、工具链

| 工具 | 说明 |
| :--- | :--- |
| **编译器** | `jiantsc` → 编译为标准 TypeScript |
| **直接执行** | `jiants-node` 直接运行 `.jts` 文件 |
| **VSCode 插件** | 语法高亮 + 自动补全 |
| **构建工具** | 支持 Vite/Webpack/esbuild 插件 |

---

## 十四、许可证

**JianTS 语法规范**采用 **Jian 社区许可协议（JCL）**：

- ✅ 个人学习、开源非商业项目：**免费使用**
- ✅ 商业内部使用（员工 ≤ 50 人）：**免费使用**
- ⚠️ 商业发行版语言或云平台服务：**须购买商业授权**
- ❌ 禁止将本规范用于闭源商业竞品开发

详细协议条款请参阅 [LICENSE](./LICENSE) 文件。

---

## 十五、获取帮助

- 📖 [完整教程](https://blog.csdn.net/2501_92449685)
- 💻 [在线 IDE](http://jianpy.yuelonghui.cn/)
- 💬 [GitHub Discussions](https://github.com/jian-lang/jianpy/discussions)
- 🐛 [Issue 反馈](https://blog.csdn.net/2501_92449685)

---

**Happy Coding! 🚀**

---
 