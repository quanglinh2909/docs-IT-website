- Để biết trạng thái request trong useQuery ta có thể sử dụng 2 cách sau:
  - Cách 1: Sử dụng biến isError, error.
  - Cách 2: Sử dụng function onError, onSuccess.

## Cách 1: Sử dụng biến isError, error, isSuccess, isLoading

```js title="Todos.page.js"
import axios from "axios";
import { useQuery } from "react-query";

function getTodos() {
  return axios.get("https://jsonplaceholder.typicode.com/todos");
}
const Todos = () => {
  const { data, isLoading, isSuccess, isError, error, isFetched, isFetching } =
    useQuery(["todos"], getTodos);
  if (isLoading) return <div>Loading...</div>;
  if (isError) return <div>Error: {error.message}</div>;
  if (isSuccess) {
    return (
      <div>
        {data?.data.map((todo) => (
          <div key={todo.id}>{todo.title}</div>
        ))}
      </div>
    );
  }
};
export default Todos;
```

- khi request bị lỗi thì mặc định request sẽ được gửi 4 lần mới trả về lỗi
- **isError**: trả về true khi request bị lỗi
- **error**: trả về lỗi nếu không có lỗi sẽ trả về null
- **isSuccess**: trả về true khi request thành công
- **isLoading**: trả về true khi request đang chờ
- **isFetched**: trả về true khi request đã được tìm nạo thành công
- **isFetching**: trả về true khi request đang được gửi false khi request đã được gửi xong
- **data**: trả về dữ liệu

## Cách 2: Sử dụng function onError, onSuccess

```js title="Todos.page.js"
import axios from "axios";
import { useQuery } from "react-query";

function getTodos() {
  return axios.get("https://jsonplaceholder.typicode.com/todos");
}
const Todos = () => {
  const { data, isLoading } = useQuery(["todos"], getTodos, {
    onSuccess: (data) => {
      console.log(data);
    },
    onError: (error) => {
      console.log(error);
    },
  });
  console.log("data", data);
  if (isLoading) return <div>Loading...</div>;
  return (
    <div>
      {data?.data.map((todo) => (
        <div key={todo.id}>{todo.title}</div>
      ))}
    </div>
  );
};
export default Todos;
```

- **onSuccess**: hàm này sẽ được gọi khi request thành công.
- **onError**: hàm này sẽ được gọi khi request bị lỗi.
