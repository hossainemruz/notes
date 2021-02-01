# map\[string\]interface{} to JSON

## Conversion

### Convert map\[string\]interface{} to JSON string <a id="sec1"></a>

```go
package main

import (
    "encoding/json"
    "fmt"
)

func main() {
    // map data
    mapData := map[string]interface{}{
        "Name": "noknow",
        "Age": 2,
        "Admin": true,
        "Hobbies": []string{"IT","Travel"},
        "Address": map[string]interface{}{
            "PostalCode": 1111,
            "Country": "Japan",
        },
        "Null": nil,
    }

    // Convert map to json string
    jsonStr, err := json.Marshal(mapData)
    if err != nil {
        fmt.Println(err)
    }

    // Output
    fmt.Println(string(jsonStr))
}
```

Ref: [https://noknow.info/it/go/how\_to\_convert\_between\_map\_string\_interface\_and\_json\_string?lang=en\#sec1](https://noknow.info/it/go/how_to_convert_between_map_string_interface_and_json_string?lang=en#sec1)

### Convert JSON string to map\[string\]interface{} <a id="sec2"></a>

```go
package main

import (
    "encoding/json"
    "fmt"
)

func main() {
    // json string
    jsonStr := `{
        "Name": "noknow",
        "Age": 2,
        "Admin": true,
        "Hobbies": ["IT","Travel"],
        "Address": {
            "PostalCode": 1111,
            "Country": "Japan"
        },
        "Null": null
    }`

    // Convert json string to map[string]interface{}
    var mapData map[string]interface{}
    if err := json.Unmarshal([]byte(jsonStr), &mapData); err != nil {
        fmt.Println(err)
    }

    // Output
    fmt.Printf("Name: %v (%T)
Age: %v (%T)
Admin: %v (%T)
Hobbies: %v (%T)
Address: %v (%T)
Null: %v (%T)
", mapData["Name"], mapData["Name"], mapData["Age"], mapData["Age"], mapData["Admin"], mapData["Admin"], mapData["Hobbies"], mapData["Hobbies"], mapData["Address"], mapData["Address"], mapData["Null"], mapData["Null"])
}

```

Ref: [https://noknow.info/it/go/how\_to\_convert\_between\_map\_string\_interface\_and\_json\_string?lang=en\#sec2](https://noknow.info/it/go/how_to_convert_between_map_string_interface_and_json_string?lang=en#sec2)

