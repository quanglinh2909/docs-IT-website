```js title="SuperHeroHook.js"
import axios from "axios";
import { useQuery } from "react-query";
const fetchSuperHeroes = () => {
  return axios.get("http://localhost:4000/SuperHeroes");
};
const fetchFriends = () => {
  return axios.get("http://localhost:4000/friends");
};
const ParallelQueriesPage = () => {
  const { data: data1 } = useQuery("super-heroes", fetchSuperHeroes);
  const { data: data2 } = useQuery("friends", fetchFriends);
  return <div></div>;
};
export default ParallelQueriesPage;
```
