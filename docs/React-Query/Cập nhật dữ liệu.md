## Tự động cập nhật lại data theo một khoảng thời gian

```js title="Todos.page.js"
import axios from "axios";
import { useQuery } from "react-query";

function getTodos() {
  return axios.get("https://jsonplaceholder.typicode.com/todos");
}
const Todos = () => {
  const { data, isLoading } = useQuery(["todos"], getTodos, {
    refetchInterval: 2000,
    refetchIntervalInBackground: true,
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

- **refetchInterval** là thời gian cập nhật lại data
- **refetchIntervalInBackground** là cập nhật data khi ngay cả khi tab đang ở trạng thái background hay không focus.
- **refetchIntervalInBackground** có giá trị mặc định là false

## Cập nhật dữ liệu bằng function

```js title="Todos.page.js"
import axios from "axios";
import { useQuery } from "react-query";

function getTodos() {
  return axios.get("https://jsonplaceholder.typicode.com/todos");
}
const Todos = () => {
  const { data, isFetching, refetch } = useQuery(["todos"], getTodos, {
    enabled: false,
  });
  console.log("data", data);
  if (isFetching) return <div>Loading...</div>;
  return (
    <div>
      <button onClick={refetch}>Get data</button>
      {data?.data.map((todo) => (
        <div key={todo.id}>{todo.title}</div>
      ))}
    </div>
  );
};
export default Todos;
```

- **refetch** là function để cập nhật lại data.
- **enabled** có giá trị mặc định là true, có gửi request lên server hay không.
- mới vào thì trang sẽ không có data.
- khi click vào button thì mới lấy data từ server về.
