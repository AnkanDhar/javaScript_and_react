## CRUD operation using react.js + axios

```jsx
function PostsCrud() {
  const [posts, setPosts] = React.useState([]);
  const [title, setTitle] = React.useState("");
  const [body, setBody] = React.useState("");
  const [editId, setEditId] = React.useState(null);

  // READ
  React.useEffect(() => {
    axios
      .get("https://jsonplaceholder.typicode.com/posts?_limit=5")
      .then((res) => setPosts(res.data));
  }, []);

  // CREATE
  const createPost = () => {
    axios
      .post("https://jsonplaceholder.typicode.com/posts", {
        title,
        body,
        userId: 1,
      })
      .then((res) => {
        setPosts([res.data, ...posts]);
        setTitle("");
        setBody("");
      });
  };

  // UPDATE
  const updatePost = () => {
    axios
      .put(`https://jsonplaceholder.typicode.com/posts/${editId}`, {
        title,
        body,
      })
      .then((res) => {
        setPosts(
          posts.map((p) => (p.id === editId ? res.data : p))
        );
        setEditId(null);
        setTitle("");
        setBody("");
      });
  };

  // DELETE
  const deletePost = (id) => {
    axios
      .delete(`https://jsonplaceholder.typicode.com/posts/${id}`)
      .then(() => {
        setPosts(posts.filter((p) => p.id !== id));
      });
  };

  const startEdit = (post) => {
    setEditId(post.id);
    setTitle(post.title);
    setBody(post.body);
  };

  return (
    <div>
      <input
        placeholder="Title"
        value={title}
        onChange={(e) => setTitle(e.target.value)}
      />
      <textarea
        placeholder="Body"
        value={body}
        onChange={(e) => setBody(e.target.value)}
      />

      {editId ? (
        <button onClick={updatePost}>Update</button>
      ) : (
        <button onClick={createPost}>Create</button>
      )}

      {posts.map((post) => (
        <div key={post.id}>
          <h4>{post.title}</h4>
          <p>{post.body}</p>
          <button onClick={() => startEdit(post)}>Edit</button>
          <button onClick={() => deletePost(post.id)}>Delete</button>
        </div>
      ))}
    </div>
  );
}
```

---

## Sending data From Child Component to Parent Component 

***Parent Component***
```jsx
import React, { useState } from "react";
import Child from "./ChildComponent";

const Parent = () => {
    const [name, setName] = useState("");

    const handleDataFromChild = (str) => {
        return setName(str);
    }

    return (
        <>
            <h4>Parent Component</h4>
            <h4> Name from parent Component : {name}</h4>
            <Child sendToParent={handleDataFromChild} />
        </>
    )
}

export default Parent;
```

**Child Component:** 
```jsx
import React, { useState } from "react";

const Child = (props) => {
    const [name, setName] = useState("");
    return (
        <>  
        <div>
            <input type="text" name="name" value={name} onChange={e => setName(e.target.value)} />
        </div>
            <button onClick={() => props.sendToParent(name)}>SendToParent</button>
        </>
    )
}

export default Child;
```
---
