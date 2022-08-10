# globalThis is not found 에러가 뜨는 경우
`npm test` 를 할 때 이와 같은 에러가 났다. 

## 원인
- 회사의 한 프로젝트에서 A님이 테스트를 할 때 jest를 사용할 수 있게 바꾸시면서 jest 버전은 28로 올리셨다. 그리고 배포를 하려고 했는데 github actions에서 테스트가 깨지는 상황이 벌어졌다.

```
/Users/####/node_modules/jest-util/build/installCommonGlobals.js:76
const DTRACE = Object.keys(globalThis).filter(key => key.startsWith('DTRACE'));
                           ^

ReferenceError: globalThis is not defined
```

검색을 하다가 왜 이런 오류가 나는 건지 알아내게 되었다.

https://stackoverflow.com/questions/66586352/referenceerror-globalthis-is-not-defined

## 해결
- globalThis 를 찾을 수 없다고 뜨는 이유는 간단했다. 내 환경과 서버의 환경에선 node 10.24.1을 사용하고 있었다. 찾아보니 globalThis는 node v12부터 사용할 수 있다고 한다. 
- 그래서 node 버전을 12 이상으로 업그레이드 시킬려고 했는데 상의 후 LTS 버전인 16 대로 완전 올려버리기로 결정했다.
    - 배포하시기 전에 했을 때 테스트가 다 통과했던 이유는 A님의 로컬 버전은 16 이셨다.

### 순서
1. nvm install 16
2. nvm use 16
3. npm test

이후 다시 테스트를 돌렸을 때 globalThis 문제가 뜨지 않고 다 성공적으로 동작했다.

## 정리
그냥 node 버전을 올려줬다. 이래서 도커를 사용하는 것 같다. node 버전을 올리는 것 말고도 다른 방법이 있지만 마침 node 버전을 슬슬 올리자는 이야기가 나오고 있었기 때문에 해당 방법을 선택했다.