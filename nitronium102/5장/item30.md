### 5장 : 제네릭
# 🍀 [아이템 30]  이왕이면 제네릭 메서드로 만들라

## 📒 제네릭 메서드 작성법
```java
public static <E/*타입 매개변수 목록*/> Set<E/*반환 타입*/> union(Set<E/*파라미터 타입*/> s1, Set<E> s2) {
    Set<E> result = new HashSet<>(s1);
    result.addAll(s2);
    return result;
}
```

### 활용 예시
```java
@Test
public void unionTest() {
    Set<String> guys = Set.of("톰", "딕", "헤리");
    Set<String> stooges = Set.of("래리", "모에", "컬리");
    Set<String> aflCio = union(guys, stooges);
    System.out.println("aflCio = " + aflCio);
}
```
- `Set<String>` 타입 2개를 합침
- 입력 2개와 반한 1개 타입이 모두 일치
- 한정적 와일드카드 타입을 사용하여 더 유연하게 개선 가능

## 📒 제네릭 싱글턴 팩토리
> 제네릭은 런타임에 타입 정보가 소거되므로 하나의 객체를 어떤 타입으로든 매개변수화 할 수 있다.

- 불변 객체를 여러 타입으로 이용할 수 있게 만들 수 있음
- 요청한 타입 매개 변수에 맞게 그 객체의 타입을 바꿔주는 정적 팩터리를 만들어야 한다
ex) Collections.reverseOrder, Collections.emptySet
```java
public static final <T> Set<T> emptySet() {
    return (Set<T>) EMPTY_SET;
}
```
- emptySet 메서드는 빈 집합(Set)을 생성하여 반환하는데, 제네릭으로 작성되어 어떤 타입의 요소를 가진 집합을 생성할지 타입 매개 변수로 받는다. 
- 이렇게 하면 코드를 호출하는 시점에 원하는 타입의 빈 집합을 얻을 수 있다.
```java
Set<Integer> intSet = Collections.emptySet(); // 빈 정수형 집합 생성
Set<String> strSet = Collections.emptySet();  // 빈 문자열 집합 생성
```

&nbsp;

## 📒 재귀적 타입한정 이용하기
> 자기 자신이 들어간 표현식을 사용하여 타입 매개변수의 허용 범위를 한정
- 타입의 자연적 순서를 정하는 Comparable 인터페이스와 함께 사용됨

```java
public static <E extends Comparable<E>> E max(Collection<E> c);
```
- `E`로 받을 타입은 오직 `Comparable<E>`를 구현한 타입만 가능
- 즉 `Comparable`을 구현한 타입만 가능
=> max 메서드를 호출할 때 컴파일러는 E 타입이 Comparable<E> 인터페이스를 구현하는 타입으로 제한되어 있기 때문에 안전하게 사용할 수 있다. 

```java
List<Integer> numbers = Arrays.asList(3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5);
Integer maxNumber = max(numbers);
```
이렇게 max 메서드를 호출하면 컴파일러가 타입 매개변수 E를 Comparable<E> 인터페이스를 구현한 Integer로 한정하고, 최댓값을 찾을 수 있다.


### 정리
클라이언트에서 입력 매개변수 및 반환값을 명시적으로 형변환하는 메서드보다 제네릭 메서드가 더 안전하고 사용하기도 쉽다.
형 변환은 런타임 시에 에러를 동반하기 쉬우므로 제네릭을 사용하자.