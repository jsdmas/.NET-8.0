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
`@rendermode` 지시문은 구성 요소에 대해 대화형 서버 렌더링을 사용하도록 설정하여 브라우저에서 사용자 인터페이스 이벤트를 처리할 수 있도록 합니다.

- [클릭] 버튼을 선택할 때마다:
  - onclick 이벤트가 발생합니다.
  - IncrementCount 메서드가 호출됩니다.
  - currentCount가 증가합니다.
  - 업데이트된 수를 표시하기 위해 구성 요소가 렌더링됩니다.

### rendermode

구성 요소의 렌더링 모드를 지정하는 데 사용됩니다.  
렌더링 모드는 구성 요소가 호스팅되는 모델, 렌더링되는 위치 및 상호 작용 여부를 결정합니다.  
[https://learn.microsoft.com/ko-kr/aspnet/core/blazor/components/render-modes?view=aspnetcore-8.0](https://learn.microsoft.com/ko-kr/aspnet/core/blazor/components/render-modes?view=aspnetcore-8.0)

# Reference

[https://dotnet.microsoft.com/ko-kr/learn/aspnet/blazor-tutorial/intro](https://dotnet.microsoft.com/ko-kr/learn/aspnet/blazor-tutorial/intro)
