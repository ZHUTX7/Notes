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
LIKE concat('%',#{account},'%')