- Giả sử ta có 2 trang một trang để hiển thị danh sách user và một trang để hiển thị thông tin của từng user trong danh sách đó.
- Mà thông tin user đã được trả về ở trang danh sách user, để tránh ảnh hưởng đến trải nghiệm người dùng thì ta có thể lấy thông tin user ở trang danh sách user và truyền vào trang thông tin user.

```js title="SuperHeroListPage.js"
import axios from "axios";
import { useQuery, useQueryClient } from "react-query";
const getDataSuperHeroes = ({ queryKey }) => {
  const heroId = queryKey[1];
  console.log(queryKey);
  return axios.get("http://localhost:4000/SuperHeroes/" + heroId);
};
export const useSuperHeroData = (heroId) => {
  const queryClient = useQueryClient();
  return useQuery(["super-hero", heroId], getDataSuperHeroes, {
    initialData: () => {
      const hero = queryClient
        .getQueryData("super-hero")
        ?.data?.find((hero) => hero.id === heroId);
      return hero ? { data: hero } : undefined;
    },
  });
};
```

- nếu trong bộ nhớ cache có dữ liệu của user thì sẽ lấy dữ liệu đó ra và truyền vào trang thông tin user.
- nếu không có dữ liệu của user thì sẽ truy vấn dữ liệu của user đó.
