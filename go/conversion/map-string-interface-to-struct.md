# map\[string\]interface{} to struct

## Conversion

### map\[string\]interface{} to struct

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

    // struct - Need to be defined according to the above map data.
    type Addr struct {
        PostalCode int
        Country string
    }
    type Me struct {
        Name string
        Age int
        Admin bool
        Hobbies []string
        Address Addr
        Null interface{}
    }

    // Convert map to json string
    jsonStr, err := json.Marshal(mapData)
    if err != nil {
        fmt.Println(err)
    }

    // Convert json string to struct
    var me Me
    if err := json.Unmarshal(jsonStr, &me); err != nil {
        fmt.Println(err)
    }

    // Output
    fmt.Printf("Name: %s
Age: %d
Admin: %t
Hobbies: %v
Address: %v
Null: %v
", me.Name, me.Age, me.Admin, me.Hobbies, me.Address, me.Null)
}
```

Ref: [https://noknow.info/it/go/how\_to\_conveert\_between\_map\_string\_interface\_and\_struct\#sec1](https://noknow.info/it/go/how_to_conveert_between_map_string_interface_and_struct#sec1)



### struct to map\[string\]interface{}

```go
package main

import (
    "encoding/json"
    "fmt"
)

func main() {
    // struct data
    type Addr struct {
        PostalCode int
        Country string
    }
    type Me struct {
        Name string
        Age int
        Admin bool
        Hobbies []string
        Address Addr
        Null interface{}
    }
    addr := Addr{
        PostalCode: 1111,
        Country: "Japan",
    }
    me := Me{
        Name: "noknow",
        Age: 2,
        Admin: true,
        Hobbies: []string{"IT","Travel"},
        Address: addr,
        Null: nil,
    }

    // Convert map to json string
    jsonStr, err := json.Marshal(me)
    if err != nil {
        fmt.Println(err)
    }

    // Convert struct
    var mapData map[string]interface{}
    if err := json.Unmarshal(jsonStr, &mapData); err != nil {
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

Ref: [https://noknow.info/it/go/how\_to\_conveert\_between\_map\_string\_interface\_and\_struct\#sec2](https://noknow.info/it/go/how_to_conveert_between_map_string_interface_and_struct#sec2)

