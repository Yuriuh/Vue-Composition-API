# Vue2的缺点
  - Readability as components grow
  - Code reuse patterns have drawbacks
  - limited TypeScript Support

## The first limitation with Vue2
Large components can become hard to read & maintain

eg: Just with Search Functionality

```javascript
// Search.vue with Vue2
<script>
export default {
  data() {
    return {
      // Search
      // Sorting
    }
  },
  methods: {
    // Search
    // Sorting
  }
}
</script>
```
Eventually we might want to add search filters and pagination.

Logical concerns(features) are organized by component options.

Options are:
- components
- props
- data
- computed
- methods
- lifecycle methods

```javascript
// Search.vue with Vue3
<script>
export default {
  setup() {
    // Search
    // Sorting

    // Composition API New Syntax Completely Optional
    // Wait, doest this mean I create a gigantic setup method?
  }
}
</script>
```

## The first limitation with Vue2
No perfect way to reuse logic between components.

Three ways:
- mixins
- mixins factories
- scoped slots

### Mixins
```javascript
// Search.vue with Vue2
<script>
const productSearchMixin = {

}
const resultSortMixin = {

}
export default {
  mixins: [
    productSearchMixin,
    resultSortMixin,
  ]
}
</script>
```

- Organized by feature
- Conflict prone
- Unclear relationships
- Not easily reusable

## Mixin Factories
Functions that return a customized version of a mixin

```javascript
// mixins/factories/search.js
export default function searchMixinFactory({}) {

}
```

```javascript
// mixins/factories/sorting.js
export default function sortingMixinFactory({}) {

}
```

```javascript
// Search.vue
import searchMixinFactory from '@mixins/factories/search'
import sortingMixinFactory from '@mixins/factories/sorting'

export default {
  mixins: [
    searchMixinFactory({
      namespace: 'productSearch',
    }),
    sortingMixinFactory({
      namespace: 'productSorting',
    })
  ]
}
```

- Easily Reusable
- Clearer Relationships
- Weak Namespacing
- Implicit property additions
- No instance access at runtime

## Scoped Slots

```javascript
// components/generic-search.vue
<script>
export default {
  props: ['getResults'],
}
</script>

<template>
  <div>
    <slot v-bind="{ query, results, run }" />
  </div>
</template>
```

```javascript
// components/generic-sorting.vue
<script>
export default {
  props: ['input', 'options'],
}
</script>

<template>
  <div>
    <slot v-bind="{ options, index, output }" />
  </div>
</template>
```

```javascript
// Search.vue
// ...
<template>
  <GenericSearch v-bind:get-result="getProducts" v-slot="productSearch">
    <GenericSorting
      v-bind:input="productSearch.results"
      v-bind:options="resultSortingOptions"
      v-slot="resultSorting"
    >
  <GenericSearch>
</template>
```

- Solves Mixin Problems
- Increases indentation
- Lots of configuration
- Less flexible
- Less performant

## Composition Function

```javascript
// use/search.js
export default function useSearch(getResults) {

}
```

```javascript
// use/sorting.js
export default function useSorting({ input, options }) {

}
```

```javascript
// Search.vue
import useSearch from '@use/search'
import useSorting from '@use/Sorting'

export default {
  setup() {
    // configuration
    const productSearch = useSearch()
    const resultSorting = useSorting({})

    return { productSearch, resultSorting }
  }
}
```

- Less code
- Familiar functions
- Extremely flexible
- Tooling friendly
- Advanced syntax