# Anime Search

## How to deploy on github

### 1 Create and build vue project

```cmd
vue create my-vue-app
cd my-vue-app
npm run build
```

### 2 Install gh-pages:

```cmd
npm install gh-pages --save-dev
```

### 3 Update vue.config.js:

```js
module.exports = {
  publicPath: process.env.NODE_ENV === "production" ? "/your-repo-name/" : "/",
};
```

### 4 Add Deploy Script:

Add a deploy script in your package.json.

```json
"scripts": {
  "deploy": "gh-pages -d dist"
}
```

### 5 Deploy:

Run the deploy script.

```cmd
npm run build
npm run deploy
```

Afterwards on your github repository, go on settings -> pages -> deploy on branch and change it to gh-pages.

## Notes

Start from Vue 3, it has a restruction/rule in naming the file. Every Vue file names except the main(App.vue) has to be multi-name.

```Vue
/* ✓ GOOD */
Vue.component('todo-item', {
  // ...
})

/* ✗ BAD */
Vue.component('Todo', {
  // ...
})
```

## Anime Search Data Base | 7/1/24 ~ TBD

### Additional Features:

- Customization in color and design of website
- When entering the webpage, it will shows a list of recent anime recommendation of anime instead of an empty page.

### Problem #1:

The main file does not display the DOM I imports from the components file. Nor the misspell or incorrect use of name.

**Solution**
In the current Vue's version, you need to use components tools from options API to define components.

```Vue
<script>
import { ref } from "vue";
import AnimeCard from "./components/AnimeCard.vue";

export default {
  components: {
    AnimeCard
  },
  setup() {
    const search_query = ref("");

    return {
      search_query,
    };
  },
};
</script>

```
