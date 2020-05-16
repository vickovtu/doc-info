# Шаг 1. Установка Vuex
```
npm install --save vuex
```

# Шаг 2. Создание хранилища Vuex
>Создайте директорию store в корне нашего приложения.

>Создайте файл index.js в этой директории и используйте код, представленный ниже, для создания нового хранилища.

```
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

export const store = new Vuex.Store({
  state: {},
  getters: {},
  mutations: {},
  actions: {},
});
```

# Шаг 3. Добавление хранилища Vuex в приложение Vue.JS
>Импортируйте хранилище в файл main.js:
```
import {store} from './store';
```

>Добавьте хранилище к экземпляру Vue, как показано ниже:
```
new Vue({
  el: '#app',
  store,
  router,
  render: h => h(App),
});
```

#Пример TODO

```
mport Vue from 'vue';
import Vuex from 'vuex';
import Axios from 'axios';

Vue.use(Vuex);

export const store = new Vuex.Store({
  state: {
    todos: null,
  },

  getters: {
    TODOS: state => {
      return state.todos;
    },
  },

  mutations: {
    SET_TODO: (state, payload) => {
      state.todos = payload;
    },

    ADD_TODO: (state, payload) => {
      state.todos.push(payload);
    },
  },

  actions: {
    GET_TODO: async (context, payload) => {
      let {data} = await Axios.get('http://yourwebsite.com/api/todo');
      context.commit('SET_TODO', data);
    },

    SAVE_TODO: async (context, payload) => {
      let {data} = await Axios.post('http://yourwebsite.com/api/todo');
      context.commit('ADD_TODO', payload);
    },
  },
});
```

>Добавление нового элемента в список дел
```
let item = 'Get groceries';
this.$store.dispatch('SAVE_TODO', item);
```

>Получение элементов списка дел
```
mounted() {
  this.$store.dispatch('GET_TODO');
}
```

>Доступ к элементам списка дел внутри компонента
```
computed: {
  todoList() {
    return this.$store.getters.TODOS;
  },
}

<div class="todo-item" v-for="item in todoList"></div>
```

>Использование метода mapGetters
```
import {mapGetters} from 'vuex';

computed : {
  ...mapGetters(['TODOS']),
  // Другие вычисляемые свойства
}

<div class="todo-item" v-for="item in TODOS"></div>
```

#Организация кода

>Создайте новую директорию внутри вашего хранилища и назовите ее modules. Добавьте в созданную директорию файл todos.js, содержащий следующий код:
```
const state = {};
const getters = {};
const mutations = {};
const actions = {};

export default {
  state,
  getters,
  mutations,
  actions,
};
```

>Теперь мы можем переместить переменные состояния, геттеры, мутации и действия из файла index.js в файл todos.js.Обновленный файл index.js должен выглядеть примерно так:
```
import Vue from 'vue';
import Vuex from 'vuex';
import Axios from 'axios';
import todos from './modules/todos';

Vue.use(Vuex);

export const store = new Vuex.Store({
  state: {},
  getters: {},
  mutations: {},
  actions: {},
  modules: {
    todos,
  },
});
```

>Файл todos.js будет выглядеть так:

```
import Axios from 'axios';

state = {
  todos: null,
};

getters = {
  TODOS: state => {
    return state.todos;
  },
};

mutations = {
  SET_TODO: (state, payload) => {
    state.todos = payload;
  },

  ADD_TODO: (state, payload) => {
    state.todos.push(payload);
  },
};

actions = {
  GET_TODO: async (context, payload) => {
    let {data} = await Axios.get('http://yourwebsite.com/api/todo');
    context.commit('SET_TODO', data);
  },

  SAVE_TODO: async (context, payload) => {
    let {data} = await Axios.post('http://yourwebsite.com/api/todo');
    context.commit('ADD_TODO', payload);
  },
};

export default {
  state,
  getters,
  mutations,
  actions,
};
```


#Резюме
>Состояние приложения хранится как один большой JSON-объект.

>Геттеры используются для доступа к значениям, находящимся в хранилище.

>Мутации обновляют ваше состояние. Следует помнить, что мутации являются синхронными.

>Все асинхронные операции должны выполняться внутри действий. Действия изменяют состояние, инициируя мутации.

>Возьмите за правило инициировать мутации исключительно через действия.

>Модули могут использоваться для организации вашего хранилища в нескольких небольших файлах.




























