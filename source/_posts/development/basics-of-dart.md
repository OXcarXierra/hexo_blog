---
title: Dart 기초 문법
tags: []
categories: [Development]
permalink: ''
thumbnailImage: ''
thumbnailImagePosition: right
date: 2022-10-07 22:13:45
---

<!-- excerpt -->

<!-- toc -->

## 변수형

- 정수형 Number
- 문자열 String
  문자열 내 변수 value - ‘$value’

- 불리안 bool

- true, false (소문자임에 주의!)

- 타입 미지정 var, dynamic

dynamic 타입으로 선언되면 할당되는 변수에 따라 타입이 바뀐다.

## 리스트 List &lt;type&gt;

- growable list : 길이가 무한히 늘어날 수 있다.
- ungrowable list의 선언 :
  ```dart
  List<String> list = new List(n);
  List list_2 = new List.from(['a','b'])
  ```
- 길이는 list.length

## Map 해쉬

```dart
Map<String, type> dictionary = {
	'key':'value',
	...
}
dictionary.keys.toList()

```

- final, const - 한 번만 선언하도록 설정, 차이점은 const는 compile time에 할당이 되어있어야 하고, final은 runtime에 지정
- Enum - 커스텀 변수형

```dart
enum Status = {approved, rejected, pending}
```

- typedef 함수를 변수처럼 사용가능하게 함.

## 연산자

??= : null인 경우 할당

/ 계산하면 double형으로 할당

is <type> , !is <type>

## 클래스

class변수는 instance라고 부른다.

constructor의 이름은 변수이름과 같다.

- Constructor의 선언

  ```dart
  class ClassName{
  	String name;
  	String id;

  	ClassName({
  		String name, String id,
  	}) : this.name = name, this.id = id;
  }
  ```

- Named Constructor
  ```dart
  //선언방법
  ClassName.constructorName() :
  ```
- class의 Getter와 setter
  class 내에서 선언하는 변수들은 \_name 언더스코어로 시작. private variable이라고 부른다.
  private variable은 외부에서 접근 불가. 그래서 getter, setter을 선언해준다

  ```dart
  class Idol{
  	String _name;
  	String _group;

  	get name{
  		return this._name;
  	}

  	set name(String newname){
  		this._name = newname;
  	}
  }

  ```

### 클래스의 상속

```dart
class ChildClass extends ParentClass{
	ChildClass(

	): super( // super은 부모클래스의 인스턴스를 지칭.

	);
}
```

부모 클래스의 인스턴스, 메소드 모두를 상속받음. 부모는 자식의 인스턴스와 자식을 상속받지 않음.

부모 클래스는 두개 이상이 될 수 없다. 하나의 클래스의 자식 클래스는 여러개가 될 수 있다.

### 메소드 오버라이딩

자식클래스에서 메소드를 다시 작성

```dart
@override // decorator
//이름은 같으면서 새로 쓴 메소드
```

### static 키워드

하나의 클래스 내에 공유되는 변수가 있다면 `static`으로 선언해준다.

### Interface 키워드

```dart
class IdolInterface{
	String name;
	void sayName(){}
}

class BoyGroup implements IdolInterface{
	String name;
	void sayName(){
		//something
	}
}
```

클래스의 형식을 지정해주는 인터페이스

### Cascade Operator

```dart
class Idol{
	String name;
	String group;
	void sayName(){};
	void sayGroup(){};
}

Idol idol = new Idol('이름','그룹');
idol.sayName();
idol.sayGroup();

new Idol('이름','그룹')
	..sayname()
	..sayGroup();
```
