判断两个Date/时间戳 对象是否是同一天

最近有个需求是判断在创建新对象的今天之前已经创建了这个对象，如果是昨天创建的，则正常；

通过Calendar来获取date对象的年份和在该年份中第几天，然后比较这两个信息来判断是不是同一天

```java
SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
Date date = format.parse("2018-09-15 23:23:23");
Date date2 = format.parse("2018-09-14 23:23:23");
Calendar calendar = Calendar.getInstance();
Calendar calendar2 = Calendar.getInstance();
calendar.setTime(date);//给calendar赋值
int y1 = calendar.get(Calendar.YEAR);//获取年份
int d1 = calendar.get(Calendar.DAY_OF_YEAR);//获取年中第几天
calendar2.setTime(date2);

int y2 = calendar2.get(Calendar.YEAR);
int d2 = calendar2.get(Calendar.DAY_OF_YEAR);
if (y1 == y2 && d1 == d2)//判断是不是同一年的同一天
    System.out.println(true);
else
    System.out.println(true);
```

```java
/**
 * 获取 多少小时之前的时间
 * @param Hour
 * @return Date
 */
public Date getPastDateByHour(int Hour) {
    Calendar calendar = Calendar.getInstance();
    calendar.setTime(new Date());
    calendar.add(Calendar.HOUR_OF_DAY, -Hour);
    return calendar.getTime();
}

public Date getPastDateByDay(int Day) {
    Calendar calendar = Calendar.getInstance();
    calendar.setTime(new Date());
    calendar.add(Calendar.DAY_OF_YEAR, -Day);
    return calendar.getTime();
}
```