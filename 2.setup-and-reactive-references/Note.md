## When to use the Composition API?

- Typescript support.
- Component is too large and needs to be organized by feature.
- Need to reuse code across other components.

## The Composition API Setup Method

```javascript
//
<template>
  <div>Capacity: {{ capacity }}</div>
</template>

<script>
  data() {
    return {
      capacity: 3
    }
  }
</script>
```

```javascript
import { watch } from 'vue'
<script>
  // Optional first argument is props
  // It is reactive and can be watched
  props: {
    name: String
  },
  setup(props) {
    watch(() => {
      console.log(props.name)
    })
  }
</script>
```