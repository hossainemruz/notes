# Restrict Key

## How to restrict patch from patching some specific key?

**Problem:** 

We want to patch an object but we don't want to allow to modify some fields of the object.

**Solution:**

Use `mergepatch.PreconditionFunc`.

What is `PreconditionFunc`?

`PreconditionFunc` asserts that an incompatible change is not present within a patch. It takes generated `patch` and return if either patch is allowed\(`true`\) or forbidden\(`false`\) based on key's existent on patch.

```go
type PreconditionFunc func(interface{}) bool
```

[mergepatch](https://github.com/kubernetes/apimachinery/tree/master/pkg/util/mergepatch/util.go) library has two helper function that return `PreconditionFunc`

```go
// RequireKeyUnchanged returns a precondition function that fails if the provided key
// is present in the patch (indicating that its value has changed).
func RequireKeyUnchanged(key string) PreconditionFunc {
	return func(patch interface{}) bool {
		patchMap, ok := patch.(map[string]interface{})
		if !ok {
			return true
		}

		// The presence of key means that its value has been changed, so the test fails.
		_, ok = patchMap[key]
		return !ok
	}
}
```

and

```go
// RequireMetadataKeyUnchanged creates a precondition function that fails
// if the metadata.key is present in the patch (indicating its value
// has changed).
func RequireMetadataKeyUnchanged(key string) PreconditionFunc {
	return func(patch interface{}) bool {
		patchMap, ok := patch.(map[string]interface{})
		if !ok {
			return true
		}
		patchMap1, ok := patchMap["metadata"]
		if !ok {
			return true
		}
		patchMap2, ok := patchMap1.(map[string]interface{})
		if !ok {
			return true
		}
		_, ok = patchMap2[key]
		return !ok
	}
}
```

**Using the above `PreconditonFunc` in** [strategicpatch](https://github.com/kubernetes/apimachinery/tree/master/pkg/util/strategicpatch)

```go
preconditions := []mergepatch.PreconditionFunc{
		mergepatch.RequireKeyUnchanged("apiVersion"),
		mergepatch.RequireMetadataKeyUnchanged("name"),
	}
```



**Creating Custom `PreconditionFunc` for** [jsonpatch](https://github.com/hossainemruz/notebook/blob/master/cloud/kubernetes/github.com/evanphx/json-patch)

```go
patch, err := jsonpatch.CreateMergePatch(curJson, modJson)
if err != nil {
    return nil, err
}
if err := meetPreconditions(patch, preconditions...); err != nil {
    return nil, err
}
```

```go
func meetPreconditions(patch []byte, fns ...mergepatch.PreconditionFunc) error {
    var patchMap map[string]interface{}
    if err := json.Unmarshal(patch, &patchMap); err != nil {
        return fmt.Errorf("failed to unmarshal patch for precondition check: %s", patch)
    }

    for _, fn := range fns {
        if !fn(patchMap) {
            return mergepatch.NewErrPreconditionFailed(patchMap)
        }
    }
    return nil
}
```



