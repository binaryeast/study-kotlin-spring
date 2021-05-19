# Kotlin 강좌

## 2주차 - 7~12강 수강 및 내용 정리
***
## 7강. 흐름제어와 논리연산자
### 흐름제어

    return : 함수 종료, 값 반환
    break : 반복 종료하고 나감
    continue : 현재 반복 중단 후 다음 반복 실행
```Kotlin
코틀린에서 추가된 기능
// -break나 continue가 적용된 반복문을 label을 통해 지정가능!

loop@for (i in 1..10) {
    for (j in 1..10) {
        if(i == 1 & j == 2) break@loop
    }
}
//loop(레이블 이름)으로 감싼 반복문이 모두 break 됨!
```
### 논리 연산자
* &&, ||, ! 등... 자바와 동일

***
## 8강. 클래스의 기본 구조
클래스는 '값'과 그 값을 사용하는 '기능'을 묶어둔 것.

    속성(고유의 특징값) + 함수(기능의 구현)

내가 배웠던 자바강의에서의 필드 + 메소드 와 비슷.
```Kotlin
fun main() {

    var a = Person("박보영", 1990)
    var b = ...

    println ("안녕하세요, ${a.birthYear}년생 ${a.name}입니다")
} 
// .은 참조연산자, a.birthYear은 a객체의 birthYear이라는 속성(필드)에 접근하는 것

//${}은 ""안에서 문자로 오인하지 않도록 전체를 감싼 것. $는 마치 %d와 비슷한 느낌


class Person (var name:String, val birthYear:Int) 
//()안에 필드값을 넣어줌. 
//()가 없고 {}안에 변수처럼 적어줬던 자바와 다른 차이.



또는 

a.introduce()
b.introduce() ...

class Person (var name:String, val birthYear:Int) {
    fun introduce() {
        println ("안녕하세요, ${birthYear}년생 ${name}입니다")
    }
}

처럼 변수 안 메소드로 구현해주어도 된다. 이 때 필드는 ()안에, 메소드는 {}안에 들어간다는 것을 주의하자!
```
***
## 9강. 클래스의 생성자
### 생성자란? 
* 새로운 객체(인스턴스)를 만들기 위해 사용하는 특수한 함수
    
        기능
        - 인스턴스의 속성 초기화
        - 인스턴스 생성시 구문 수행 (init 통해 가능)
### init 함수란? 
* 파라미터 / 반환형이 없는 특수한 함수.
* 생성자를 통해 객체(인스턴스)가 만들어질 때 호출 됨!
```Kotlin
class Person (var name:String, val birthYear:Int) {
    init {
        println ("${this.birthYear}년생 ${this.name}님이 생성되었습니다")
    }
    //this : 클래스 내부에서 쓰이는, 자신을 가리키는 키워드
}
```
### 기본 생성자? 
* 클래스를 만들 때 기본으로 선언
* 위 예제의 (var name:String, val birthYear:Int)이 기본 생성자이다.
### 보조 생성자? - constructor()
* 필요에 따라 추가적으로 선언
* 인스턴스 생성시 편의 제공 & 추가적인 구문 수행
```Kotlin
...
    constructor(name:String) : this(name, 1997) {
        // 현재 (name:String)으로 이름만 입력받는 것을 볼 수 있음.
        // : this()는 보조생성자가 기본생성자를 호출하도록 하는 것
    }
}


위 생성자는 ()안에 있는 name만 입력받은 후, 받은 name / 1997을 : this()를 통해 기본 생성자로 넘겨주게 된다.

기본 생성자는 넘겨받은 내용을 토대로 객체를 만들며, 이 객체는 1997이 고정되있음을 알 수 있다!
```


### 이런 생성자들은 다양한 인스턴스 초기화 방법을 제시해 '편의'를 제공하는 기능일 뿐임!
***
## 10강. 클래스의 상속
### 상속이 필요한 경우?
  1. 이미 존재하는 클래스를 확장하여 새로운 기능을 추가한 클래스를 만들려 할 때
  2. 여러 클래스 중 공통점을 뽑아 코드관리를 편하게 하고싶을 때

    * 물려주는 쪽 : 슈퍼 클래스 
    * 물려받는 쪽 : 서브 클래스
  
### 예시
```Kotlin
fun main() {

    var a = Animal("별이", 5, "개")
    var b = Dog("별이", 5)  //"개"를 넣어주지 않아도 기본값으로 지정되있음! 즉 위 코드와 결과 동일.

    a.introduce()
    b.introduce()

    b.bark()

    var c = Cat("루이", 1)

    c.introduce()
    c.meow()
}

//슈퍼(부모)클래스. class 앞에 open 키워드 붙여주기 (코틀린은 상속금지가 기본값)
open class Animal (var name:String, var age:Int, var type:String)
{
    fun introduce() {
        println("저는 ${type} ${name}이고, ${age}살 입니다.")
    }
}

//서브(자식)클래스
class Dog (name:String, age:Int) : Animal(name, age, "개")
{
    fun bark() {
        println("멍멍")
    }
}

//서브(자식)클래스
class Cat (name:String, age:Int) : Animal(name, age,"고양이")
{
    fun meow(){
        println("야옹")
    }
}
```
### 상속의 규칙
1. 서브 클래스는 슈퍼 클래스에 존재하는 속성과 '같은 이름'의 속성을 가질 수 X
2. 서브 클래스 생성 시 반드시 슈퍼클래스의 생성자까지 호출해야 함.

```Kotlin
class Dog (name:String, age:Int) : Animal(name, age, "개")
{
    ...
}


이 코드에서 (var name:Sring...)처럼 앞에 var을 안 붙이는 이유?
    - 슈퍼클래스 Animal에서 정의했던 속성인 var name:String와 겹치기 때문이다.
    - 즉 생성자에서 이름과 나이를 받긴 하지만, 클래스 자체 속성으로 만들어주는 var을 붙이지말고, 일반 패러미터로 받아 Animal 클래스의 생성자에 넘겨주어야 함.
    (var, val 등을 붙이면 속성으로 선언됨!)


슈퍼클래스의 생성자에 넘겨주는 방법?
    클래스 : 슈퍼클래스이름 (넘길내용)

    : Animal (name, age, "개")  // name과 age는 (name:String, age:Int)에서 입력받은 값, 그리고 type에 "개"라는 내용이 들어가게 됨.
```
***
## 11강. 오버라이딩과 추상화
### 오버라이딩
* 이미 구현이 끝난 함수의 기능을 서브클래스에서 변경(재정의)해주는 것.
```Kotlin
fun main(){

    var t = Tiger()

    t.eat()

}

open class Animal(){
    open fun eat(){ //open을 붙여 오버라이딩 허용
        println("음식을 먹습니다")
    }
}

class Tiger(){
    override fun eat(){ //override 키워드를 붙임
        println("고기를 먹습니다")
    }
}

//실행하면 '고기를 먹습니다' 가 출력된다!
```
### 추상화
* 형식만 선언하고 실제구현은 서브클래스에 일임할 때 사용.

        구성
        * 선언부만 있고 기능이 구현되지 않은 '추상함수'
        * 추상함수를 포함하는 '추상클래스'
```Kotlin
abstract class Animal { //추상클래스. abstract를 붙인다
    abstract fun eat() //추상함수. abstract를 붙인다. 비어있는 껍데기라고 생각
}

ㅡㅡㅡㅡㅡㅡㅡㅡㅡ
추상클래스는 미완성 클래스이기 때문에 반드시 서브클래스에서 상속을 받아 abstract 표시가 된 함수들을 구현해야 함.


class Rabbit : Animal() { //Animal 클래스 상속
    override fun eat() { //오버라이딩을 추상함수 재구현
        println("당근을 먹습니다")
    }
}
```
### 인터페이스
* 서로 다른 기능들을 여러개 물려줘야할 때 사용.

        - 코틀린에서는 인터페이스가 추상함수 외에도 (필드, 메소드)를 가질 수 있음!
        - 추상함수는 생성자를 가질 수 있는 반면, 인터페이스는 생성자를 가질 수 X
        - 한 번에 여러 인터페이스 상속가능! 
            (ex. 인터페이스 A와 B의 기능을 모두 상속받음)


        구현부가 있는 함수 -> open 함수로 간주
        구현부가 없는 함수 -> abstract 함수로 간주
        => 별도의 키워드가 없어도 포함된 모든 함수를 서브클래스에서 구현 / 재정의 가능!

```Kotlin
fun main(){

    var d = Dog

    d.eat()
    d.run()

}

interface Runner {  //interface 키워드 붙이기
    fun run()   //abstract을 안 적어줘도 됨
}

interface Eater {
    fun eat() { //open을 안 적어줘도 됨
        println("음식을 먹습니다")
    }
}

class Dog : Runner, Eater { //여러개의 상속은 ,로 구분!
    override fun run() {    //그러나 override는 까먹지 말기!
        println("우다다다 뜁니다")
    }

    override fun eat() {
        println("허겁지겁 먹습니다")
    }
}
```
***
## 12강. 기본 프로젝트 구조
우리가 코틀린으로 코딩을 할 때, '프로젝트'라는 구조로 작업하게 됨. 
***
## 물리적 구조
* 프로젝트, 모듈, 폴더 및 파일이 실제 파일 시스템에 기반한 물리적인 구조를 담당!
### 프로젝트?
* 코틀린으로 프로그램을 짤 때 관련한 모든 내용을 담는 '큰 틀'

### 모듈?
* 하나의 프로젝트는 여러개의 모듈로 구성됨.
* 모듈은 직접 만들수도, 이미 구현된 '라이브러리 모듈'을 가져와 붙일 수도 있음.

        모듈 안?
            다수의 폴더, 파일 존재!
                - 코틀린 코드파일(.kt)
                - 모듈과 관련된 설정, 리소스 파일...
***
## 논리적 구조
### 패키지?
* 개발 시 소스 코드의 '소속'을 지정하기 위한 논리적 단위
* 코드 내 이름이 용도에 따라 충돌하지 않도록 유니크한 패키지 이름을 짓는 것이 중요!

        형식(추천!)
        서버 도메인 거꾸로 + 프로젝트 명 + 기능별 세분화..
            ex. com.youtube.dimo
                com.youtube.dimo.kotlin
                com.youtube.dimo.talk
                ...

### 어떻게 패키지로 묶나요?
```Kotlin
package com.youtube.dimo 
//코드 파일 맨 윗줄에 'package'를 적고, 패키지 이름 적기
//패키지를 명시하지 않으면 자동으로 'default' 패키지로 묶임
```
* 코틀린은 자바와 달리 폴더 구조와 패키지 명을 일치시키지 않아도 OK!
* 단순히 파일 상단에 package ... 을 명시해주면 컴파일러가 알아서 처리
* 같은 패키지 내에서는 변수/함수/클래스 공유 가능!

        BUT 패키지가 다르면 공유할 수 없다...

### 패키지가 다를 때 변수/함수/클래스 공유하는법? 
* 패키지 선언 바로 아래에 'import'를 쓰고, 사용할 외부 패키지 이름을 작성
```kotlin
package com.youtube.dimo 

import com.youtube.dimo.base
//com.youtube.dimo.base패키지의 변수,함수,클래스 등을 그대로 사용가능!

//이때 두 패키지에 동일한 이름의 클래스 A가 있다?
// => package com.youtube.dimo.base.A 처럼 패키지 명을 포함한 풀네임 작성
```
* 코틀린은 클래스명과 파일명이 일치 안해도 됨!
* 하나의 파일에 '여러개의 클래스'를 넣어도 알아서 컴파일!

이는 파일이나 폴더로 구분하지 않고, 파일내에 있는 package 키워드를 기준으로 구분하기 때문!