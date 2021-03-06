---
title: rxjs学习记录
categories:
  - Angular
---

### 基础概念

- Observable (可观察对象): 表示一个概念，这个概念是一个可调用的未来值或事件的集合。
- Observer (观察者): 一个回调函数的集合，它知道如何去监听由 Observable 提供的值。
- Subscription (订阅): 表示 Observable 的执行，主要用于取消 Observable 的执行。
- Operators (操作符): 采用函数式编程风格的纯函数 (pure function)，使用像 map、filter、concat、flatMap 等这样的操作符来处理集合。
- Subject (主体): 相当于 EventEmitter，并且是将值或事件多路推送给多个 Observer 的唯一方式。
- Schedulers (调度器): 用来控制并发并且是中央集权的调度员，允许我们在发生计算时进行协调，例如 setTimeout 或 requestAnimationFrame 或其他。


### 常用操作符

- of: 返回一个observable，依次发出传入的值
- switchMap: 会取消上次发出的observable，通常和interval配合使用
- mergeMap/flatMap: 与map的区别是能将返回结果打平
- merge: 将多个Observable合并为一个
- mergeAll: 收集并订阅所有Observable,map + mergeAll === mergeMap

### 关于取消订阅

#### 结束observable的方式

1. Observable发送完数据之后执行Observable.onComplete()
2. Observable发生错误，执行Observable.OnError()
3. 订阅者主动取消，subscription.unsubscribe()

#### angular中常见的Observable哪些需要取消订阅

- 调用httpClient方法返回的Observable不需要取消订阅，因为发送数据之后，angular会调用Observable.onComplete()
- 使用AsyncPipe不需要取消订阅
- 通过 Subject，BehaviorSubject，AsyncSubject，ReplaySubject 在各个 Component 之间通信，需要调用 unsubscribe()取消订阅
- 使用rxjs自带的操作符不需要取消订阅

### 应用实例

#### 处理多个http请求

遇到需要调用多个接口返回数据并统一处理的时候，可以使用forkJoin，类似Promise.all()

```
  GetDetails(projectId: string) {
    const projects = this._http.get<any>(`/api/factory/projects/${projectId}`);
    const powers = this._http.get<any>(`/api/factory/projectpermissions/projects/${projectId}/userpermissions`);
    return forkJoin([projects, powers]).pipe(
      map(values => {
        const project = values[0];
        const power = values[1];
        return { ...project, power };
      })
    );
  }
```

#### 事件节流

#### subscribe嵌套问题



#### BehaviorSubject、ReplaySubject和AsyncSubject区别

##### BehaviorSubject

可以设置初始值，并且订阅的时候会马上发送存储的最后一个值给订阅者

##### ReplaySubject

可以实现重播，new ReplaySubject(存储几次广播，过期时间)

##### AsyncSubject

一样可以存储数据，但是只会在Observable结束时发送数据