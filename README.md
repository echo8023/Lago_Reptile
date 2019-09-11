# Lago_Reptile
Learning HTTP module of node.js
// 初学node.js中的http模块---爬虫练习

// 第一版，请求获取到了拉勾网所有文件数据
// // 引入模块
// const http=require("https");
// const fs=require("fs");
// const cheerio=require("cheerio");
// // 发送请求
// http.get("https://www.lagou.com/",(res)=>{ //response响应我们的请求
//     res.setEncoding("utf8");
//     let rawData="";
//     res.on("data",(chunk)=>{ //响应成功执行(获取数据)
//         console.log("------");//方便查看拼接的次数，可有可无
//         rawData+=chunk;
//     });
//     res.on("end",()=>{ //获取数据结束后执行
//         console.log(rawData);
//     });
// }).on("error",(e)=>{ //链式操作，响应不成功执行
//     console.error(`Got error:${e.message}`);
// });





// 第二版，请求获取到了拉勾网html文件数据
// 引入模块
// const http = require("https");
// const fs = require("fs");
// const cheerio = require("cheerio");
// // 发送请求
// http.get("https://www.lagou.com/", (res) => { //response响应我们的请求


//     // 获取html文件(获取我们需要的文件，在此处加一个判断)
//     const { statusCode } = res;//解构赋值res对象，拿到里面的http状态码
//     const contentType = res.headers["content-type"]; //申明内容类型，请求头中包含了很多信息，其中就包括内容类型
//     let error;
//     if (statusCode !== 200) {
//         error = new Error("请求失败");
//     } else if (!/^text\/html/.test(contentType)) { //状态码200，请求成功的情况下，判断内容是不是需要的html类型
//         error = new Error("文件类型错误");
//     }
//     if (error) {
//         console.error(error.message);
//         res.resume();//清除缓存，作用？？？？？？？？
//         return;
//     }


//     res.setEncoding("utf8");
//     let rawData = "";
//     res.on("data", (chunk) => { //响应成功执行(获取数据)
//         console.log("------");//方便查看拼接的次数，可有可无
//         rawData += chunk;
//     });
//     res.on("end", () => { //获取数据结束后执行
//         console.log(rawData);
//     });
// }).on("error", (e) => { //链式操作，响应不成功执行
//     console.error(`Got error:${e.message}`);
// });






// 第三版，通过获取到的拉勾网html文件数据，提取出自己需要的数据(比如提取图片url)
// 通过第三方模块cheerio。操作DOM结构
// 引入模块
// const http = require("https");
// const fs = require("fs");
// const cheerio = require("cheerio");
// // 发送请求
// http.get("https://www.lagou.com/", (res) => { //response响应我们的请求


//     // 获取html文件(获取我们需要的文件，在此处加一个判断)
//     const { statusCode } = res;//解构赋值res对象，拿到里面的http状态码
//     const contentType = res.headers["content-type"]; //申明内容类型，请求头中包含了很多信息，其中就包括内容类型
//     let error;
//     if (statusCode !== 200) {
//         error = new Error("请求失败");
//     } else if (!/^text\/html/.test(contentType)) { //状态码200，请求成功的情况下，判断内容是不是需要的html类型
//         error = new Error("文件类型错误");
//     }
//     if (error) {
//         console.error(error.message);
//         res.resume();//清除缓存，作用？？？？？？？？
//         return;
//     }


//     res.setEncoding("utf8");
//     let rawData = "";
//     res.on("data", (chunk) => { //响应成功执行(获取数据)
//         console.log("------");//方便查看拼接的次数，可有可无
//         rawData += chunk;
//     });




//     res.on("end", () => { //获取数据结束后执行
//         // console.log(rawData); //在控制台输出获取的数据(所有的html文件内容)
//         // 从获取的数据中(html文件内容)，提取出自己需要的数据(比如提取图片),通过cheerio模块处理这块内容--获取数据结束后执行的内容

//         const $=cheerio.load(rawData);
//         $("img").each((index,elm)=>{ //从获取的数据中找到img标签,遍历。
//             console.log($(elm).attr("src")); //获取img标签中的src属性值，发现输出的链接开头的格式不全是https的。
//         });



//     });
// }).on("error", (e) => { //链式操作，响应不成功执行
//     console.error(`Got error:${e.message}`);
// });






// 第四版，通过获取到的拉勾网html文件数据，提取出自己需要的数据(比如提取图片，并保存到本地文件中)
// 通过第三方模块cheerio。操作DOM结构
// 引入模块
const http = require("https");
const fs = require("fs");
const cheerio = require("cheerio");
// 发送请求
http.get("https://www.lagou.com/", (res) => { //response响应我们的请求


    // 获取html文件(获取我们需要的文件，在此处加一个判断)
    const { statusCode } = res;//解构赋值res对象，拿到里面的http状态码
    const contentType = res.headers["content-type"]; //申明内容类型，请求头中包含了很多信息，其中就包括内容类型
    let error;
    if (statusCode !== 200) {
        error = new Error("请求失败");
    } else if (!/^text\/html/.test(contentType)) { //状态码200，请求成功的情况下，判断内容是不是需要的html类型
        error = new Error("文件类型错误");
    }
    if (error) {
        console.error(error.message);
        res.resume();//清除缓存，作用？？？？？？？？
        return;
    }


    res.setEncoding("utf8");
    let rawData = "";
    res.on("data", (chunk) => { //响应成功执行(获取数据)
        console.log("------");//方便查看拼接的次数，可有可无
        rawData += chunk;
    });




    res.on("end", () => { //获取数据结束后执行
        // console.log(rawData); //在控制台输出获取的数据(所有的html文件内容)
        // 从获取的数据中(html文件内容)，提取出自己需要的数据(比如提取图片),通过cheerio模块处理这块内容--获取数据结束后执行的内容

        // const $=cheerio.load(rawData);
        // $("img").each((index,elm)=>{ //从获取的数据中找到img标签,遍历。
        //     console.log($(elm).attr("src")); //获取img标签中的src属性值，发现输出的链接开头的格式不全是https的。通过下面方式处理，将不符合规则的替换掉。
        // 如下操作：
        // });

        
        // 将不符合规则的开头用正则替换掉。需要替换的有以//和http开头的
        // 如下操作：
        let reg = /^(\/\/)|^(http:\/\/)/; //设置以//和http开头的规则
        const $ = cheerio.load(rawData);
        
        $("img").each((index, elm) => {
            let url = $(elm).attr("src").replace(reg,"https://");
            // console.log(url); //字符串
            //通过文件操作,将拿到的图片地址存到本地目录下(经过上面操作以统一为了https//开头的url)：如下

            // 2.截取url中，最后一个斜杠后面的内容(文件名)。 
            // 下面的fs.writeFile()中的第一个参数需要用到
            let file = url.split("/"); //数组
            // file.pop() 获得文件名。//数组的pop()方法

            //1. 发起请求图片的请求
            http.get(url, (res) => {
                let imgData = "";
                res.setEncoding("binary"); //设置图片的编码格式

                res.on("data", (chunk) => { //响应成功执行(获取数据)
                    imgData += chunk;
                });
                res.on("end", () => { //获取数据结束后执行(想要存入本地文件中--写入文件writeFile()方法, 此处需要操作文件模块，不要忘了在开头引入该模块)
                    fs.writeFile("./imgjd2/" + file.pop(), imgData, "binary", (err) => { //writeFile()四个参数，第一个写入到的文件路径，第二个参数获取的数据，第三个参数图片编码格式，第四个回调函数
                        if (err) {
                            console.log("图片下载失败");
                        } else {
                            console.log("图片下载成功");
                        }
                    });

                });
            });
        });


    });
}).on("error", (e) => { //链式操作，响应不成功执行
    console.error(`Got error:${e.message}`);
});


