# 1、异步

## 图解

<img src="https://raw.githubusercontent.com/xj-070/MarkDown/project/myproject/crowdfunding/image-20220521193244404.png" alt="image-20220521193244404" style="zoom:67%;" />

## 代码

```jsp
<script type="text/javascript">
    $(function (){
        /*async:asynchronous异步的*/
        $("#asyncBtn").click(function () {
            console.log("ajax函数之前");
            $.ajax({
                "url":"test/ajax/async.html",
                "type":"post",
                "dataType":"text",
                "success":function(response){
                // success 是接收到服务器端响应后执行
                    console.log("ajax 函数内部的success 函数"+response);
                }
            });
              setTimeout(function () {
                // 在$.ajax()执行完成后执行，不等待success()函数
                console.log("ajax函数之后");
            },5000);
        });
    });
</script>
```

**结果**

<img src="https://raw.githubusercontent.com/xj-070/MarkDown/project/myproject/crowdfunding/image-20220521194930545.png" alt="image-20220521194930545" style="zoom:67%;" />

❗️ **注意：**405错误，Controller层中的@Controller注解没写



# 2、同步

## 图解

<img src="https://raw.githubusercontent.com/xj-070/MarkDown/project/myproject/crowdfunding/image-20220521195348992.png" alt="image-20220521195348992" style="zoom:67%;" />

## 代码

<font color = blue>**async:false**</font>

**❗️ 注意**：false不要加引号，如if（”false“）会被判真，（非空字符串）

```js
<script type="text/javascript">
    $(function (){
        /*async:asynchronous异步的*/
        $("#asyncBtn").click(function () {
            console.log("ajax函数之前");
            $.ajax({
                "url":"test/ajax/async.html",
                "type":"post",
                "dataType":"text",
                a/*改成同步*/
                async:false,// 关闭异步工作模型，使用同步方式工作。此时：所有操作在同一个线程内按顺序完成
                "success":function(response){
                // success 是接收到服务器端响应后执行
                    console.log("ajax 函数内部的success 函数"+response);
                }
            });
            // 异步的
          /*  setTimeout(function () {
                // 在$.ajax()执行完成后执行，不等待success()函数
                console.log("异步：ajax函数之后");
            },5000);*/

            // 同步的
            console.log("同步：ajax函数之后")
        });
    });
</script>
```



## 3、本质

- 同步：同一个线程内部按顺序执行
- 异步：多个线程同时并行执行，谁也不等谁





