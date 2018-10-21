

1. int[]数组转ArrayList

```java
ArrayList<Integer> temp = (ArrayList<Integer>) Arrays.stream(a).boxed().collect(Collectors.toList());
```

1. ArrayList拼接字符串

```java
temp.stream().map(Objects::toString).collect(Collectors.joining());
```

1. 对汉字排序

   ```java
   private final static Comparator<Object> CHINA_COMPARE = Collator.getInstance(java.util.Locale.CHINA);
   
   Comparator<WorkOrderVendorVo> cm = new Comparator<WorkOrderVendorVo>() {
       @Override
       public int compare(WorkOrderVendorVo o1, WorkOrderVendorVo o2) {
           return CHINA_COMPARE.compare(o1.getVendorShortName(), o2.getVendorShortName());
       }
   };
   Collections.sort(vendorResList, cm);
   ```

2. 





















































