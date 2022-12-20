---
layout: post
title:  "start to read gson"
date:   2022-12-20 22:34:01 +0900
categories: gson
---
<br>
Tables of contents
1. `git clone gson`
2. open in IntelliJ
3. solve red underline, error
4. pick a starting class
5. constructor, fromJson()

---
### 1. git clone gson

```shell
git clone https://github.com/google/gson.git
```
- 난 google gson repository 를 fork 하고 내 repo 를 git clone 했다.
- 코드를 읽다 혹시 contribution 할 거리를 발견할 경우를 대비하기 위함이었다.

### 2. open in IntelliJ
![open-in-intellij](https://user-images.githubusercontent.com/120941926/208683382-c37faef4-9ed9-4d02-8b2b-0a674c53eee8.png)
- clone 받은 폴더를 open 한다.
- 멀티모듈 구조로 되어 있기 때문에 각 모듈이 아니라 전체 폴더를 open 해야 한다.

### 3. solve red underline, error
![start-to-read-gson-2-error](https://user-images.githubusercontent.com/120941926/208683848-f1899463-a372-4976-9f43-bbb57714f596.png)
- gson 모듈을 보면 import 문을 비롯한 몇 군데에 빨간 줄이 보인다.
- `import com.google.gson.internal.GsonBuildConfig;` 

<br>
[problem]
- GsonBuildConfig 클래스는 java-templates 폴더 하위에 있다.
- 분명 클래스 파일이 존재하는데 왜 못 찾을까?
- IntelliJ 에서 해당 디렉토리를 소스 루트로 인식하지 않기 때문인 듯하다.
- (gson 무한신뢰 및 경험으로 코드가 잘못되었을 것이라는 생각은 하지 않았음)

<br>
[how to solve]
- IntelliJ 가 해당 디렉토리를 소스 루트로 인식할 수 있도록 도와줘야 한다.
- java-templates 폴더 우클릭, Mark Directory as - Sources Root
- 이렇게 하면 java-templates 폴더 아이콘 색이 바뀌면서 빨간 줄이 없어진다.

### 4. pick a starting class
- gson 코드에는 4개의 모듈이 존재했다. <br>
  1) extras <br>
  2) gson <br>
  3) metrics <br>
  4) proto <br>
- 이름이 눈에 익은 proto 모듈부터 보고 싶었지만, gson의 시작점을 찾고 싶었다.
- gson 모듈에 들어가 Gson 클래스를 열었다.
- `This is the main class for using Gson.`
- 그렇지, 모든 코드의 시작은 main class. 시작점 찾았다!

### 5. constructor, fromJson()
- 생성자로 시작해볼까?
```
public Gson() {
  this(Excluder.DEFAULT, DEFAULT_FIELD_NAMING_STRATEGY,
          Collections.<Type, InstanceCreator<?>>emptyMap(), DEFAULT_SERIALIZE_NULLS,
          DEFAULT_COMPLEX_MAP_KEYS, DEFAULT_JSON_NON_EXECUTABLE, DEFAULT_ESCAPE_HTML,
          DEFAULT_PRETTY_PRINT, DEFAULT_LENIENT, DEFAULT_SPECIALIZE_FLOAT_VALUES,
          DEFAULT_USE_JDK_UNSAFE,
          LongSerializationPolicy.DEFAULT, DEFAULT_DATE_PATTERN, DateFormat.DEFAULT, DateFormat.DEFAULT,
          Collections.<TypeAdapterFactory>emptyList(), Collections.<TypeAdapterFactory>emptyList(),
          Collections.<TypeAdapterFactory>emptyList(), DEFAULT_OBJECT_TO_NUMBER_STRATEGY,
          DEFAULT_NUMBER_TO_NUMBER_STRATEGY,
          Collections.<ReflectionAccessFilter>emptyList());
}
```
- 예상은 했지만 변수들이 너무너무 많다.
- 변수의 의미를 파악하기보다는 눈에 익은 메서드부터 찾아가자.
- gson을 사용하면서 수도 없이 봤고 이용했던 fromJson() 을 찾았다!
```
public <T> T fromJson(String json, Class<T> classOfT) throws JsonSyntaxException {
        T object = fromJson(json, TypeToken.get(classOfT));
        return Primitives.wrap(classOfT).cast(object);
        }
```
- fromJson 메서드가 오버라이딩된 것이 보인다.
- 타고 타고 들어가보니 한두 개가 아니다!
- 역시 너무 매력적인 코드다.
- 하지만 관계를 파악하려면 시간을 들여야 하므로 오늘의 코드 한줄 읽기는 여기서 끝!