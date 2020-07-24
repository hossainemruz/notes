# Basics

## Array

### Declare an array

We can declare array in bash script using,

```bash
declare -a prefixArray
```

Now, we can put data on array at any index. Array is 0 indexed.

```bash
prefixArray[INDEX]=VALUE
prefixArray[0]="this is first element"
```

After using array clear the memory using `unset`,

```bash
unset prefixArray
```



