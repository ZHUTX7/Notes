```java
//-----------------------1 ------------------------
//    @RequestMapping("/get/{id}")
//    public void get(@PathVariable("id") String idStr) throws Exception{
//        if ("".equals(idStr)) {
//            throw new Exception("id为空");
//        }
//        Integer id = Integer.parseInt(idStr);
//        User user = userService.get(id);
//        logger.info("获取的用户信息：{}",user.toString());
//
//    }

//
//-----------------------2-----------------------
    @GetMapping("/get")
    public void get( String id) throws Exception{
        if ("".equals(id)) {
            throw new Exception("id为空");
        }
        Integer id2 = Integer.parseInt(id);
        User user = userService.get(id2);
        logger.info("获取的用户信息：{}",user.toString());

    }
//  假如ID=1有值 , 则第一种方法 url = ".../get/1"
//                 第二种方法 url  = ".../get?id=1"
```

