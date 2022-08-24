## Phân trang

- việc phân trang thường được sử dụng trong các trang có số lượng dữ liệu lớn, ví dụ như trang danh sách các user, trang danh sách các sản phẩm, trang danh sách các bài viết, ...

- Để phân trang trong react query hãy xem ví dụ sau:

```js title="SuperHeroListPage.js"
import axios from "axios";
import { useState } from "react";
import { useQuery } from "react-query";

function getTodos(page) {
  return axios.get(
    "https://jsonplaceholder.typicode.com/todos?_limit=10?_page=" + page
  );
}
const Todos = () => {
  const [page, setPage] = useState(1);
  const { data, isFetching, isLoading } = useQuery(["todos", page], getTodos, {
    keepPreviousData: true,
  });
  console.log("data", data);
  if (isLoading) return <div>Loading...</div>;
  return (
    <div>
      {data?.data.map((todo, i) => (
        <div key={todo.id}>{i + 1 + ". " + todo.title}</div>
      ))}
      {isFetching ? <div>Updating...</div> : null}
      <button onClick={() => setPage((old) => Math.max(old - 1, 1))}>
        Previous
      </button>
      <button
        onClick={() =>
          setPage((old) => (!data || !data.data.length ? old : old + 1))
        }
      >
        Next
      </button>
    </div>
  );
};
export default Todos;
```

## Load More

```js title="SuperHeroListPage.js"
import axios from "axios";
import { useState } from "react";
import { useInfiniteQuery, useQuery } from "react-query";

function getTodos({ pageParam = 1 }) {
  console.log({ pageParam });
  return axios.get(
    "https://jsonplaceholder.typicode.com/todos?_limit=2?_page=" + pageParam
  );
}
const Todos = () => {
  const {
    data,
    isFetching,
    isLoading,
    hasNextPage,
    fetchNextPage,
    isFetchingNextPage,
  } = useInfiniteQuery(["todos"], getTodos, {
    getNextPageParam: (_lastPage, page) => {
      // console.log({ _lastPage, page });
      if (page.length > 4) return undefined;
      return page.length + 1;
    },
  });
  // console.log("data", data);
  if (isLoading) return <div>Loading...</div>;
  return (
    <div>
      {data?.pages.map((page) => {
        return page.data.map((todo) => {
          return <div key={todo.id}>{todo.title}</div>;
        });
      })}
      <button disabled={!hasNextPage} onClick={fetchNextPage}>
        {isFetchingNextPage ? "Loading more..." : "Load More"}{" "}
      </button>
      {isFetching && !isFetchingNextPage ? "Fetching..." : null}
    </div>
  );
};
export default Todos;
```
