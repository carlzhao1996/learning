# JS 数组函数

e.g.
```
const items = [
    { name: "Bike", price: 100 },
    { name: "TV", price: 200 },
    { name: "Album", price: 10 },
    { name: "Book", price: 5 },
    { name: "Phone", price: 500 },
    { name: "Computer", price: 1000 },
    { name: "Keyboard", price: 25 },
];
```

## Filter

```
function itemFilter(price) {
    return items.filter((item)=>{
        return item.price > price
    });
}

let result = itemFilter(100)
console.log(result)

/* result 
[ { name: 'TV', price: 200 },
  { name: 'Phone', price: 500 },
  { name: 'Computer', price: 1000 } ]
  */
```

## Reduce
```
const total = items.reduce((currentTotal,item)=>{
    return item.price+currentTotal
},0)

// total=1840 
```

## Map
```
const itemNames = items.map((item)=>{
    return item.name
})
console.log(itemNames)
// ["Bike", "TV", "Album", "Book", "Phone", "Computer", "Keyboard"]
```

## Find
```
function itemFinder(name) {
    let a = items.find((item)=>{
        return item.name == name;
    })
    if(!a){
        return "not found"
    }
    return a
}

console.log(itemFinder("Bike"))

// {name: "Bike", price: 100}
```
