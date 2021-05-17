# Restful

==URL请求格式改变==

传统URL请求格式

```
http://127.0.0.1/user/query/1 GET 根据用户id查询用户数据

http://127.0.0.1/user/save POST 新增用户

http://127.0.0.1/user/update POST 修改用户信息

http://127.0.0.1/user/delete GET/POST 删除用户信息
```

Restful请求格式

```
http://127.0.0.1/user/1 GET 根据用户id查询用户数据

http://127.0.0.1/user POST 新增用户

http://127.0.0.1/user PUT 修改用户信息

http://127.0.0.1/user DELETE 删除用户信息
```



使用Restful方法举例

```java
@Controller
public class RestFulController {
    
    @GetMapping("/add/{a}/{b}")
    //@PostMapping
    //@PutMapping
    //DeleteMapping
    //@PatchMapping
    public String test1(@PathVariable int a,@PathVariable String b,Model model){
        String res = a+b;
        model.addAttribute("msg","结果为"+res);
        return "test";
    }
}
```



@GetMapping == @RequestMapping( value = "/add"  method = RequestMethod.GET)