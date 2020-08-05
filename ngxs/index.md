# [Ngxs](https://www.ngxs.io/)

- [Ngxs](#ngxs)
  - [介绍](#介绍)
  - [主要概念](#主要概念)
  - [Store](#store)
    - [创建`action`](#创建action)
    - [调度`action`](#调度action)
    - [快照 `Snapshots`](#快照-snapshots)
  - [动作 `Action`](#动作-action)
  - [参考](#参考)

## 介绍

Ngxs是Angluar的状态管理模式 + 库。作为应用程序状态的唯一真实来源，为可预测的状态突变提供简单的规则

## 主要概念

- Store: 全局状态容器、操作调度器和选择器
- Actions: 该类描述要执行的操作及其关联的元数据
- State: 该类定义状态
- Selects: 状态切片选择器

> 这些概念创建了一个循环的控制流，从分派动作的组件到对动作作出反应的存储器，再到通过状态选择回到组件

![diagram](images/diagram.png)

---

## Store

> `Store`是一个全局状态管理器，它分派状态容器侦听的操作，并提供从全局状态中选择数据切片的方法

### 创建`action`

例如：
```typescript
export class AddAnimal {
  static readonly type = '[Zoo] Add Animal';
  constructor(public name: string) {}
}
```

### 调度`action`

要调度 `action`，你需要注入`Store`服务到你的组件/服务中，并且用 `dispatch` 方法触发一个`action`或者一组`action`
```typescript
import { Store } from '@ngxs/store';
import { AddAnimal } from './animal.actions';

@Component({ ... })
export class ZooComponent {
  constructor(private store: Store) {}

  addAnimal(name: string) {
    this.store.dispatch(new AddAnimal(name));
  }
}
```

调用一组`action`
```typescript
this.store.dispatch([new AddAnimal('Panda'), new AddAnimal('Zebra')]);
```

在执行`action`后，如果需要重置表单。我们的`dispatch`方法实际上返回的是一个`Observable`，所以，我们可以在成功后订阅它并且重置表单
```typescript
import { Store } from '@ngxs/store';
import { AddAnimal } from './animal.actions';

@Component({ ... })
export class ZooComponent {

  constructor(private store: Store) {}

  addAnimal(name: string) {
    this.store.dispatch(new AddAnimal(name)).subscribe(() => this.form.reset());
  }

}
```

`dispatch`方法返回的`Observable`是`void`类型，这是因为可以有多个`state`监听同一个`@Action`，因此，从这些`actions`中返回`state`是不现实的，因为我们不知道它们的具体形式。

如果你在之后需要得到这些`state`，只需使用一个`@Select`
```typescript
import { Store, Select } from '@ngxs/store';
import { Observable } from 'rxjs';
import { withLatestFrom } from 'rxjs/operators';
import { AddAnimal } from './animal.actions';

@Component({ ... })
export class ZooComponent {

  @Select(state => state.animals) animals$: Observable<any>;

  constructor(private store: Store) {}

  addAnimal(name: string) {
    this.store
      .dispatch(new AddAnimal(name))
      .pipe(withLatestFrom(this.animals$))
      .subscribe(([_, animals]) => {
        // do something with animals
        this.form.reset();
      });
  }

}
```

---

### 快照 `Snapshots`

> 你可以通过调用`Store.snapshot()`来获取该时间点存储的所有值

---

## 动作 `Action`

> `Action`是可以被触发的命令（疑惑）

## 参考

- [Ngxs](https://www.ngxs.io)
- [徐海峰: Angular 真的需要状态管理么？](https://zhuanlan.zhihu.com/p/45121775)
