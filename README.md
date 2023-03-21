# 학습시작기간 2023.03.15

# 목차
- [스프링 MVC - 웹 페이지 만들기](#스프링-MVC---웹-페이지-만들기)

## 스프링 MVC - 웹 페이지 만들기

### 타임리프 간단히 알아보기

#### 타임리프 사용 선언
```html
<html xmlns:th="http://www.thymeleaf.org">
```

#### 타임리프 핵심
- 핵심은 th:xxx 가 붙은 부분은 서버사이드에서 렌더링 되고, 기존 것을 대체한다. th:xxx 이 없으면 기존html의 xxx 속성이 그대로 사용된다.
- HTML을 파일로 직접 열었을 때, th:xxx 가 있어도 웹 브라우저는 th: 속성을 알지 못하므로 무시한다.
- 따라서 HTML을 파일 보기를 유지하면서 템플릿 기능도 할 수 있다.

#### URL 링크 표현식 - @{...},
```html
th:href="@{/css/bootstrap.min.css}"
```
- @{...} : 타임리프는 URL 링크를 사용하는 경우 @{...} 를 사용한다. 이것을 URL 링크 표현식이라 한다.
- URL 링크 표현식을 사용하면 서블릿 컨텍스트를 자동으로 포함한다.

#### 속성 변경 - th:onclick
```
th:onclick="|location.href='@{/basic/items/add}'|"
```

#### 리터럴 대체 - |...|
- 타임리프에서 문자와 표현식 등은 분리되어 있기 때문에 더해서 사용해야 한다.
  - <span th:text="'Welcome to our application, ' + ${user.name} + '!'">
- 다음과 같이 리터럴 대체 문법을 사용하면, 더하기 없이 편리하게 사용할 수 있다.
  - <span th:text="|Welcome to our application, ${user.name}!|">

#### 반복 출력 - th:each
```
<tr th:each="item : ${items}">
```
- 반복은 th:each 를 사용한다. 이렇게 하면 모델에 포함된 items 컬렉션 데이터가 item 변수에 하나씩 포함되고, 반복문 안에서 item 변수를 사용할 수 있다.

#### 변수 표현식 - ${...}
```
<td th:text="${item.price}">10000</td>
```
- 모델에 포함된 값이나, 타임리프 변수로 선언한 값을 조회할 수 있다.
- 프로퍼티 접근법을 사용한다. ( item.getPrice() )

#### 내용 변경 - th:text
```
<td th:text="${item.price}">10000</td>
```
- 내용의 값을 th:text 의 값으로 변경한다.
- 여기서는 10000을 ${item.price} 의 값으로 변경한다.

#### URL 링크 표현식2 - @{...},
```
th:href="@{/basic/items/{itemId}(itemId=${item.id})}"
```

#### 속성 변경 - th:value
```
th:value="${item.id}"
```
- 모델에 있는 item 정보를 획득하고 프로퍼티 접근법으로 출력한다. ( item.getId() )
- value 속성을 th:value 속성으로 변경한다.

#### 속성 변경 - th:action
- th:action
- HTML form에서 action 에 값이 없으면 현재 URL에 데이터를 전송한다.

### @ModelAttribute
- @ModelAttribute 는 Item 객체를 생성하고, 요청 파라미터의 값을 프로퍼티 접근법(setXxx)으로 입력해준다.
- 모델(Model)에 @ModelAttribute 로 지정한 객체를 자동으로 넣어준다.

모델에 데이터를 담을 때는 이름이 필요하다. 이름은 @ModelAttribute 에 지정한 name(value) 속성을 사용한다. 만약 다음과 같이 @ModelAttribute 의 이름을 다르게 지정하면 다른 이름으로 모델에 포함된다.
- @ModelAttribute("hello") Item item 이름을 hello 로 지정
- model.addAttribute("hello", item); 모델에 hello 이름으로 저장

@ModelAttribute 의 이름을 생략하면 모델에 저장될 때 클래스명을 사용한다. 이때 클래스의 첫글자만
소문자로 변경해서 등록한다.

@ModelAttribute 자체도 생략가능하다. 대상 객체는 모델에 자동 등록된다. 나머지 사항은 기존과동일하다

```java
    //@PostMapping("/add")
    public String addItemV2(@ModelAttribute("item") Item item){
        itemRepository.save(item);
        return "basic/item";
    }

    //@PostMapping("/add")
    public String addItemV3(@ModelAttribute Item item){
        itemRepository.save(item);
        return "basic/item";
    }
    
    //@PostMapping("/add")
    public String addItemV4(Item item){
        itemRepository.save(item);
        return "basic/item";
    }
```

### 리다이렉트
- 스프링은 redirect:/... 으로 편리하게 리다이렉트를 지원한다.
- 컨트롤러에 매핑된 @PathVariable 의 값은 redirect 에도 사용 할 수 있다.
  - redirect:/basic/items/{itemId} {itemId} 는 @PathVariable Long itemId 의 값을 그대로 사용한다.

### PRG Post/Redirect/Get 
브라우저의 새로 고침은 마지막에 서버에 전송한 데이터를 다시 전송한다.

#### 상품 저장 후에 뷰 템플릿으로 이동하는 구조
![](https://velog.velcdn.com/images/gcael/post/69ddfc8a-0f96-4d72-8c04-aea30ac2bb42/image.PNG)

#### 상품 저장 후에 리다이렉트로 이동하는 구조
![](https://velog.velcdn.com/images/gcael/post/b622f6b0-0c50-48c8-8564-7816cbf8539a/image.PNG)

 
_참고 문서 및 링크_
- 스프링 MVC 1편 - 백엔드 웹 개발 핵심 기술(김영한)
