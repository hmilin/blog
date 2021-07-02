
#### Github Stars

akita: 3.1k
ngxs: 3.1k
ngrx: 6.8k


#### Akita

#### ngxs



#### ngrx

来自官方的介绍

- Managing global and local state.
- Isolation of side effects to promote a cleaner component architecture.
- Entity collection management.
- Integration with the Angular Router.
- Developer tooling that enhances developer experience when building many different types of applications.

概念
actions
代表一个应用中唯一发生的事件

```
interface Action {
	type: string;  // [Source] description
}
// 举个栗子
{
	type: '[Auth API] Login Success'
}
{
  type: '[Login Page] Login',
  username: string;
  password: string;
}
```

actions用法

定义
```
import { createAction, props } from '@ngrx/store';

export const login = createAction(
  '[Login Page] Login',
  props<{ username: string; password: string }>()
);
```

调用

```
onSubmit(username: string, password: string) {
  store.dispatch(login({ username: username, password: password }));
}
```

reducers

根据action的类型通过一个纯函数来更新state

用法

```
const scoreboardReducer = createReducer(
  initialState,
  on(ScoreboardPageActions.homeScore, state => ({ ...state, home: state.home + 1 })),
  on(ScoreboardPageActions.awayScore, state => ({ ...state, away: state.away + 1 })),
  on(ScoreboardPageActions.resetScore, state => ({ home: 0, away: 0 })),
  on(ScoreboardPageActions.setScores, (state, { game }) => ({ home: game.home, away: game.away }))
);

export function reducer(state: State | undefined, action: Action) {
  return scoreboardReducer(state, action);
}
```

selectors

selectors是用来获取当前状态的一个纯函数
ngrx提供了 createSelector 和 createFeatureSelector 来创建selector


##### 工作流

1. 定义state
2. 编写aciton，定义一个数据可以有哪些操作
3. 编写reducer，使用createReducer方法，定义哪些action的触发需要对state做哪些修改，on的第二个参数必须为纯函数
4. 在module文件里的StoreModule.forRoot方法传入新定义的state与之对应的reducer
5. 利用selector来获取当前state
6. 利用dispatch方法触发action

