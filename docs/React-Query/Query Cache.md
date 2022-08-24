- Mặc định các request sẽ được lưu trong cache trong 5 phút
- khi page được load lần đầu tiện thì data sẽ được lưu trong bô nhớ cache với keys là key mình truyền vào
  khi này ta sẽ sử dụng isLoadding để hiển thị loading
- khi page được load lần thứ 2 hoặc tiêu điểm của trang web thay đổi thì react query sẽ gửi request lên server để lấy dữ liệu mới đồng thời so sánh dữ liệu mới với dữ liệu cũ nếu dữ liệu mới giống dữ liệu cũ thì sẽ không cập nhật lại dữ liệu, nếu dữ liệu mới khác dữ liệu cũ thì sẽ cập nhật lại dữ liệu.
  khi này ta không thể sử dụng isLoadding để hiển thị loading vì nó cập nhật trọng nền nên ta sẽ sử dụng isFetching để hiển thị loading
- Nếu muốn thay đổi thời gian lưu cache thì có thể dùng hàm `cacheTime` để thay đổi thời gian lưu cache

```js title="Todos.page.js"
import axios from "axios";
import { useQuery } from "react-query";

function getTodos() {
  return axios.get("https://jsonplaceholder.typicode.com/todos");
}
const Todos = () => {
  const { data, isLoading } = useQuery(["todos"], getTodos, {
    cacheTime: 2000,
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

- Khi chuyển tab sau 2 giây thì data sẽ được xóa khỏi bô nhớ cache
- nếu không chuyển tab thì request sẽ dữ nguyên

## Thiết lập thời gian gửi lại request

- khi chuyên tab rồi chuyển lại hay khi mất tiêu điểm rồi focus lại thì react query sẽ gửi lại request lên server để lấy dữ liệu mới.
- Nếu bạn muốn tránh việc gửi lại request nhiều lần thì có thể thiết lập lại thời gian gửi lại request bằng cách dùng hàm `staleTime`.

```js title="Todos.page.js"
import axios from "axios";
import { useQuery } from "react-query";

function getTodos() {
  return axios.get("https://jsonplaceholder.typicode.com/todos");
}
const Todos = () => {
  const { data, isLoading } = useQuery(["todos"], getTodos, {
    staleTime: 3000,
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

- **staleTime** là thời gian mà react query sẽ gửi lại request lên server để lấy dữ liệu mới.
- mặc định thời gian gửi lại request là 0
- thời gian sẽ được tính kể từ khi request được lấy về thành công

## Tắt tự động refetch data

```js title="Todos.page.js"
import axios from "axios";
import { useQuery } from "react-query";

function getTodos() {
  return axios.get("https://jsonplaceholder.typicode.com/todos");
}
const Todos = () => {
  const { data, isLoading } = useQuery(["todos"], getTodos, {
    refetchOnMount: false,
    refetchOnWindowFocus: false,
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

- **refetchOnMount** là tắt tự động refetch data khi component được mount, giá trị mặc định là true.
- **refetchOnWindowFocus** là tắt tự động refetch data khi mất tiêu điểm rồi focus, lại giá trị mặc định là true.
- **refetchOnMount** và **refetchOnWindowFocus** có giá trị là true,false hoặc 'always'
