- trong thực tế dữ liệu giữa backend và frontend không giống nhau, nên cần phải chuyển đổi dữ liệu trước khi hiển thị lên UI. Ví dụ như dữ liệu trả về từ backend có dạng:

```js
{
  "id": 1,
  "title": "delectus aut autem",
  "completed": false
}
```

- ở frontend thì dữ liệu sẽ được chuyển đổi thành dạng:

```js
{
  "id": 1,
  "title": "delectus aut autem",
  "isCompleted": false
}
```

- để chuyển đổi dữ liệu trả về từ backend thì sử dụng **select**:

  ```js
  useQuery("super-heroes", getDataSuperHeroes, {
    select: (data) => {
      //logic sử lý
      return data.data;
    },
  });
  ```

  - **select** là function để chuyển đổi dữ liệu trả về từ backend.
  - **data** là dữ liệu trả về từ backend.
