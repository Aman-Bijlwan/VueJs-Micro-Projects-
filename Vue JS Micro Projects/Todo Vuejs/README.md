# 📋 Todo App with Vue.js
> This Todo App is a simple yet powerful implementation using Vue.js. It showcases key Vue concepts such as reactivity, computed properties, watchers, localStorage integration, and the v-for directive.
> 
> Below is a detailed breakdown of the concepts and how they work together in this application.

## 🚀 Concepts in Use
### 1. Reactivity with ref
- The `ref()` function in Vue is used to create reactive variables. When these reactive variables change, Vue automatically updates the UI to reflect the changes.
- In this app, ref is used to create reactive state variables such as:
     - todos: An array storing the to-do list.
     - name: A string storing the user's name.
     - input_content, input_category: Temporary inputs for creating new todos.
     
```js
const todos = ref([]);
const name = ref('');
const input_content = ref('');
const input_category = ref(null);
```

### 2. Computed Properties
- computed properties automatically update themselves whenever their dependent data changes. They are great for creating derived state or values based on reactive data.
- In this app, todos_asc is a computed property that sorts the todos array by the createdAt timestamp, ensuring the to-dos are always shown in chronological order.

```js 
const todos_asc = computed(() => todos.value.sort((a, b) => a.createdAt - b.createdAt));
```

### 3. Watchers
- watch allows you to observe changes to a reactive property and execute a function when the value changes.
- Here, we are using watch to monitor changes to the name and todos variables and store them in localStorage so the data persists even after a page reload.

```js 
watch(name, (newVal) => {
  localStorage.setItem('name', newVal);
});

watch(todos, (newVal) => {
  localStorage.setItem('todos', JSON.stringify(newVal));
}, { deep: true });
```

### 4. Lifecycle Hook: onMounted
- onMounted is a lifecycle hook that runs when the component is mounted (i.e., when it's rendered to the DOM for the first time).
- This hook is used to retrieve data from localStorage when the app loads, ensuring the name and todos persist after page reloads.

```js 
onMounted(() => {
  name.value = localStorage.getItem('name') || '';
  todos.value = JSON.parse(localStorage.getItem('todos')) || [];
});
```

### 5. Directives: v-for
- The v-for directive is used to loop over data and create DOM elements for each item.
- In this app, v-for is used to loop over the todos_asc array to dynamically render each to-do item.

```js 
<div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">
  <label>
    <input type="checkbox" v-model="todo.done" />
    <span :class="`bubble ${todo.category == 'business' ? 'business' : 'personal'}`"></span>
  </label>
  <div class="todo-content">
    <input type="text" v-model="todo.content" />
  </div>
</div>

```

### 6. Form Handling with @submit.prevent
- The form uses the @submit.prevent directive, which prevents the form's default behavior (page reload) when submitted. Instead, it calls the addTodo() method, adding the new to-do item to the list.

```js
<form id="new-todo-form" @submit.prevent="addTodo">
  <input type="text" v-model="input_content" />
  <div class="options">
    <label>
      <input type="radio" value="business" v-model="input_category" />
      <span class="bubble business"></span>
    </label>
    <label>
      <input type="radio" value="personal" v-model="input_category" />
      <span class="bubble personal"></span>
    </label>
  </div>
  <input type="submit" value="Add todo" />
</form>
```

## 📝 Key Features
1. **Persistent Data with LocalStorage:** The app saves the user's name and todos in localStorage using watch, allowing for data persistence across page reloads.
2. **Add Todos:** Users can input content and select a category (business/personal), which gets added to the todo list.
3. **Dynamic List:** Todos are displayed dynamically using v-for, and users can mark them as done, delete them, or edit their content directly.
4. **Sorting with Computed Properties:** Todos are automatically sorted by their creation time using a computed property.
## 🔧 Code Breakdown
### addTodo()
This method adds a new to-do to the todos array, using the content from input_content and input_category, and sets a createdAt timestamp.

```JS
const addTodo = () => {
  if (input_content.value.trim() === '' || input_category.value === null) return;
  
  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    editable: false,
    createdAt: new Date().getTime(),
  });
};
```

### removeTodo()
This method removes a selected to-do item from the todos array.

```JS 
const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo);
};
```
## 💡 Concepts Summary:
- **Reactivity:** Vue's ref makes data reactive and automatically updates the UI.
- **Computed Properties:** Used for derived state and efficient calculations.
- **Watchers:** Useful for side effects, like saving data to localStorage whenever a value changes.
- *Lifecycle Hooks:* onMounted is triggered when the component is rendered for the first time.
- **Directives:** v-for is a powerful tool for looping through data and rendering it in the DOM.

****
Feel free to explore the code and expand it with additional features, such as filtering todos, adding deadlines, or implementing a priority system. Happy coding! 🎉