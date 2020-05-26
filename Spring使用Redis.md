# 1.  导入Maven依赖

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis</artifactId>
    </dependency>
</dependencies>
```



# 2.  配置application.properties

```xml
spring.redis.host=127.0.0.1
spring.redis.port=6379
spring.redis.password=123456
spring.redis.database=0
```



# 3. 创建Controller类

```
@RestController
@RequestMapping("/user")
public class UserController {

    public static Logger logger = LogManager.getLogger(UserController.class);

    @Autowired
    private StringRedisTemplate stringRedisTemplate;

    @Autowired
    private UserService userService;


    @Autowired
    private RedisTemplate<String, Serializable> redisCacheTemplate;

    @RequestMapping("/test")
    public void test() {
        redisCacheTemplate.opsForValue().set("userkey", new User(1, "张三", 25));
        User user = (User) redisCacheTemplate.opsForValue().get("userkey");
        redisCacheTemplate.opsForValue().set("ttt","123",30L, TimeUnit.SECONDS);
        logger.info("当前获取对象：{}", user.toString());
    }
    @RequestMapping("/add")
    public void add() {
        User user = userService.save(new User(4, "test_add", 30));

        logger.info("添加的用户信息：{}",user.toString());
    }

    @RequestMapping("/delete")
    public void delete() {
        userService.delete(4);
    }

    @RequestMapping("/get/{id}")
    public void get(@PathVariable("id") String idStr) throws Exception{
        if ("".equals(idStr)) {
            throw new Exception("id为空");
        }
        Integer id = Integer.parseInt(idStr);
        User user = userService.get(id);
        logger.info("获取的用户信息：{}",user.toString());

    }

}
```



# 4. Service层



往Redis里面放值

第一种方法:

主要运用RedisTemplate 中 opsForValue方法,

 其中opsForValue 有三个set方法

```
void set(K var1, V var2);
void set(K var1, V var2, long var3, TimeUnit var5);
default void set(K key, V value, Duration timeout) 
```

可以定义过期事件, 调用第二个方法的需要指定时间单位, 第三个则默认时间单位为天



第二种方法: 

运用标签

```
@CachePut(value ="user", key = "#user.id")
@Override
public User save(User user) {
    userMap.put(user.getId(), user);
    logger.info("进入save方法，当前存储对象：{}", user.toString());
    return user;
}
```



```
@CacheEvict(value="user", key = "#id")
@Override
public void delete(int id) {
    userMap.remove(id);
    logger.info("进入delete方法，删除成功");
}
```



```
@Cacheable(value = "user", key = "#id")
@Override
public User get(Integer id) {
    logger.info("进入get方法，当前获取对象：{}", userMap.get(id)==null?null:userMap.get(id).toString());
    return userMap.get(id);
}
```





其中Cacheable 是先去内存中找是否存在user , 如果存在则直接返回user, 不存在则进入方法体执行方法 . 

而cacheput是直接进入方法, 往内存塞值 .