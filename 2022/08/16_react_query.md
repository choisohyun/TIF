## 1. 배경

진행하던 프로젝트에서는 fetch API만을 래핑해서 사용하였습니다. fetch API만으로도 다양한 메소드를 지원하기 때문에 충분하긴 했지만, 문제는 에러 처리였습니다. 에러 처리를 계속해서 try catch 문으로 묶어주거나 예외처리로 계속 핸들링해야 하는 번거로움이 생겼습니다.  
그래서 추가로 찾은 API 전용 툴은 react-query, swr가 있었습니다. 에러처리나 로딩처리가 자유롭고 잘 래핑해서 쓰면 좋을 것 같다고 판단하였습니다. 또한, react-query가 다운수가 더 많고 프로덕션 레벨에서의 레퍼런스도 많아 선택하게 되었습니다.  
https://www.npmtrends.com/swr-vs-react-query

## 2. `React Query란`: 문법 간단 설명

그러면 React Query는 정확히 무엇을 제공하는 것일까요? 
기본 기능 2가지와 재미있는 기능 2가지 간단히 설명하겠습니다.
api 통신하는 부분뿐만 아니라 메모리 캐시하는 기능들도 있는데 이부분은 정확히 이해하지 못해 이글에서 다루진 않습니다.

### 1) useQuery

가장 기본으로 useQuery는 모든 API method에 대응하는 훅입니다. 공식 홈페이지에서는 서버에 데이터를 변경하는 경우에는 mutation 관련 훅을 권장하고 있습니다. 

useQuery의 응답인데 기본적으로 상태에 대한 값이 많습니다. 저는 여기서 주로 쓰는 값은 **data, isError, error, isLoading, isSuccess, refetch** 정도였습니다.
```ts
export interface QueryObserverBaseResult<TData = unknown, TError = unknown> {
    data: TData | undefined;
    dataUpdatedAt: number;
    error: TError | null;
    errorUpdatedAt: number;
    failureCount: number;
    isError: boolean;
    isFetched: boolean;
    isFetchedAfterMount: boolean;
    isFetching: boolean;
    isIdle: boolean;
    isLoading: boolean;
    isLoadingError: boolean;
    isPlaceholderData: boolean;
    isPreviousData: boolean;
    isRefetchError: boolean;
    isRefetching: boolean;
    isStale: boolean;
    isSuccess: boolean;
    refetch: <TPageData>(options?: RefetchOptions & RefetchQueryFilters<TPageData>) => Promise<QueryObserverResult<TData, TError>>;
    remove: () => void;
    status: QueryStatus;
}
```

사용법도 간단합니다. 첫번째 인자에는 **유니크한 키값**, 두번째는 **Promise로 return하는 함수**, 세번째는 **옵션객체**를 넣어주면 됩니다. 
옵션도 너무 많아서 제가 주로 설정한 내용만 가져왔습니다. 

여기서 옵션값을 state로 가지고 있게 된다면 state가 변경될 때마다 fetch가 실행됩니다. 
이를 이용하여 enabled 옵션을 로그인 여부 값으로 설정했습니다.
enabled, onError 부분만 봐도 벌써 생산성이 올라갔습니다. 
```ts
  const fetchUserInfo = useQuery('getProfile', api.member.getProfile, {
      enabled: state.logined, // 로그인 되어있으면 fetch, 아니면 실행시키지 않음
      retry: false, // 에러나면 3번까지 재시도하는 것이 기본 옵션이라 꺼둠
      refetchOnWindowFocus: false, // 탭이나 화면 이동할 때에도 계속 fetch해서 꺼둠
      onError: removeAccessToken, // 에러났을 때 실행되는 부분
  });
```


### 2) useMutation

get 이외의 method을 호출할 때 사용하는 훅입니다. 서버쪽 상태를 변경하는 내용이 주를 이루기 때문에 useQuery보다 간단한 구성을 이루고 있습니다.

mutate, mutateAsync 로 주로 호출하게 됩니다. 저는 주로 Promise를 반환하는 mutateAsync를 사용했습니다.
useQuery랑 동일하게 onError, onSuccess, onSettled를 옵션으로 지정할 수 있습니다.
```ts
export declare type UseBaseMutationResult<...(생략)...>, {
    mutate: UseMutateFunction<TData, TError, TVariables, TContext>;
}> & {
    mutateAsync: UseMutateAsyncFunction<TData, TError, TVariables, TContext>;
};

export interface MutationObserverErrorResult<TData = unknown, TError = unknown, TVariables = void, TContext = unknown> extends MutationObserverBaseResult<TData, TError, TVariables, TContext> {
    data: undefined;
    error: TError;
    isError: true;
    isIdle: false;
    isLoading: false;
    isSuccess: false;
    status: 'error';
}
```

useQuery보다 사용법도 더 간단합니다. 여기서는 사용자가 찜하기 버튼을 누르면 응답값을 받아 classname이 바뀌도록 구현했습니다.
```ts
  const changeLikeProduct = useMutation((productNos: number[]) =>
    api.product.postProfileLikeProducts({ requestBody: { productNos } }),
  );
  ...
  const result = await changeLikeProduct.mutateAsync([productNo]);
  setLiked(head(result.data)?.result ?? false);
```

### 3) onMutate: Optimistic Update

useMutate에서 api 호출이 성공할 것으로 보고 미리 업데이트를 시켜주는 옵션입니다. 실패하면 없던 것으로 롤백됩니다.
위의 코드를 아래와 같이 리팩토링할 수 있습니다. 코드도 간편해지고 UX도 개선이 될 것으로 보입니다.
(이건 사실 보미님이 공유해 주신 우형 유투브 보고 알게 되었습니다.)
```ts
  const changeLikeProduct = useMutation(
    (productNos: number[]) => api.product.postProfileLikeProducts({ requestBody: { productNos } }),
    { options: { onMutate: () => setLiked((prev) => !prev) } },
  );
  ...
  changeLikeProduct.mutate([productNo]);
```

### 4) Paginated & Infinite Queries

페이지네이션과 인피니트 로딩을 위한 옵션을 넣을 수 있습니다. 

- 페이지네이션: https://react-query.tanstack.com/guides/paginated-queries
- useInfiniteQuery: https://react-query.tanstack.com/guides/infinite-queries

## 3. `활용`: QueryClientProvider, QueryClient

useQuery는 너무 편리하지만 안 쓰는 옵션이 있고, 에러 시 메시지 알럿창 등을 공통으로 처리해주는 것이 필요했습니다.  
QueryClient에 defaultOptions를 삽입하여 프로젝트 전체에 적용할 옵션을 설정하였습니다.  
(처음에는 이걸 제대로 활용하지 못해서 복잡하게 했었는데 간단한 해결방법이 있었습니다..)


```ts
export const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      enabled: false,
      retry: false,
      refetchOnWindowFocus: false,
      onError: error => globalHttpErrorHandler(error as HTTPError),
    },
    mutations: {
      onError: error => globalHttpErrorHandler(error as HTTPError),
    },
  },
});

const App = () => {
  const element = useRoutes(routes);

  return <QueryClientProvider client={queryClient}>{element}</QueryClientProvider>;
};
```

## 4. 결론

- react query는 선언적으로 에러와 상태 관리를 처리할 수 있습니다. 최근에 만들어지고 유지보수가 되고 있는만큼 코드들도 모던합니다.
- 단점이라고 하면 react에 종속적인 측면이 다른 툴보다 강하다고 볼 수 있겠습니다. 
- 에러 처리 등 공통으로 처리해야 하는 부분이나 API 호출 시 상태관리가 빈번한 프로젝트에서 사용하면 적절할 것 같다는 생각이 들었습니다. 
