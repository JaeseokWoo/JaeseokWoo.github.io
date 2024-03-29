---
date: '2022-11-17'
title: '자바스크립트 런타임'
categories: ['Node']
summary: '서버'
thumbnail: '../images/node_js_textbook.jpg'
---

# 자바스크립트 런타임

> Node.js®는 [Chrome V8 JavaScript 엔진](https://v8.dev/)
> 으로 빌드된 JavaScript 런타임입니다.

[노드 공식 사이트](https://nodejs.org/ko/)에서 게시된 노드 소개글에 따르면 **노드는 자바스크립트 런타임**입니다. **런타임은 특정 언어로 만든 프로그램들을 실행할 수 있는 환경**을 뜻합니다. 따라서 노드는 자바스크립트 프로그램을 컴퓨터에서 실행할 수 있도록 하는 실행기라고 봐도 무방합니다.

브라우저는 자바스크립트 런타임을 내장하고 있으므로, 기존에는 자바스크립트 프로그램을 웹 브라우저 위에서만 실행할 수 있었습니다. 브라우저 외의 환경에서 자바스크립트를 실행하기 위한 여러 시도가 있었으나, 자바스크립트의 실행 속도 문제 때문에 모두 큰 호응을 얻지 못했습니다.

하지만 2008년 구글이 V8 엔진을 사용하여 크롬을 출시하자 이야기가 달라졌습니다. 당시 V8 엔진은 다른 자바스크립트 엔진과 달리 매우 빨랐고, 오픈 소스로 코드를 공개했습니다. 속도 문제가 해결되자 라이언 달(Ryan Dahl)은 2009년 V8 엔진 기반의 노드 프로젝트를 시작했습니다.

<img src='./images/internal_structure_of_node.JPG' width='350px'>

노드는 V8과 더불어 libuv라는 라이브러리를 사용합니다. V8과 libuv는 C와 C++로 구현되어 있습니다. 노드가 알아서 자바스크립트 코드를 V8과 libuv에 연결해줍니다. libuv 라이브러리는 노드의 특성인 이벤트 기반, 논 블로킹 I/O 모델을 구현하고 있습니다.
