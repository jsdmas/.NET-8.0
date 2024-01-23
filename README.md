# Blazor Tutorial

## dotnet install

Apple M1 칩이 있는 Mac에 있는 경우 ARM64 버전의 SDK를 설치해야 합니다.

```
설치확인
~$ dotnet --version
```

## 앱 만들기

```
~$ dotnet new blazor -o BlazorApp
```

이 명령은 새 Blazor 웹앱 프로젝트를 만들고 현재 위치 내의 BlazorApp라는 새 디렉터리에 배치합니다.

### files

- `Program.cs`는 서버를 시작하고 앱 서비스와 미들웨어를 구성하는 앱의 진입점입니다.
- `App.razor`는 앱의 루트 구성 요소입니다.
- `Routes.razor`는 Blazor 라우터를 구성합니다.
- `Components/Pages` 디렉터리에는 앱에 대한 몇 가지 예제 웹 페이지가 포함되어 있습니다.
- `BlazorApp.csproj`는 앱 프로젝트와 해당 종속성을 정의합니다.
- `Properties` 디렉터리 내의 launchSettings.json 파일은 로컬 개발 환경에 대한 다양한 프로필 설정을 정의합니다. 포트 번호는 프로젝트 생성 시 자동으로 할당되어 이 파일에 저장됩니다.

> Razor 구성 요소 파일 이름에는 첫 글자를 대문자로 사용해야 합니다.

## 앱 실행

```
~$ dotnet watch
```

dotnet watch 명령은 앱을 빌드 및 시작한 다음 코드를 변경할 때마다 앱을 업데이트합니다. Ctrl+C를 선택하여 언제든지 앱을 중지할 수 있습니다.

앱에 `http://localhost:<port number>`에서 수신 대기 중이라고 표시되고 해당 주소에서 브라우저가 시작될 때까지 기다립니다.

## 카운터 사용해 보기

```c#
@page "/counter"
@rendermode InteractiveServer

<PageTitle>Counter</PageTitle>

<h1>Counter</h1>

<p role="status">Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

맨 위의 `@page` 지시문에 지정된 대로 브라우저에서 /counter를 요청하면 Counter 구성 요소가 해당 콘텐츠를 렌더링합니다.

- [클릭] 버튼을 선택할 때마다:
  - onclick 이벤트가 발생합니다.
  - IncrementCount 메서드가 호출됩니다.
  - currentCount가 증가합니다.
  - 업데이트된 수를 표시하기 위해 구성 요소가 렌더링됩니다.

## 구성 요소 수정

구성 요소 매개 변수는 특성 또는 자식 콘텐츠를 사용하여 지정되며, 이를 통해 하위 구성 요소에 대한 속성을 설정할 수 있습니다.  
Counter 구성 요소에 매개 변수를 정의하여 버튼을 클릭할 때마다 증가하는 양을 지정합니다.

- [Parameter] 특성을 사용하여 IncrementAmount에 대한 공개 속성을 추가합니다.
- currentCount 값을 증가시킬 때 IncrementAmount를 사용하도록 IncrementCount 메서드를 변경합니다.

```C#
@* Counter.razor *@

@code {
    private int currentCount = 0;

    [Parameter]
    public int IncrementAmount{get;set;} = 1;

    private void IncrementCount()
    {
        currentCount+= IncrementAmount;
    }
}
```

```C#
@* Home.razor *@
<Counter IncrementAmount="10" />
```

# rendermode

구성 요소의 렌더링 모드를 지정하는 데 사용됩니다.  
렌더링 모드는 구성 요소가 호스팅되는 모델, 렌더링되는 위치 및 상호 작용 여부를 결정합니다.  
[https://learn.microsoft.com/ko-kr/aspnet/core/blazor/components/render-modes?view=aspnetcore-8.0](https://learn.microsoft.com/ko-kr/aspnet/core/blazor/components/render-modes?view=aspnetcore-8.0)

## InteractiveServer

대화형 SSR은 서버에서 구성 요소를 렌더링한 다음 클라이언트에 HTML을 전송하는 방식입니다.  
이 모드를 사용하면 웹 페이지가 처음 로드될 때 콘텐츠를 볼 수 있습니다.

# @inherits LayoutComponentBase

현재 구성 요소가 레이아웃 구성 요소임을 지정하는 특수 지시어입니다.  
레이아웃 구성 요소는 Blazor 웹 앱의 공통 구조와 UI 요소를 정의하는 구성 요소입니다.  
다른 구성 요소를 레이아웃 내부에 중첩하여 앱의 전체적인 레이아웃을 구성할 수 있습니다.

## 주요 역할

- 공통 UI 요소 정의: 헤더, 탐색 모음, 바닥글 등 여러 페이지에서 반복적으로 사용되는 UI 요소를 한곳에 정의하여 코드 중복을 줄입니다.
- 일관된 레이아웃 제공: 모든 페이지에서 동일한 구조와 디자인을 유지하여 앱의 통일성을 보장합니다.
- 중첩된 구성 요소 관리: 레이아웃 구성 요소는 다른 구성 요소를 중첩하여 앱의 페이지 구조를 구성합니다.

## 사용 방법

- 레이아웃 구성 요소 생성: .razor 파일을 만들고 @inherits LayoutComponentBase 지시어를 추가합니다.
- 공통 UI 요소 배치: 레이아웃 구성 요소의 Razor 템플릿에 공통적으로 사용할 UI 요소를 배치합니다. 예: 헤더, 탐색 모음, 바닥글 등
- 중첩 지점 지정: 다른 구성 요소를 삽입할 위치에 @Body 지시어를 배치합니다.
- 레이아웃 적용: 다른 구성 요소에서 @layout 지시어를 사용하여 생성한 레이아웃 구성 요소를 적용합니다.

```C#
// MainLayout.razor (레이아웃 구성 요소)
@inherits LayoutComponentBase

<div class="container">
    <header>@HeaderContent</header>
    <main>
        @Body
    </main>
    <footer>@FooterContent</footer>
</div>

```

```C#
// Page1.razor (Page1 구성 요소)
@page "/"
@layout MainLayout

<h1>페이지 1의 콘텐츠</h1>

```

위 예시에서 Page1 구성 요소는 MainLayout 레이아웃 구성 요소를 사용하며, Page1의 콘텐츠는 MainLayout의 @Body 위치에 삽입됩니다.

# Reference

- [https://dotnet.microsoft.com/ko-kr/learn/aspnet/blazor-tutorial/intro](https://dotnet.microsoft.com/ko-kr/learn/aspnet/blazor-tutorial/intro)
- [https://learn.microsoft.com/ko-kr/training/modules/build-blazor-webassembly-visual-studio-code/7-exercise-razor-binding?pivots=vscode](https://learn.microsoft.com/ko-kr/training/modules/build-blazor-webassembly-visual-studio-code/7-exercise-razor-binding?pivots=vscode)
