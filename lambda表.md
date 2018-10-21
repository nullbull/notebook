

| 操作      | 类型              | 返回类型    | 使用的类型/函数式接口  | 函数描述符     |
| --------- | ----------------- | ----------- | ---------------------- | -------------- |
| filter    | 中间              | Stream<T>   | Predicate<T>           | T -> boolean   |
| distinct  | 中间(有状态-无界) | Stream<T>   |                        |                |
| skip      | 中间(有状态-无界) | Stream<T>   | long                   |                |
| limit     | 中间(有状态-无界) | Stream<T>   | long                   |                |
| map       | 中间              | Stream<R>   | Function<T, R>         | T -> R         |
| flatMap   | 中间              | Stream<R>   | Function<T, Stream<R>> | T -> Stream<R> |
| sorted    | 中间              | Stream<T>   | Comparator<T>          | (T, T) -> int  |
| anyMatch  | 终端              | boolean     | Predicate<T>           | T -> boolean   |
| noneMatch | 终端              | boolean     | Predicate<T>           | T -> boolean   |
| allMatch  | 终端              | boolean     | Predicate<T>           | T -> boolean   |
| findAny   | 终端              | Optional<T> |                        |                |
| findFirst | 终端              | Optional<T> |                        |                |
| forEach   | 终端              | void        | Consumer<T>            | T -> void      |
| collect   | 终端              | R           | Collector<T, A, R>     |                |
| reduce    | 终端              | Optional<T> | BinaryOperator<T>      | (T, T) -> T    |
| count     | 终端              | long        |                        |                |











1. 找出2011年发生的所有交易，并按交易额排序
2. 交易员在哪些不同的城市工作过
3. 查找所有来自剑桥的交易员，并按姓名排序
4. 返回所有交易员的姓名字符串，并按字母顺序排序
5. 有没有交易员在米兰工作的？
6. 打印生活在剑桥的交易员的所有交易额
7. 所有交易中，最高的交易额是多少
8. 找到交易额最小的交易



```
package Lambda;

public  class Trader{

    private String name;
    private String city;

    public Trader(String n, String c){
        this.name = n;
        this.city = c;
    }

    public String getName(){
        return this.name;
    }

    public String getCity(){
        return this.city;
    }

    public void setCity(String newCity){
        this.city = newCity;
    }

    public String toString(){
        return "Trader:"+this.name + " in " + this.city;
    }
}
```

```
package Lambda;

public class Transaction{

    private Trader trader;
    private int year;
    private int value;

    public Transaction(Trader trader, int year, int value)
    {
        this.trader = trader;
        this.year = year;
        this.value = value;
    }

    public Trader getTrader(){
        return this.trader;
    }

    public int getYear(){
        return this.year;
    }

    public int getValue(){
        return this.value;
    }

    public String toString(){
        return "{" + this.trader + ", " +
                "year: "+this.year+", " +
                "value:" + this.value +"}";
    }
}
```

```
public class PuttingIntoPractice{
    public static void main(String ...args){
        Trader raoul = new Trader("Raoul", "Cambridge");
        Trader mario = new Trader("Mario","Milan");
        Trader alan = new Trader("Alan","Cambridge");
        Trader brian = new Trader("Brian","Cambridge");

        List<Transaction> transactions = Arrays.asList(
                new Transaction(brian, 2011, 300),
                new Transaction(raoul, 2012, 1000),
                new Transaction(raoul, 2011, 400),
                new Transaction(mario, 2012, 710),
                new Transaction(mario, 2012, 700),
                new Transaction(alan, 2012, 950)
        );


        // Query 1: Find all transactions from year 2011 and sort them by value (small to high).
        //
        List<Transaction> tr2011 = transactions.stream()
                .filter(transaction -> transaction.getYear() == 2011)
                .sorted(comparing(Transaction::getValue))
                .collect(toList());
        System.out.println(tr2011);

        // Query 2: What are all the unique cities where the traders work?
        List<String> cities =
                transactions.stream()
                        .map(transaction -> transaction.getTrader().getCity())
                        .distinct()
                        .collect(toList());
        System.out.println(cities);

        // Query 3: Find all traders from Cambridge and sort them by name.

        List<Trader> traders =
                transactions.stream()
                        .map(Transaction::getTrader)
                        .filter(trader -> trader.getCity().equals("Cambridge"))
                        .distinct()
                        .sorted(comparing(Trader::getName))
                        .collect(toList());
        System.out.println(traders);


        // Query 4: Return a string of all traders’ names sorted alphabetically.

        String traderStr =
                transactions.stream()
                        .map(transaction -> transaction.getTrader().getName())
                        .distinct()
                        .sorted()
                        .reduce("", (n1, n2) -> n1 + n2);
        System.out.println(traderStr);

        // Query 5: Are there any trader based in Milan?

        boolean milanBased =
                transactions.stream()
                        .anyMatch(transaction -> transaction.getTrader()
                                .getCity()
                                .equals("Milan")
                        );
        System.out.println(milanBased);


        // Query 6: Update all transactions so that the traders from Milan are set to Cambridge.
        transactions.stream()
                .map(Transaction::getTrader)
                .filter(trader -> trader.getCity().equals("Milan"))
                .forEach(trader -> trader.setCity("Cambridge"));
        System.out.println(transactions);


        // Query 7: What's the highest value in all the transactions?
        int highestValue =
                transactions.stream()
                        .map(Transaction::getValue)
                        .reduce(0, Integer::max);
        System.out.println(highestValue);
    }
}
```





1. 





