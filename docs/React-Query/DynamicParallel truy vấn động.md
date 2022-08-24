- React query hỗ trợ việc truy vấn nhiều request mà chưa biết cụ thể bao nhiêu request
- Ví dụ: truy vấn danh sách các user và truy vấn thông tin của từng user trong danh sách đó
  mà chưa biết trước số lượng user trong danh sách đó là bao nhiêu.

- Để truy vấn nhiều request mà chưa biết cụ thể bao nhiêu request thì sử dụng **useQueries**:

```js title="SuperHeroHook.js"
import axios from "axios";
import { useQueries } from "react-query";
const fetchSuperHeroes = (heroId) => {
  return axios.get("http://localhost:4000/SuperHeroes/" + heroId);
};
const DynamicParallelPage = ({ heroIds }) => {
  const result = useQueries(
    heroIds.map((heroId) => {
      return {
        queryKey: ["super-hero-", heroId],
        queryFn: () => fetchSuperHeroes(heroId),
      };
    })
  );
  console.log({ result });
  return <div></div>;
};
export default DynamicParallelPage;
```

**result** sẽ trả về dưới dạng mảng
