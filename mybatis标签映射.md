@Mapper
public interface StudentMapper {
    /**
     * 查询所有学生
     * @return
     */
    @Select("select * from student")
    @Results({
            @Result(id = true,property = "id",column = "id"),
            @Result(property = "name",column = "name"),
            @Result(property = "age",column = "age"),
            @Result(property = "gender",column = "gender"),
            @Result(property = "cid",column = "cid"),
            //用cid字段通过配置的路径去查找这个aClass对象
            @Result(property = "aClass",column = "cid",one = @One(select = "",
                    fetchType = FetchType.LAZY)),
    })
    })
    List<Student> findAll();
————————————————
版权声明：本文为CSDN博主「xyc118476」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/xyc118476/article/details/89505483