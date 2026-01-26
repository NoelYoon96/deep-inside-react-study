### 1. 지연 로딩
#### Lottie를 dynamic loading을 사용하여 초기 번들 사이즈를 줄임
- 상황 : 홈 페이지에서 특정 투표 요소를 클릭하면, 모달이 나오고 그 위에 lottie가 뜨는 케이스
  - 모달 내부에서만 필요한 무거운 라이브러리로 (+json 파일) dynamic import로 분리해 홈 초기 번들 비용을 줄임
  - 첫 화면 유저 경험이 중요한 홈이었다는 점 -> 사용자가 실제로 쓸 때만 다운로드되도록 Lottie를 지연 로딩
```tsx
const Lottie = dynamic(() => import('react-lottie-player'), { ssr: false })

{open && (
  <Lottie
    animationData={flower_animation}
    style={{ width: '100%', height: '100%' }}
    play
    loop
  />
)}
```


### 2. React Query & Zustand
-> SPA 내에서의 상태/데이터 관리 문제

#### Reacy Query
##### 발생 상황
- 탭/페이지 이동 시 API 재호출
- 같은 데이터를 여러 컴포넌트에서 중복 호출

##### React Query에서의 캐싱
- 서버 상태 캐싱
- staleTime을 기반으로 언제 다시 호출할지를 규정 -> SPA에서 가장 큰 문제인 중복 호출/깜빡임 등을 감소

- **staleTime :** 해당 데이터를 언제까지 신선한 데이터로 판단할 것인지 
  - staleTime이 지나지않았다면, refetch하지 않고 재사용
- **cacheTime :** 미사용 중인 데이터를 언제까지 메모리에 저장하고 있을 것인지
  - cacheTime이 지났으면, 캐시에서 삭제

- staleTime이 자났을때, cacheTime이 아직 안 지났다면 => 캐시 결과로 화면에 바로 데이터 뜸
