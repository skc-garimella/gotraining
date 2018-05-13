# Structs and Alignment

## Alignment Boundries

It is more efficient for hardware to read the memory along the alignment boundaries. Software(compiler) takes care of the alignment issues so that hardware need not do this. This was a nice cost trade-off.

```go
type exampleStruct struct {
    b bool      //1 byte, 0
                //1 byte padding for alignment, 1
    i int16     //2 bytes, 2-3
    f float32   //4 bytes, 4-7
}
```

go has the following rules about alignment boundries:

* Every 2 byte value should start at 2 byte boundry (0, 2, 4, ...).
* Every 4 byte value should start at 4 byte boundry (0, 4, 8, ...).
* Every 8 byte value on 8 byte boundry and so on...

int16, being 2 bytes, should start at 2 byte boundry. Hence 1 byte of padding is added after the bool.

### Alignment examples

```go
type exampleStruct struct {
    b bool      //1 byte, 0
                //3 bytes padding for alignment, 1-3
    i int32     //4 bytes, 4-7
    f float32   //4 bytes, 8-11
}
```

```go
type exampleStruct struct {
    b bool      //1 byte, 0
                //7 bytes padding for alignment, 1-7
    i int64     //8 bytes, 8-15
    f float32   //4 bytes, 16-19
}
```

```go
type exampleStruct struct {
    b  bool      //1 byte, 0-0
                 //7 bytes padding for alignment, 1-7
    i  int64     //8 bytes, 8-15
    b1 bool      //1 bytes, 16-16
                 //7 bytes padding for alignment, 17-23
    i1 int64     //8 bytes, 24-31
    b2 bool      //1 bytes, 32-32
                 //3 bytes padding for alignment, 33-35
    f  float32   //4 bytes, 36-39
}
```

***Always lay the fields out from highest to lowest to minimize padding.***

```go
type exampleStruct struct {
    i  int64
    i1 int64
    f  float32
    b  bool
    b1 bool
    b2 bool
}
```

## Declaring structs and accessing its fields

```go
type exampleStruct struct {
    f float32
    i int16
    b bool
}

//Declare variable of type exampleStruct set to its zero value
var e exampleStruct

//Declare variable of type exampleStruct and initialize it using struct literal
e1 := exampleStruct{
    f: 1.23,
    i: 1,
    b: true,
}

//Display fields
fmt.Println("bool", e1.b)
fmt.Println("integer", e1.i)
```

### Anonymous struct

```go
e := struct {
    f float32
    i int16
    b bool
}{
    f: 1.23,
    i: 1,
    b: true,
}
```
