# Java 枚举工具

|    date    | tag  |
|    ---     | ---  |
| 2020-12-31 | enum |

## 定义接口

```java
//: BaseEnum.java
public interface BaseEnum<K, V> {

    K getKey();

    V getValue();
}
//:~
```

## 工具

```java
//: BaseEnums.java
public class BaseEnums {

    private static final Map<String, Map> CACHE = new ConcurrentHashMap<>();

    public static <K, V, T extends Enum<T> & BaseEnum<K, V>> V getValueByKey(Class<T> childClass, K key) {
        return Optional.ofNullable(getByKey(childClass, key)).map(BaseEnum::getValue).orElse(null);
    }

    public static <K, V, T extends Enum<T> & BaseEnum<K, V>> T getByKey(Class<T> childClass, K key) {
        return Optional.ofNullable(getMap(childClass)).map(map -> map.get(key)).orElse(null);
    }

    @SuppressWarnings("unchecked")
    private static <K, V, T extends Enum<T> & BaseEnum<K, V>> Map<K, T> getMap(Class<T> childClass) {
        return CACHE.computeIfAbsent(childClass.getName(),
                key -> Stream.of(childClass.getEnumConstants())
                        .collect(Collectors.toMap(BaseEnum::getKey, Function.identity()))
        );
    }
}
//:~
```

## 用法

```java
//: CustomEnum.java
public enum CustomEnum implements BaseEnum<Integer, String>{

    CUSTOM(0, "");

    private final Integer code;
    private final String value;

    CustomEnum(Integer code, String value) {
        this.code = code;
        this.value = value;
    }

    @Override
    public Integer getKey() {
        return this.code;
    }

    @Override
    public String getValue() {
        return this.value;
    }
}
//:~
```

```java
String value = BaseEnums.getValueByKey(CustomEnum.class, 0);
```

## orign

```java
//: BaseKV.java
public interface BaseKV<K, V> {

    K getKey();

    V getValue();

    static <K, V, Child extends Enum<Child> & BaseKV<K, V>> V getByKey(Class<Child> childClass, K key) {
        return Optional.ofNullable(Cache.getMap(childClass).get(key)).map(BaseKV::getValue).orElse(null);
    }

    class Cache {
        private Cache() {}
        private static Map<Class, Map> mapCaChe = new ConcurrentHashMap<>();
        static <K, V, Child extends Enum<Child> & BaseKV<K, V>> Map<K, Child> getMap(Class<Child> childClass) {
            return mapCaChe.computeIfAbsent(childClass, key -> Stream.of(((Class<Child>) key).getEnumConstants())
                    .collect(Collectors.toMap(BaseKV::getKey, Function.identity())));
        }
    }
}
//:~
```
