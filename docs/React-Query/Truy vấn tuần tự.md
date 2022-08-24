- việc truy vấn tuần tự là truy vấn một request sau khi request trước đó hoàn thành
  là một điều rất thường xuyên trong các ứng dụng thực tế.

- vậy trong react query thì làm sao để truy vấn tuần tự?
- hãy xem ví dụ sau:

```js title="SuperHeroHook.js"
//DependentQueries.page.js
import axios from "axios";
import { useQuery } from "react-query";
const fetchByEmail = (email) => {
  return axios.get("http://localhost:4000/users/" + email);
};
const fetchcoursesByChanleId = (chanleId) => {
  return axios.get("http://localhost:4000/chanels/" + chanleId);
};

const DependentQueriesPage = ({ email }) => {
  const { data: user } = useQuery(["user", email], () => fetchByEmail(email));
  const chanelId = user?.data.channelId;
  console.log(chanelId);
  const { data: chanel } = useQuery(
    ["chanel", chanelId],
    () => fetchcoursesByChanleId(chanelId),
    {
      enabled: !!chanelId,
    }
  );
  console.log({ user, chanel });

  return <div></div>;
};
export default DependentQueriesPage;
```

- trong ví dụ trên, chúng ta sẽ truy vấn tuần tự 2 request:
  - request 1: truy vấn thông tin của user theo email
  - request 2: truy vấn thông tin của chanel theo chanelId
- khi truy vấn một chưa hoàn thành thì request 2 sẽ không được thực hiện
  vì vậy chúng ta sẽ sử dụng **enabled** để kiểm tra điều kiện truy vấn request 2
- **enabled** có là một giá trị boolean sẽ thực hiện nếu là true và ngược lại
