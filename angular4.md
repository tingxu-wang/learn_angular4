title: amgular4 讲解
speaker: tingxu.wang
transition: rollIn

[slide]
# angular4 讲解
## 作者：王庭旭

[slide]
# 讲解纲要
- 浏览器支持情况
- 开发环境简介
  - 目录结构
- TypeScript简介
  - 与JavaScript的关系
  - 常用语法简介
- 语法讲解
  - 数据绑定语法
    - 单向绑定
    - 双向绑定
  - controller
  - directives(component)
  - service
  - filter
  - 绑定class到module
- 我对新版angular的学习感受

[slide]
- 浏览器支持情况：

![浏览器支持情况](/images/brower-support.jpeg)

不像angular1支持IE8，新版本的angular放弃了它

[slide]
- 开发环境简介

- 工具：angular-cli
- 常用命令解释
  - ```
      $ ng new my-app //构建项目脚手架
    ```
    构建后的项目结构：
    ```
        .
    |-- app/
    |   |-- app.component.css
    |   |-- app.component.html
    |   |-- app.component.spec.ts
    |   |-- app.component.ts
    |   |-- app.module.ts
    |   |-- index.ts
    |   `-- shared/
    |       `-- index.ts
    |-- assets/        //放置静态资源
    |-- environments/  //环境配置
    |   |-- environment.dev.ts
    |   |-- environment.prod.ts
    |   `-- environment.ts
    |-- favicon.ico
    |-- index.html     //根html文件
    |-- main.ts        //指明整个应用主入口的配置文件，ng serve跑服务的依据
    |-- polyfills.ts   //语法兼容polyfill
    |-- styles.css     //全局样式文件，因为在新版angular中，所有的组件都拥有私有的样式文件，之后我会详细说明
    |-- test.ts        //单元测试用的，官网说的有点笼统，没细研究
    |-- tsconfig.json  //类似于webpack.config.js
    `-- typings.d.ts
    ```
  - ```
      $ ng serve //启动本地开发环境
    ```

[slide]
- TypeScript简介

  ![typescript](/images/typescript.png)

  简单说来es6是typescript的超集，在支持ES6的基础之上加入了一些新的特性，我认为比较重要的有：
  - 类型 *
  - 类
  - 注解 *
  - 模块导入
  - 语言工具包（解构等）

[slide]
- TypeScript简介

  1. 变量声明
  ``` typescript
    // 同时声明数据类型

    //---- 基本类型
    var name: string = 'JinDi';
    var age: number = 666;
    var isNiuBi: boolean = true;
    //不做限制
    var something: any = 'as string';
    something = 1;

    //---- 引用类型
    var names: Array<string> = ['yansu', 'baiqing', 'renliang', 'bike', 'tingxu'];//在声明数据类型的同时可以声明内容的数据类型
    var names: string[] = ['yansu', 'baiqing', 'renliang', 'bike', 'tingxu'];//简写

    function setName(name: string): void {//不期望返回值
      this.name = name;
    }

    class Person {
      first_name: string;
      last_name: string;
      age: number;

      constructor(first_name: string, last_name: string, age: number) {//与普通的ES6写法很像
        this.first_name = first_name;
        this.last_name = last_name;
        this.age = age;
      }

      greet() {
         console.log("Hello", this.first_name);
      }
    }
  ```

[slide]
- 语法讲解

- 数据绑定语法
- 单向绑定

  1. model->dom
  ``` html
  <div>用户姓名：{{ name }}</div>
  <app-show-user [name]="name"></app-show-user>
  ```
  2. model<-dom
  ``` html
  <button (click)="postForm()">点击提交</button>
  ```

- 双向绑定
  ``` html
  您的姓名：<input type="text" [(ngModel)]="userName">
  ```

[slide]
- controller
  被砍掉了，整个都放在了component之中，新版本在利用注解的时候就已经指明了模块暴露的class是什么类型的angular类了

  - 旧版写法

  ```JavaScript
  //节选自auth.js
  angular
    .module("app.controllers")
    .controller('authCtrl', authCtrl);

  function authCtrl($scope){
    //...
  }
  ```

  - 新版写法

  ```TypeScript
    @Component({
      selector: 'auth',
      templateUrl: './auth.component.html',
      styleUrls: [ './auth..component.css' ],
    })

    export class authComponent {
      //...

      constructor(){
        //...
      }
    }
  ```

[slide]
- directives

  1. 说明

    在新版本中指令特指在view中声明的各种angular语法譬如\*ngIf,\*ngFor等

  2. 新旧语法对比

  ```html
  <button ng-click="method()">click</button>
  <button (click)="method()">click</button>

  <input type="text" ng-model="name">
  <input type="text" [(ngModel)]="name">

  <div ng-if="names.length">
    if
  </div>
  <div *ngIf="names.length">
    if
  </div>

  <h3 ng-show="isShow">
    show
  </h3>
  <h3 [hidden]="!isShow">
    Your favorite hero is: {{favoriteHero}}
  </h3>
  ```

[slide]

``` html

<ul>
  <li ng-for="name in names">{{ name }}</li>
</ul>
<ul>
  <li *ngFor="let name of names">{{ name }}</li>
</ul>

<a ng-href="companyUrl">to company</a>
<a [href]="companyUrl">to company</a>

<div ng-class="{active: isActive}">

</div>
<div [ngClass]="{active: isActive}">

</div>

<div ng-style="{color: colorPreference}">
<div [ngStyle]="{color: colorPreference}"><!--多style-->
<div [style.color]="colorPreference"><!--单个style绑定-->
```

[slide]
- service

- 注册service

```TypeScript
import { Injectable, Type } from '@angular/core';

@Injectable()
export class TestService{
  splitStrToArr (str:string){
    return str.split('')
  }
}
```

[slide]
- 引用service

```TypeScript
import { Component, OnInit, Input } from '@angular/core';
import { TestService } from '../service/test.service';
@Component({
  selector: 'app-user-item',
  templateUrl: './user-item.component.html',
  styleUrls: ['./user-item.component.css']
})
export class UserItemComponent implements OnInit {
  //---- 声明变量数据类型
  names:string[];
  isShowCompany:boolean;
  splitedCompanyName:object;
  getSplitedCompanyName:object;

  //---- props
  @Input() companyName:string;

  //---- data
  constructor(private testSer:TestService) {//引入service
    this.names=['yansu', 'baiqing', 'renliang', 'bike', 'tingxu'];

    this.isShowCompany=false;
    this.getSplitedCompanyName=function(){
      return this.testSer.splitStrToArr(this.companyName);
    }

  }

  //---- methods
  toggleShow (){
    this.isShowCompany=!this.isShowCompany;
  }

  //---- ng-init
  ngOnInit() {
    this.companyName='北京金堤科技';
  }

}

```

[slide]
- 绑定class到module

``` TypeScript
@NgModule({
  declarations: [//注入组件
    AppComponent,
    HelloWorldComponent,
    UserItemComponent
  ],
  imports: [//模块依赖
    BrowserModule,
    FormsModule,
    HttpModule
  ],
  providers: [
    TestService
  ],
  bootstrap: [AppComponent]//模块渲染根组件
})
```

[slide]
- demo演示

[slide]
# 我对新版angular的学习感受

[slide]
# 谢谢大家！
