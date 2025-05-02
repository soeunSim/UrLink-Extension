# 📚 UrLink

<div align="center">

  <img width="250" src="https://github.com/user-attachments/assets/20c8066b-e725-4f7b-975b-5ebc11aa995f" />
  
  <br />
  
  [UrLink 익스텐션 다운로드](https://chromewebstore.google.com/detail/urlink/ahfnojpakpdiddbnafbmjngbifalkaeh?hl=ko&utm_source=ext_sidebar)
  
  <br />
  북마크가 많아서 원하는 내용을 찾기 힘드신가요?<br />이제 직접 방문하지 말고 검색해 보세요.<br /><br />
</div>

URLink는 여러분이 직접 북마크 페이지에 방문하지 않아도, 사용자가 검색한 키워드를 기반으로 관련된 북마크를 정확하게 찾아주는 북마크 관리 서비스입니다.<br /><br />

## 목차
- [1. 개발 동기](#1-%EA%B0%9C%EB%B0%9C-%EB%8F%99%EA%B8%B0)
- [2. 기술 스택](#2-%EA%B8%B0%EC%88%A0-%EC%8A%A4%ED%83%9D)
- [3. 기능](#3-%EA%B8%B0%EB%8A%A5)
- [4. 구현 세부사항](#4-%EA%B5%AC%ED%98%84-%EC%84%B8%EB%B6%80%EC%82%AC%ED%95%AD)
  * [4-1. 크롬에서 북마크 가져오기](#4-1-%ED%81%AC%EB%A1%AC%EC%97%90%EC%84%9C-%EB%B6%81%EB%A7%88%ED%81%AC-%EA%B0%80%EC%A0%B8%EC%98%A4%EA%B8%B0)
    + [4-1-1. ```chrome.bookmarks.getTree()```의 반환값](#4-1-1-chromebookmarksgettree%EC%9D%98-%EB%B0%98%ED%99%98%EA%B0%92)
    + [4-1-2. 트리 구조 평탄화하기](#4-1-2-%ED%8A%B8%EB%A6%AC-%EA%B5%AC%EC%A1%B0-%ED%8F%89%ED%83%84%ED%99%94%ED%95%98%EA%B8%B0)
  * [4-2. 크롤링을 선택한 이유[SPA, Iframe 해결]](#4-2-%ED%81%AC%EB%A1%A4%EB%A7%81%EC%9D%84-%EC%84%A0%ED%83%9D%ED%95%9C-%EC%9D%B4%EC%9C%A0spa-iframe-%ED%95%B4%EA%B2%B0)
  * [4-3. 느린 크롤링 속도로 인한 사용자 경험 개선하기](#4-3-%EB%8A%90%EB%A6%B0-%ED%81%AC%EB%A1%A4%EB%A7%81-%EC%86%8D%EB%8F%84%EB%A1%9C-%EC%9D%B8%ED%95%9C-%EC%82%AC%EC%9A%A9%EC%9E%90-%EA%B2%BD%ED%97%98-%EA%B0%9C%EC%84%A0%ED%95%98%EA%B8%B0)
    + [4-3-1. 크롤링 요청을 소규모로 그룹화하기](#4-3-1-%ED%81%AC%EB%A1%A4%EB%A7%81-%EC%9A%94%EC%B2%AD%EC%9D%84-%EC%86%8C%EA%B7%9C%EB%AA%A8%EB%A1%9C-%EA%B7%B8%EB%A3%B9%ED%99%94%ED%95%98%EA%B8%B0)
    + [4-3-2. Promise.race를 선택하지 않은 이유](#4-3-2-promiserace%EB%A5%BC-%EC%84%A0%ED%83%9D%ED%95%98%EC%A7%80-%EC%95%8A%EC%9D%80-%EC%9D%B4%EC%9C%A0)
  * [4-4. 사용자에게 크롤링 없이 추가검색 제공하기](#4-4-%EC%82%AC%EC%9A%A9%EC%9E%90%EC%97%90%EA%B2%8C-%ED%81%AC%EB%A1%A4%EB%A7%81-%EC%97%86%EC%9D%B4-%EC%B6%94%EA%B0%80%EA%B2%80%EC%83%89-%EC%A0%9C%EA%B3%B5%ED%95%98%EA%B8%B0)
    + [4-4-1. Extension Storage 추가 검색에 활용하기](#4-4-1-extension-storage-%EC%B6%94%EA%B0%80-%EA%B2%80%EC%83%89%EC%97%90-%ED%99%9C%EC%9A%A9%ED%95%98%EA%B8%B0)
    + [4-4-2. Extension Storage 용량 관리](#4-4-2-extension-storage-%EC%9A%A9%EB%9F%89-%EA%B4%80%EB%A6%AC)
  * [4-5. 북마크 URL 유효성 검증 및 크롤링 오류 처리](#4-5-%EB%B6%81%EB%A7%88%ED%81%AC-url-%EC%9C%A0%ED%9A%A8%EC%84%B1-%EA%B2%80%EC%A6%9D-%EB%B0%8F-%ED%81%AC%EB%A1%A4%EB%A7%81-%EC%98%A4%EB%A5%98-%EC%B2%98%EB%A6%AC)
  * [4-6. React에서 안전하게 키워드를 하이라이팅 처리하기](#4-6-react%EC%97%90%EC%84%9C-%EC%95%88%EC%A0%84%ED%95%98%EA%B2%8C-%ED%82%A4%EC%9B%8C%EB%93%9C%EB%A5%BC-%ED%95%98%EC%9D%B4%EB%9D%BC%EC%9D%B4%ED%8C%85-%EC%B2%98%EB%A6%AC%ED%95%98%EA%B8%B0)
- [5. 협업 방식](#5-%ED%98%91%EC%97%85-%EB%B0%A9%EC%8B%9D)
  * [5-1. 협업 코어 타임](#5-1-%ED%98%91%EC%97%85-%EC%BD%94%EC%96%B4-%ED%83%80%EC%9E%84)
  * [5-2. 깃 전략](#5-2-%EA%B9%83-%EC%A0%84%EB%9E%B5)
- [6. 일정](#6-%EC%9D%BC%EC%A0%95)
  * [6-1. 1차 개발](#6-1-1%EC%B0%A8-%EA%B0%9C%EB%B0%9C)
  * [6-2. 2차 개발](#6-2-2%EC%B0%A8-%EA%B0%9C%EB%B0%9C)
- [7. 팀원](#7-%ED%8C%80%EC%9B%90)


## 1. 개발 동기
일상적으로 인터넷을 사용하다 보면, 저장해둔 북마크가 어느새 쌓여가는 경험을 하게 됩니다. 그러다 보니 북마크의 제목만으로는 실제 페이지의 내용을 파악하기 어려워, 필요한 정보를 찾기 위해선 저장해둔 북마크를 매번 직접 방문해 확인해야 하는 불편함이 생겼습니다. 더불어 기존의 서비스들은 주로 북마크 제목을 기반으로 검색 기능을 제공했기에, 세세한 내용 검색에는 한계가 있었습니다.

이 문제를 해결하기 위해, 사용자가 북마크를 일일이 방문하지 않아도 원하는 키워드를 검색하면 북마크 문서 내부를 탐색하고, 검색한 키워드가 포함된 북마크를 추출해주는 익스텐션을 개발하게 되었습니다.

## 2. 기술 스택
<div align="center">
  
| 프론트엔드 | 백엔드 | 빌드 | 테스트 |
| ---------------------------------------------- | ---------------------------------------------- | ---------------------------------------------- | ---------------------------------------------- | 
| <img src="https://img.shields.io/badge/React-3B4250?style=flat-square&logo=React&logoColor=#61DAFB"/> <br /> <img src="https://img.shields.io/badge/Tailwind-3B4250?style=flat-square&logo=tailwindcss&logoColor=#06B6D4"/> <br /> | <img src="https://img.shields.io/badge/Node-3B4250?style=flat-square&logo=Node.js&logoColor=#5FA04E"/> <br /> <img src="https://img.shields.io/badge/Puppeteer-3B4250?style=flat-square&logo=Puppeteer&logoColor=#40B5A4"/> <br /> <img src="https://img.shields.io/badge/Express-3B4250?style=flat-square&logo=Express&logoColor=#646CFF"/>| <img src="https://img.shields.io/badge/Vite-3B4250?style=flat-square&logo=vite&logoColor=#646CFF"/> | <img src="https://img.shields.io/badge/Vitest-3B4250?style=flat-square&logo=vitest&logoColor=#6E9F18"/> |

</div>

## 3. 기능

<table>
  <tr>
    <td width="60%" align="center">
      <img width="300" center height="450" style="display:inline-block;" src="https://github.com/user-attachments/assets/28c5aee5-619a-4458-89d1-80bcc114f5af" />
    </td>
    <td width="40%">
      * 북마크 검색<br /><br />
      사용자의 북마크 목록을 불러온 뒤, 사용자의 북마크 URL을 순회하며 각 페이지를 크롤링하고, 본문 내 지정된 키워드를 검색하여 검색 결과를 점진적으로 화면에 렌더링합니다. <br/>
	    (익스텐션 초기진입화면)
    </td>
  </tr>
  <tr>
    <td>
      <img height="350" src="https://github.com/user-attachments/assets/f07088fa-ba09-480b-9dd6-f09e9279ace2" />
    </td>
    <td>
      * 결과 내 재 검색<br /><br />
      사용자는 익스텐션에서 수행한 검색 기록을 기반으로, 옵션 페이지 내에서 결과를 다시 검색할 수 있습니다. <br/>
      이때 저장된 검색 데이터는 로컬 스토리지를 통해 제공되므로, 추가적인 크롤링 없이 빠르게 재검색이 가능합니다.<br/>
     (옵션페이지 초기진입화면)
    </td>
  </tr>
</table>

## 4. 구현 세부사항
### 4-1. 크롬에서 북마크 가져오기
프로젝트의 출발점은 사용자가 저장해 놓은 크롬 북마크 데이터를 불러오는 것이었고, 크롬 익스텐션 형태로 크롬의 API를 활용하기 위해 프로젝트를 기획한 만큼 Chrome Extension API인 ```chrome.bookmarks.getTree()```를 사용하여 북마크를 가져오기로 했습니다.

#### 4-1-1. ```chrome.bookmarks.getTree()```의 반환값
```chrome.bookmarks.getTree()``` 가 어떤 반환값을 가져오는지 알기 위해 아래 코드를 사용해 확인 해봤습니다. 그 결과로 예측할 수 없는 깊이의 트리 구조라는 것을 알 수 있었습니다.
```js
chrome.bookmarks.getTree((treeList) => {
  console.log(treeList);
});
```

<br />
예측할 수 없는 트리구조인 이유는 크롬은 사용자 북마크 관리를 위해 폴더를 생성할 수 있고, 해당 폴더는 제한 없이 중첩할 수 있기 때문입니다.

<div align="center">
  <table>
    <tr>
      <th>크롬 북마크 폴더구조</th>
    </tr>
    <tr>
      <td>
        <img width="400" src="https://github.com/user-attachments/assets/26559c09-60b1-44fc-915b-c76e2206cece" />
      </td>
    </tr>
  </table>
</div>

#### 4-1-2. 트리 구조 평탄화하기
트리 구조를 평탄화 하기 위해서 처음에는 재귀 구조의 함수를 사용했지만 재귀는 콜 스택 중첩으로 인한 스택 오버플로우를 야기할 수 있고 이는 익스텐션 성능에 악영향을 끼칠 수 있다고 판단했고, 비재귀적인 접근방식으로 ```while``` 을 사용해 평탄화를 진행했습니다.<br /><br />
<div align="center">
  <table>
    <tr>
      <th>재귀 방식</th>
      <th>비재귀 방식</th>
    </tr>
    <tr>
      <td><img width="418" alt="스크린샷 2025-03-11 오후 2 26 40" src="https://github.com/user-attachments/assets/79c0485d-9003-4fbe-8a09-022c6fde9557" /></td>
      <td><img width="417" alt="스크린샷 2025-03-11 오후 2 45 24" src="https://github.com/user-attachments/assets/41132adf-c6e8-4210-a1ee-55584d1884f3" /></td>
    </tr>
  </table>
</div>

평탄화가 필요했던 이유는 트리 구조는 React상태 관리시 불변성을 지키기 힘들고, 값을 꺼내야 할 때 불편함이 존재하기 때문에 저희가 사용할 목적에 맞는 자료구조로의 개선이 필요했기 때문입니다.<br />
따라서, 아래 코드처럼 while을 사용해 비재귀적 방식으로 DFS(Depth-First Search)를 구현하여 스택 오버플로우를 회피하면서 중첩 구조의 북마크 객체를 평탄화 할 수 있었습니다.
```js
const getAllBookmark = (nodeItems) => {
  const newBookmarkList = [];
  const stack = [...nodeItems];

  while (stack.length) {
    const node = stack.pop();
    if (Array.isArray(node)) {
      stack.push(...node);
    } else if (typeof node === "object" && node !== null) {
      if (node.children) {
        stack.push(...node.children);
      }
      if (node.title && node.url) {
        newBookmarkList.push(node);
      }
    }
  }

  setBookmarkList(newBookmarkList);
};
```

<br/>

### 4-2. 크롤링을 선택한 이유[SPA, Iframe 해결]
UrLink 프로젝트는 크롤링을 통해서 SPA(Single Page Application)의 HTML 렌더링을 기다리고, iframe 내부 문서에 CORS문제 없이 접근할 수 있었습니다.<br />
크롤링을 선택하기 이전에는 북마크 URL을 받아 해당 URL로 fetch요청을 보내고, 응답으로 받은 HTML 문자열에서 사용자가 입력한 키워드를 찾는 것이었습니다.<br /><br />
MPA(Multi Page Application)의 경우 저희 의도대로 잘 동작이 됐으나 fetch를 이용한 방법은 2가지 문제점이 있었습니다.
1. SPA(Single Page Application)의 경우, fetch 요청으로 가져오는 HTML은 렌더링되기 전의 비어있는 HTML을 받아오기 때문에 키워드를 찾을 수 없었습니다.
2. iframe DOM 파싱 과정에서 내부 HTML을 로드하기 때문에, 문자열로 얻어온 HTML에선 iframe 내부 문서를 확인할 수 없었습니다.<br /><br />

<div align="center">
  
  | | MPA | SPA | iframe |
  | ----- | ------ | ------ | ------ | 
  | 렌더링 시점 | 완성된 페이지 전체를 렌더링 | HTML에 페이지의 일부분을 받아 점진적 렌더링 | DOM파싱 중 iframe 요소를 발견했을 때 로드 |
  
</div>

이를 해결하기 위해 Puppeteer의 헤드리스 브라우저 모드를 사용했고, MPA뿐만 아니라 SPA와 iframe까지 대응할 수 있었습니다.
1. SPA 페이지 로딩 대기: Puppeteer를 사용하여 인간적인 브라우징 패턴을 따라하고, SPA 페이지가 완전히 로딩될 때까지 대기하기 때문에 페이지가 완전히 렌더링된 후의 HTML을 가져올 수 있었습니다.
2. iframe 내부 내용 접근: Puppeteer를 사용하여 iframe 내부의 내용을 읽어올 수 있습니다. iframe이 발견되면, iframe의 src 속성을 통해 iframe 내부로 이동하여 다시 크롤링 진행하기 때문에 iframe 내부의 DOM에 접근해 일치하는 키워드를 찾을 수 있었습니다.
위와 같은 방법으로 크롤링을 통해 SPA와 iframe 문제를 해결하고, 사용자가 입력한 키워드를 보다 더 넓은 범위로 사용자 검색에 대응할 수 있게 되었습니다.<br />

<br/>

### 4-3. 느린 크롤링 속도로 인한 사용자 경험 개선하기
북마크 내부의 DOM을 탐색하고 사용자가 입력한 키워드를 검색하기 위해선 크롤링이 필수적이었습니다. 다만, 크롤링에 걸리는 시간은 절대적으로 필요한 시간이며 이를 줄일 방법은 많지 않았습니다. 따라서 시간을 줄이는 대신 크롤링 요청을 나누어 처리하는 방법을 도입했습니다. 이를 통해 사용자가 결과를 보다 신속하게 확인할 수 있도록 개선했습니다.<br /><br />

#### 4-3-1. 크롤링 요청을 소규모로 그룹화하기
기존 로직은 Promise.allSettled를 사용했습니다. allSettled는 all과 다르게 하나의 요청에서 에러가 발생해도 나머지 요청을 계속 진행하기 때문에, 모든 북마크에 대한 요청을 병렬로 한 번에 처리하고, 사용자에게 결과를 보여주려 했습니다.<br />
그러나 사용자가 50개의 북마크를 가지고 있다면 50개를 한 번에 요청하게 되어 서버 과부하와 OOM(Out-of-Memory)을 유발할 수 있었고, 사용자는 50개의 북마크가 크롤링을 마칠 때까지 결과를 기다려야 했기 때문에 사용자 경험에 있어서 좋지 않다고 생각했습니다.<br /><br />
이를 개선하기 위해, Promise.allSettled를 5개 단위로 묶은 프로미스 배열을 사용하여 5개씩 병렬로 처리하고, 이 병렬 처리 그룹을 직렬로 처리하는 방식으로 변경했습니다. 이를 통해 서버 과부하를 줄이고, 사용자는 크롤링 결과를 5개씩 순차적으로 빠르게 확인할 수 있게 되었습니다.<br />

<div align="center">
  <img src="https://github.com/user-attachments/assets/be184ec0-cd19-48ab-92ac-0dd421fe2ba1" />
</div>

#### 4-3-2. Promise.race를 선택하지 않은 이유
사용자 개선을 위해서 Promise.allSetteld를 5개씩 묶어 요청했다면 Promise.race를 통해 가장 빨리 끝난 요청을 사용자에게 보여줄 수 있기 때문에 Promise.race도 사용자 경험 개선에 있어서 적합한 선택이라고도 생각됩니다.<br /><br />
하지만 사용자가 검색한 키워드가 북마크 내부에 존재하는지 정확히 알 수 없기 때문에 만약 100개의 북마크 배열에서 99번 인덱스에 키워드를 포함한 북마크가 존재한다면 사용자는 총 100번의 크롤링을 기다려야만 결과를 확인할 수 있기 때문에 Promise.race를 선택하지 않았습니다.


<br/>

### 4-4. 사용자에게 크롤링 없이 추가검색 제공하기
생각외로 북마크는 사용자 취향에 맞는 페이지들을 북마크 해놓은 것이라 중복 되는 키워드가 많이 존재했고, 사용자가 정말 원하는 북마크를 찾는데에 방해가 될 수 있었습니다. 그래서 1차로 나온 검색 결과를 가지고 추가적인 검색을 제공 하면 사용자 편의성이 증가할 것이라고 생각했습니다.<br /><br />
추가 검색은 크롤링을 재사용하지 않고, 크롤링 과정에서 키워드를 찾으며 탐색 했던 HTML 문자열들을 배열 형태로 저장한 후 이 문자열 배열을 이용해 추가 검색을 할 수 있도록 했습니다.<br /><br />
크롤링을 재사용하지 않은 이유는 크롤링의 속도였습니다. Promise.allSetteld()를 5개씩 묶어 보내고 결과를 먼저 보여줌으로써 검색 사용성을 개선했는데, 다시 추가 검색을 통해 크롤링의 느린 속도를 경험한다면 사용자 이탈률이 높아질 것이라고 생각했습니다.<br />
따라서, 추가 검색은 1차 검색보다 빠른 속도를 제공해야 했고, 1차 검색 시 단순히 키워드가 포함된 문장만 가져오는 것이 아닌 페이지 HTML 본문에서 ```innerText```를 통해 숨겨진 텍스트를 제외한 모든 텍스트를 가져와 사용자가 추가 검색 시 HTML 본문에서 즉시 추가 키워드를 찾을 수 있도록 했습니다.


#### 4-4-1. Extension Storage 추가 검색에 활용하기
firebase, mongoDB 등 다양한 스토리지가 있지만 저희는 익스텐션 LocalStorage에 저장하기로 했습니다.<br />
익스텐션 LocalStorage는 크롬 익스텐션에서 제공해주는 스토리지로 최대 10mb까지 사용할 수 있고 사용하기에 간편하다는 장점이 있고 또한, 옵션 페이지를 적용한 저희 익스텐션에서 옵션 페이지와 익스텐션 간의 데이터 공유 시 LocalStorage를 사용해 편리하게 공유할 수 있기에 적합한 선택이라고 생각했습니다.<br /><br />

일반 웹 환경과 동일한 브라우저 기반이라 생각하여 ```window.localStorage.setItem()```을 사용하면 데이터를 저장할 수 있을 거라 기대했지만, 익스텐션 환경에서는 해당 함수가 정상적으로 호출되지 않았습니다.
```js
ReferenceError: localStorage is not defined
```
이 에러와 공식문서를 통해 익스텐션 환경에서는 localStorage를 사용할 수 없음을 확인했고, 크롬 익스텐션 공식문서를 살펴보며 익스텐션 환경에서는 기존 브라우저 Storage와 다른 Extension Storage를 써야 한다는 것을 알게 되었습니다.
```js
chrome.storage.local.set();
```

<br/>

#### 4-4-2. Extension Storage 용량 관리
```StorageManager API``` 를 사용하여 현재 용량을 확인하고 저장 공간이 부족하면 가장 오래된 데이터를 자동으로 삭제한 후 새로운 데이터를 저장하는 방식을 적용했습니다.<br /><br />

<div align="center">
  <img width="700" src="https://github.com/user-attachments/assets/f30ca9bf-2d36-4bf4-83e2-d5dce8494df7"/>
</div>

</br>
초기에는 데이터를 저장할 때 용량을 확인하지 않고 무조건 저장을 시도했습니다. 그러나 Chrome Storage의 용량 제한(기본 10MB)으로 인해 저장 공간이 가득 차는 문제가 발생할 수 있었습니다.<br /><br />

Storage에 저장된 데이터의 용량을 파악하기 위해 Chrome Storage API의 ```chrome.storage.local.getBytesInUse()```를 활용했습니다. 이를 통해 현재 Storage가 얼마나 사용되고 있는지 실시간으로 확인할 수 있도록 했고, 데이터를 저장할 때 무조건 저장을 시도하는 것이 아니라, 먼저 현재 Storage의 사용량을 확인한 후 저장할 공간이 충분한 경우에만 데이터를 추가하고, 만약 공간이 부족하면 가장 오래된 데이터를 삭제한 후 저장을 시도하도록 로직을 변경했습니다.

<br/>

### 4-5. 북마크 URL 유효성 검증 및 크롤링 오류 처리
페이지 유효성 검사와 크롤링 오류 처리 강화를 위해, 잘못된 URL은 **400 Bad Request**로, 네트워크·렌더링 실패는 **500 Internal Server Error**로 명확히 구분하도록 개선했습니다. <br/>

대부분의 북마크 URL은 사용자가 등록할 때 올바른 형태를 갖추지만, 사용자 입력 실수나 디코딩 과정에서 예외가 발생할 수 있어 별도 예외 처리가 필요했습니다. 따라서 URL 형식 검증 단계에서 잘못된 요청 형식, 유효하지 않은 요청 메시지를 의미하는 **400 Bad Request**로 응답하도록 구현했습니다. <br/>

그 외 네트워크 또는 렌더링 오류로 크롤링이 실패할 경우, **500 Internal Server Error**를 반환해 사용자에게 접근 실패 사유를 명확히 제공하도록 로직을 처리하였습니다.
```js
catch (error) {
  if (!isCheckTrueThisUrl(decodedLink)) {
	  // 예외케이스, URL 포맷 오류 확인 처리.
    return res
      .status(400)
      .send({ message: `[Invalid Characters in HTTP request]  ${error}` });
  } else {
	  // 네트워크·렌더링 실패 처리.
    return res
      .status(500)
      .send({ message: `[ServerError occured] ${error}` });
  }
} 
```
그러나 **500 Internal Server Error** 처리에서 들어오는 값을 확인 하던 중 일부 북마크 페이지가 iframe 내부에서만 콘텐츠를 렌더링하며, 최초 ```await page.$eval("body", ...)``` 호출 시 ```innerText```가 빈 값으로 인식되어 불필요한 500 에러가 반환 되는 사례를 발견하게 되었습니다.

<div align="center">
  <img src="https://github.com/user-attachments/assets/e2ba5ff8-6d0f-4ab4-9add-55b8e40701ca"/>
</div>

초기 ```body.innerText```가 빈 값으로 반환될 경우, iframe 요소의 src 경로로 ```page.goto()```를 다시 호출하여 해당 URL에 재접속해 콘텐츠를 재수집하도록 아래 로직과 같이 수정했습니다.

```js
if (!innerText) {
  // iframe 로드 대기
  await page.waitForSelector("iframe", { timeout: TIMEOUT });

  // iframe src로 재접속
  const iframeUrl = await page.$eval("iframe", iframe => iframe.src);
  await page.goto(iframeUrl);

  // 재추출한 본문에서 키워드 검색
  innerText = await page.evaluate(() => document.body.innerText);
  hasKeyword = innerText.toUpperCase().includes(upperCasedKeyword);

  // 비허용 iframe URL 방어
  if (!iframeUrl || !iframeUrl.startsWith("https://blog.naver.com")) {
    throw new Error(`[Invalid iframe URL]`);
  }
}
```
이로써 iframe 기반 페이지에서도 안정적으로 크롤링이 가능해졌으며, 사용자에게 정의된 **400 Bad Request & 500 Internal Server Error** 에러 사유를 일관적으로 전달할 수 있게 되었습니다.

<br/><br/>

### 4-6. React에서 안전하게 키워드를 하이라이팅 처리하기
React에서는 기본적으로 HTML을 직접 삽입할 수 없기 때문에, 문장에서 특정 키워드를 강조(하이라이팅) 하기 위해 `split`과 `map`을 활용한 방식으로 구현했습니다.<br /> 

**[❌ 사용하지 않은 방식 1: `mark` 태그]** <br/>
HTML5의 `mark` 태그는 문서 내 특정 검색 결과 또는 중요 텍스트를 의미적으로 강조할 때 사용됩니다.
하지만 이번 케이스는 단순 스타일 목적의 하이라이팅이기 때문에, [MDN 공식문서](https://developer.mozilla.org/ko/docs/Web/HTML/Reference/Elements/mark)에서도 권장하지 않는 사용 방식이었습니다.

<br/>

**[❌ 사용하지 않은 방식 2 : `dangerouslySetInnerHTML`]** <br/>
dangerouslySetInnerHTML를 통해 HTML을 직접 삽입하면 간편하게 강조 처리를 할 수 있지만,
[XSS(교차 사이트 스크립팅) 공격과 같은 보안 위험이 있고 최대한 피하는 것](https://react.dev/reference/react-dom/components/common#dangerously-setting-the-inner-html) 이 React 공식 문서에서도 권장하지 않는 것을 확인하였습니다.

<br/>

**[✅ 최종선택 : `split()`와 `map()`으로 안전하게 하이라이트 처리]** <br/>
HTML 삽입 없이도 키워드를 강조하기 위해, 다음과 같은 방식을 적용했습니다.

1. 먼저 `split()`으로 문장을 검색 키워드 기준으로 분할합니다.
2. map()으로 각 조각을 순회합니다.
3. 키워드 위치에만 안전한 <span> 요소로 강조 태그를 삽입합니다.

``` js
{bookmark.urlText.split(searchKeyword).map((item, index) => {
  if (item === "") {
    return;
  }

  return (
    <span key={index}>
      {index > 0 && (
        <span className="bg-blue-800 rounded-lg px-1 inline-block text-white mx-px">
          {searchKeyword}
        </span>
      )}
      {item}
    </span>
  );
})}
```
이처럼 보안성과 의미를 모두 고려한 방식으로 키워드 강조 기능을 구현할 수 있었습니다.

<br/>

## 5. 협업 방식
### 5-1. 협업 코어 타임
오전 10시 ~ 자정<br /><br />
매일 정해진 시간에 오프라인 방식으로 모여 직접 스크럼 회의를 통해 협업 상황에서 발생할 수 있는 깃 전략, 컨벤션 문제들을 공유하고 같이 해결해 나갔습니다.<br /><br />
  <div align="center">
    <a href="https://psh5032.notion.site/1776fb66a2e4802997e0f9c151b5a4c1">
      <img width="500" src="https://github.com/user-attachments/assets/690f4024-061f-493f-a9b5-59c67ac1ae8f" />
    </a>
  </div>

### 5-2. 깃 전략
Git Flow 전략과 Squash merge를 통해 한 기능에 대한 다수의 커밋 기록을 합쳐 기능 별로 깔끔한 커밋 기록을 유지하고자 했습니다. 기존 커밋 내역은 Squash merge의 description 부분에 적습니다.<br /><br />

브랜치 전략
- feature branch<br />
각 기능이나 디자인, 버그 수정 작업은 별도의 feature 브랜치에서 진행했습니다.<br />
feature 브랜치는 dev 브랜치로부터 분기합니다.<br />
작업이 완료되면, Pull Request를 통해 필수적으로 팀원의 코드 리뷰를 진행합니다.<br />
코드 리뷰 단계에서는 질문, 보완할 점, 좋은 점, 에러 가능성을 주로 리뷰 합니다.<br />

- dev branch<br />
모든 개발 작업은 dev 브랜치에서 통합됩니다.<br />
dev 브랜치는 항상 최신 상태를 유지하며, 배포 전 단계의 안정적인 상태를 나타내야 합니다.<br />
코드 리뷰를 통해 모든 변경 사항이 검토된 후에만 dev 브랜치에 병합됩니다.<br />

- main branch<br />
main 브랜치는 배포를 위한 브랜치로, 항상 배포 가능한 수준의 안정성을 유지합니다.<br />
새로운 버전을 배포할 때만 dev 브랜치로부터 main 브랜치로 병합합니다.<br />

- Squash merge<br />
모든 feature 브랜치에서 dev 브랜치로 병합할 때 Squash merge를 사용합니다.<br />
이를 통해 여러 개의 커밋을 하나의 커밋으로 합쳐 히스토리를 깔끔하게 유지합니다.<br />

## 6. 일정
### 6-1. 1차 개발
2025년 1월 13일 ~ 2025년 2월 2일<br />
> 진행 사항
> - 협업 규칙 및 Git 플로우 규칙 설정
> - 프로젝트 esLint, pretteir, husky 설정
> - 서버 Express, Puppeteer 설정
> - 크롤링[Puppeteer] 서버 개발
> - 익스텐션 검색 로직 구현
> - 익스텐션 옵션페이지 추가

### 6-2. 2차 개발
2025년 3월 27일 ~ 2025년 4월 12일<br />
> 진행 사항
> - 검색 시 fetch 요청이 중단되는 이슈해결
> - localStorage에 추가할 크롤링 data 추출
> - 옵션 페이지, 결과 내 재검색 기능 구현
> - 크롤링요청 개선 - 타임아웃 로직 추가
> - 1차, 익스텐션 배포를 위한 프로젝트 설정

**[추가개발 고려사항]**
> - 전역상태 설정
> - fetch Aixos로 변경

## 7. 팀원
- 심소은 : euns127@gmail.com
- 박성훈 : seonghoon.dev@gmail.com
- 이수보 : nullzzoa@gmail.com
