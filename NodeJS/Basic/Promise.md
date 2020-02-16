# Promise 对象

## Promise 对象返回值获取

e.g.
```
const request = require('request');

function get(url)
{
    return new Promise((resolve,reject)=>{
        request({url,method:'GET'},(error,response,body)=>{
            if (error){
                return reject(error);
            }
            return resolve({body,response});
        })
    }).catch((error)=>{
        console.log(error);
    })
    
}

async function main()
{
    let a = await get("http://localhost:50000/beta/panel/data-point?filter=count");
    console.log(a.body);
}
main()
```