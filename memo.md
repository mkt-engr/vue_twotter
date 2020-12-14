以下のコマンドでダッシュボードが立ち上がる。

```
vue ui
```

alt + shift + F でコードが成形されるときに Vetur か Prettier のどちらかを選ぶか聞かれて Prettier を選んだ。

# Chapter3:Building Single File Components

```js
export default {
  name: "HelloWorld",
  props: {
    msg: {
      type: String,
      required: false,
    },
  },
};
```

# Chapter4:Template Conditionals,Loops,Events

`props`は親から受け取る値。

- TwootItem.js

```js
<script>
  export default {
    name: "TwootItem",
    props: {
      username: {
        type: String,
        required: true,
      },
      twoot: {
        type: Object,
        required: true,
      },
    },
  };
</script>
```

`data()`の部分と props として子供に渡す。v-bind で変数として渡せる

- UserProfile.js

```js
<template>
  <TwootItem
      v-for="twoot in user.twoots"
      :key="twoot.id"
      :username="user.username"
      :twoot="twoot"
    />
</template>

import TwootItem from "./TwootItem";
components:{TwootItem},
data(){
  return{
    user: {
      id:1,
      twoots:[
        {id:1,content:"aaa"},{id:2,content:"bbb"}
      ]
    }
  }
}
```

## \$emit (子の値を親に渡す)

- TwootItem.vue(子のコンポーネント)

  - `@click="＜そのファイル内で定義したメソッド(methodsに書く)＞"`

  - `this.$emit`で親に値を渡す
    - `this.$emit("＜カスタムイベントの名前＞",＜親に渡したい変数＞)`
      - イベント＝@click とか@hover とかのこと。それのカスタムバージョン
    * \$emit の第 2 引数以降は増やせる

  ```js
  <template>
    <div class="twoot-item" @click="favouriteTwoot(twoot.id)">
    ~~~
  </template>
  <script>
  methods:{
    favouriteTwoot(id){
      this.$emit("favourite",id) //親で受け取る名前,親に渡したい変数
    }
  }
  </script>
  ```

- UserProfile.vue(親のコンポーネント)
  - methods の引数の id は親コンポーネントの\$emit の第 2 引数
  ```js
  <template>
    <TwootItem
      v-for="twoot in user.twoots"
      ~~~
      @favourite="toggleFavourite"
      //@<$emitの第1引数>="このファイルで定義したメソッドの名前"
    />
  </template>
  <script>
    methods: {
        toggleFavourite(id) {
          console.log(`Favourited Twoot #${id}`);
        },
    }
  </script>
  ```

# Chapter5 Forms & Handing User Input

`v-modal`とか打つと`ypeError: Cannot read property 'created' of undefined`みたいなエラーが出る

## v-model

双方向 binding

v-model で入れた変数は双方向に binding される。
textarea に入力すると newTwootContent も変更される

```js
<template>
  <textarea
    id="newTwoot"
    v-model="newTwootContent"
  ></textarea>
</template>
<script>
  data() {
    return {
      newTwootContent: "",
    }
  }
</script>
```

## @submit.prevent

HTML の submit は何も設定をしないとページ遷移する。それを止める。止めた時に何をするかを右辺に書く

ネイティブの JS の preventDefault メソッドと同じ

## vue のイベント修飾子

参考：https://techblog.roxx.co.jp/entry/2019/02/08/12291：

```
.stop = stopPropagation
```

# Chapter 6 Dynamic Styling,SCSS,Global Styling

SCSS を使うためにまず以下を実行。Vue UI から GUI でもできる。

```
npm i node-sass sass-loader
```

- vue-config.js
  グローバルに SCSS をインポートする。

  参考：https://cli.vuejs.org/guide/css.html#passing-options-to-pre-processor-loaders

  `@/`は`src/`のアライアス

  ```
  module.exports = {
    css: {
      loaderOptions: {
        sass: {
          additionalData: "@import '@/styles/base.scss';",
        },
      },
    },
  };
  ```

# Chapter7 The Vue3 Composition API
