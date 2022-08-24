## Cài đặt Reac-query

chạy dòng lệnh sau để cài đặt react-query

```bash
npm i @tanstack/react-query
```

```js title="App.js"
import {
  useQueryClient,
  QueryClient,
  QueryClientProvider,
} from "@tanstack/react-query";
// Create a client
const queryClient = new QueryClient();

function App() {
  return (
    // Provide the client to your App
    <QueryClientProvider client={queryClient}>
      <Todos />
    </QueryClientProvider>
  );
}
```

## Cách dùng 1

```js title="Todos.page.js"
import axios from "axios";
import { useQuery } from "react-query";

const Todos = () => {
  const { data, isLoading } = useQuery(["todos"], () =>
    axios.get("https://jsonplaceholder.typicode.com/todos")
  );
  if (isLoading) return <div>Loading...</div>;
  return (
    <div>
      {data.data.map((todo) => (
        <div key={todo.id}>{todo.title}</div>
      ))}
    </div>
  );
};
export default Todos;
```

## Cách dùng 2

```js title="Todos.page.js"
import axios from "axios";
import { useQuery } from "react-query";

function getTodos() {
  return axios.get("https://jsonplaceholder.typicode.com/todos");
}
const Todos = () => {
  const { data, isLoading } = useQuery(["todos"], getTodos);

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
