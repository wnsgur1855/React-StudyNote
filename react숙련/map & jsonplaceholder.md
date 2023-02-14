원래 있던 배열의 개수만큼 return을 하는데 가공해서 return한다
```js
import { useEffect, useState } from 'react';

import './App.css';

  

function App() {

  const [data, setData] = useState([]);

  

  useEffect(() => {

    fetch('https://jsonplaceholder.typicode.com/posts')

      .then((response) => response.json())

      .then((json) => {

        setData([...json]); //----->새로운 값이 바꼈구나

        return console.log(json);

      }, []);

    return (

      <>

        <h3>JSONPlaceholder DATA</h3>

        {data.map((item) => {

          return (

            <div style={{ border: '1px solid black' }}>

              <ui>

                <ui>{item.userId}</ui>

                <ui>{item.id}</ui>

                <ui>{item.title}</ui>

                <ui>{item.body}</ui>

              </ui>

            </div>

          );

        })}

      </>

    );

  });

}

  

export default App;
```


