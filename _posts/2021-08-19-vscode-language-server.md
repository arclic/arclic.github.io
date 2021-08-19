---
title: "[TIL]Visual Studio Code Extension Language Server"
excerpt: "VSC Extension Language Server"

categories:
  - TIL
tags:
  - TIL
  - Extension

toc: true
---

## TIL

최근에 Github blog에 글을 포스팅 할 일이 많아지다 보니, 매번 글을 복사 붙여넣기 해서 맞춤법 검사하는 것이 조금 귀찮아졌고 시간도 오래 걸렸다. 그래서 VSCode 내부에서 자동으로 한글 맞춤법을 교정해주는 Visual Studio Code Extension을 하나 만들어 보고자 하였고, Extension API Documentation을 읽으면서 앞으로 개발하면서 주로 사용하게 될 것 같은 Language Server에 대해 정리해놓으면 좋을 것 같다고 생각돼 이렇게 글을 적는다.

## Visual Studio Code Language Extension

먼저 Visual Studio Code를 자주 사용하는 사람이라면 알겠지만, VSCode는 Syntax Highlighting, Snippet completion 그리고 Auto Completion 등과 같은 다양한 기능들을 유저가 사용하는 프로그래밍 언어에 맞춰 제공한다. 이런 기능들은 VSCode에 내장되어(built-in) 있지 않다. 하지만 언어별로 만들어진 다양한 extension을 통해 VSCode는 이런 편리하고 똑똑한 기능들을 제공한다. 예를 들어 HTML extension을 통해 .html 파일내의 syntax를 highlighting 하고, Typescript extension을 활용해 `console.`만 입력했을 때 `log`가 자동으로 띄어지도록 한다. 이런 언어 관련 기능들은 **Declarative language feature**과 **Programmatic language feature** 두 가지 카테고리로 나눠서 생각할 수 있다.

### Declarative language feature

Declarative language feature들은 규칙이 extension의 configuration에 정의된 기능들이다. 보통 정규표현식 형태로 규칙을 정의해놓고, 규칙에 해당되는 문장이나 단어나 나왔을 경우 해당 기능을 수행한다. Declarative language feature에 포함된 기능들은 아래와 같다.

- Syntax Highlighting
- Snippet completion
- Bracket matching
- Bracket autoclosing 등

유용한 많은 기능들이 포함되어 있지만, Programmatic language feature에 비하면 단순한 기능들이다. 이 포스팅은 Programmatic language feature을 주로 다루고 있기 때문에 Declarative language feature에 관심이 있는 사람은 [Declarative-language-server](https://code.visualstudio.com/api/language-extensions/overview#declarative-language-features)를 통해 확인하길 바란다.

### Programmatic language feature

Programmatic language feature은 Declarative language feature에 비해 훨씬 동적(dynamic)이다. Programmatic language feature은 조금 더 복잡한 기능을 수행하기에 보통 Language Server를 거쳐 서비스 된다. Language Server는 그저 extension과 별개인 다른 프로그램으로써 유저가 작성 중인 프로젝트를 분석하고 분석 결과를 extension에 제공하는 역할을 한다. 즉, Declarative language feature은 상대적으로 간단한 기능들로 구성되어 있기에 (규칙-기능) 쌍의 정보를 미리 configuration에 저장해두면 해당 규칙에 맞는 기능을 자동적으로 수행한다. 반대로 **<u>Programmatic language feature의 기능은 복잡하고 방대하기에 미리 모든 (규칙-기능) 쌍의 정보를 기록 해놓을 수는 없다.</u>** 따라서 documentation 분석 기능을 하는 별도의 프로그램이 필요했고, 이것이 **Language Server**의 역할이다. Programmatic language feature의 대표적인 기능은 아래와 같다.

- Hover information
- Auto completion
- Jump to definition
- Error checking 등

---

Language Server를 통해 Programmatic language feature을 제공하는 방법은 아래와 같다. 편의상 Javascript 파일내에서 Hover information feature가 동작한다고 가정하겠다. (특정 javascript code 위에 마우스를 올려놓을 경우 부가적인 정보가 widget에 보여짐)

1. Extension이 Javascript를 위한 Language Client와 Language Server를 제공한다.
2. Language Client는 Node.js Extension Host context 내에서 동작하고 여타 다른 Extension과 다를 바 없다. Language Client가 실행될 때 Language Server가 생성된 새로운 프로세스 위에서 동작하고, Language Client와 Language Server는 Language Server Protocol(LSP)를 활용해 통신한다.
3. 유저가 마우스를 Javascript code위에 올려놓는다.(Hover 이벤트 발생)
4. VS Code가 Language Client에게 hover 이벤트가 발생했다는 것을 알린다.
5. Language Client는 Language Server에게 hover 이벤트에 대한 결과를 요청하고, Language Server는 결과 처리 후 Language Client에게로 전송한다.
6. Language Client는 받은 결과를 다시 VS Code에게 전송하고, VS Code는 결과를 hover widget에 나타낸다.

Language Server를 통해 language feature를 제공하는 것이 복잡해 보이지만, 2가지 큰 장점을 가지고 있다.

- Language Server는 어떤 언어로든 작성될 수 있다.(Extension은 javascript로 작성 해야하지만 Language Server는 별도의 프로세스 이기 때문에 어떤 언어로든 작성될 수 있음)
- Language Server는 다른 editor에서도 재사용 될 수 있다.

## Language Server

앞서 살펴보았듯이 Programmatic language feature을 제공하기 위해 Language Server가 필요하다. 하지만 Language Server를 적용하는 과정에서 VSCode 개발진들은 아래와 같은 3가지 문제점을 발견했다.

### Language Server의 3가지 문제점

- **Language Server가 native language로 작성되어 있는 문제**  
  VSCode는 Node.js 런타임위에서 동작하기 때문에 보통 native language로 작성되어 있는 Language Server를 VSCode에 붙이기가 매우 까다로웠다.
- **Language Server가 resource intensive한 문제**  
  Programmatic language feature들은 dynamic하기 때문에 기능 제공을 위해서는 상대적으로 많은 CPU와 memory를 필요로 했다. 따라서 VSCode는 이런 resource intensive한 기능도 제공하면서 VSCode의 속도에 영향이 가지 않도록 해야만 했다.
- **다양한 editor에 적용할 수 없다는 문제**
  개발한 Language Server는 특정 editor에 종속되어 있었기 때문에 다른 에디터를 위한 Language Server는 별도로 제작해야 했다.

이 문제를 해결하기 위해 Microsoft는 [Language Server Protocol(LSP)](https://microsoft.github.io/language-server-protocol)를 도입했다. 이 LSP가 editor와 language server 사이의 통신을 표준화 함에 따라 Language Server는 어떤 언어로 작성될 수 있었고, 별도의 process에서 실행될 수 있었으며 특정 editor에 종속될 필요가 없게 되었다.

<center>
<img src="/assets/images/lsp-languages-editors.png">
<figcaption>Language Server Protocol(LSP)</figcaption></center>

### Implementing Language Server

VSCode 에서 Language Server(일반적인 의미에서의 language server)는 **Language Client**와 **Language Server**로 구성되어 있다.

- Language Client: 일반 VSCode extension으로 Javascript 또는 Typescript를 활용해 만들어짐
- Language Server: 별도의 process에서 동작하는 language analysis tool

둘 사이의 통신은 아래와 같이 Language Server Protocol을 활용한다. 특히 별도의 process를 활용하기 때문에 PHP Language Server는 PHP로 만들어져 있어도 LSP를 활용해 통신 가능하다.

<center>
<img src="/assets/images/lsp-illustration.png">
<figcaption>VSCode 내부의 Language Server</figcaption></center>

Node를 활용해 직접 새로운 Language Server을 구현하는 것은 [VSCode language-server-extension-guide](https://code.visualstudio.com/api/language-extensions/language-server-extension-guide#implementing-a-language-server)에 잘 설명되어 있다.

## 참고 자료

- [Visual Studio Code Extension API Guide](https://code.visualstudio.com/api)
