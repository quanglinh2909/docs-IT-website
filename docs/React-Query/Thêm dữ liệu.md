```js title="SuperHeroListPage.js"
import axios from "axios";
import { useState } from "react";
import { useInfiniteQuery, useMutation, useQuery } from "react-query";

function getTodos(data) {
  return axios.post("https://jsonplaceholder.typicode.com/todos", data);
}
const Todos = () => {
  const { mutate, isLoading, isError, isSuccess, error } = useMutation(
    getTodos,
    {
      onSuccess: (data) => {
        console.log({ data });
      },
      onError: (error) => {
        console.log({ error });
      },
    }
  );
  if (isLoading) return <div>Loading...</div>;
  if (isError) return <div>{error.message}</div>;
  if (isSuccess) return <div>Success</div>;

  return (
    <div>
      <button onClick={() => mutate({ id: 20, name: "demo" })}>Add Todo</button>
    </div>
  );
};
export default Todos;
```

## Thêm dữ liệu mới đồng thời cập nhật lại danh sách dữ liệu

```js title="SuperHeroListPage.js"
import axios from "axios";
import { useState } from "react";
import {
  useInfiniteQuery,
  useMutation,
  useQuery,
  useQueryClient,
} from "react-query";

function getTodos(data) {
  return axios.post("https://jsonplaceholder.typicode.com/todos", data);
}
const Todos = () => {
  const queryClient = useQueryClient();
  const { mutate, isLoading, isError, isSuccess, error } = useMutation(
    getTodos,
    {
      onSuccess: (data) => {
        console.log({ data });
        queryClient.invalidateQueries("todos");
      },
      onError: (error) => {
        console.log({ error });
      },
    }
  );
  if (isLoading) return <div>Loading...</div>;
  if (isError) return <div>{error.message}</div>;
  if (isSuccess) return <div>Success</div>;

  return (
    <div>
      <button onClick={() => mutate({ id: 20, name: "demo" })}>Add Todo</button>
    </div>
  );
};
export default Todos;
```

- để cập nhật lại dữ liệu khi thành công thì ta sử dụng `queryClient.invalidateQueries("todos");` để cập nhật lại dữ liệu trong ham `onSuccess`

## Thêm dữ liệu mới đồng thời cập nhật lại danh sách dữ liệu

- với ví dụ trên khi thêm mới đối tượng thì phải gửi thêm một request để lấy lại danh sách dữ liệu mới nhất, nếu dữ liệu lớn thì việc này sẽ tốn thời gian và tốn tài nguyên.
- để giải quyết vấn đề này ta sử dụng `useQueryClient` để cập nhật lại dữ liệu mà không cần gửi thêm request

````js title="SuperHeroListPage.js"
```js title="SuperHeroListPage.js"
import axios from "axios";
import { useState } from "react";
import { useInfiniteQuery, useMutation, useQuery } from "react-query";

function getTodos(data) {
  return axios.post("https://jsonplaceholder.typicode.com/todos", data);
}
const Todos = () => {
  const { mutate, isLoading, isError, isSuccess, error } = useMutation(
    getTodos,
    {
      onSuccess: (data) => {
        console.log({ data });
      },
      onError: (error) => {
        console.log({ error });
      },
    }
  );
  if (isLoading) return <div>Loading...</div>;
  if (isError) return <div>{error.message}</div>;
  if (isSuccess) return <div>Success</div>;

  return (
    <div>
      <button onClick={() => mutate({ id: 20, name: "demo" })}>Add Todo</button>
    </div>
  );
};
export default Todos;
````

## Thêm dữ liệu mới đồng thời cập nhật lại danh sách dữ liệu

```js title="SuperHeroListPage.js"
import axios from "axios";
import { useState } from "react";
import {
  useInfiniteQuery,
  useMutation,
  useQuery,
  useQueryClient,
} from "react-query";

function getTodos(data) {
  return axios.post("https://jsonplaceholder.typicode.com/todos", data);
}
const Todos = () => {
  const queryClient = useQueryClient();
  const { mutate, isLoading, isError, isSuccess, error } = useMutation(
    getTodos,
    {
      onSuccess: (data) => {
        console.log({ data });
        queryClient.invalidateQueries("todos");
      },
      onError: (error) => {
        console.log({ error });
      },
    }
  );
  if (isLoading) return <div>Loading...</div>;
  if (isError) return <div>{error.message}</div>;
  if (isSuccess) return <div>Success</div>;

  return (
    <div>
      <button onClick={() => mutate({ id: 20, name: "demo" })}>Add Todo</button>
    </div>
  );
};
export default Todos;
```

- để cập nhật lại dữ liệu khi thành công thì ta sử dụng `queryClient.invalidateQueries("todos");` để cập nhật lại dữ liệu trong ham `onSuccess`

## Thêm dữ liệu mới đồng thời cập nhật lại danh sách dữ liệu

- với ví dụ trên khi thêm mới đối tượng thì phải gửi thêm một request để lấy lại danh sách dữ liệu mới nhất, nếu dữ liệu lớn thì việc này sẽ tốn thời gian và tốn tài nguyên.
- để giải quyết vấn đề này ta sử dụng `useQueryClient` để cập nhật lại dữ liệu mà không cần gửi thêm request

```js title="SuperHeroListPage.js"
import axios from "axios";
import { useState } from "react";
import {
  useInfiniteQuery,
  useMutation,
  useQuery,
  useQueryClient,
} from "react-query";

function getTodos(data) {
  return axios.get("http://localhost:4000/friends");
}
function postTodos(data) {
  return axios.post("http://localhost:4000/friends", data);
}
const Todos = () => {
  const queryClient = useQueryClient();
  const [id, setId] = useState(21);
  const { data } = useQuery("todos", getTodos);
  const { mutate, isLoading, isError, isSuccess, error } = useMutation(
    postTodos,
    {
      onSuccess: (data) => {
        queryClient.setQueryData("todos", (oldData) => {
          return { ...oldData, data: [...oldData.data, data.data] };
        });
        setId(id + 1);
      },
      onError: (error) => {
        console.log({ error });
      },
    }
  );
  if (isLoading) return <div>Loading...</div>;
  if (isError) return <div>{error.message}</div>;
  return (
    <div>
      {data?.data.map((todo) => (
        <div key={todo.id}>{todo.name}</div>
      ))}
      <button onClick={() => mutate({ id: id, name: "demo" + id })}>
        Add Todo
      </button>
    </div>
  );
};
export default Todos;
```
