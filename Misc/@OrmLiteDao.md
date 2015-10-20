#Ormlite
>since AndroidAnnotation 2.7

感谢Johan Poirier,你能够使用`@OrmLiteDao`注解。

**Note：需要的最低版本是4.21**

`@OrmLiteDao`拥有一个强制性的属性：
 - `helper`中必须拥有database helper（从`com.j256.ormlite.android.apptools.OrmLiteSqliteOpenHelper`中集成而来。
）

**Note**：为了能够使用和释放helper，我们使用`OpenHelperManager`这个类，该类不能同时处理两个不同的helper。所以，小心了，最好不要同时出现两个helper。

栗子：

```java
@EActivity
public class MyActivity extends Activity {

    // UserDao is a Dao<User, Long>
    @OrmLiteDao(helper = DatabaseHelper.class)
    UserDao userDao;

    @OrmLiteDao(helper = DatabaseHelper.class)
    Dao<Car, Long> carDao;

}
```

在所有的AA 注解中都能使用！

##RuntimeExceptionDao
>since AndroidAnnotation 3.0


##Transactional
数据库操作为原子操作，如果执行的方法在运行时抛出RunntimeException，事务将进行回滚。

注解的方法必须至少有一个参数，且必须是SQLiteDatabase。

方法必须是private并且不能抛出异常。

栗子：

```java
@Transactional
void doSomeDbWork(SQLiteDatabase db) {
    db.execSQL("Some SQL");
}
```


