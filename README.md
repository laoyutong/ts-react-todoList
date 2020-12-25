# ts-react-todoList
用react+ts完成简单的todoList

```js
import React, { useEffect, useRef, useState } from "react";
import "./App.css";
import produce from "immer";

interface TodoList {
  id: string;
  content: string;
}

function App() {
  const inputRef = useRef<HTMLInputElement>(null);

  const [todoList, setTodoList] = useState<TodoList[]>([]);

  const deleteTodoItem = (id: string): void => {
    setTodoList(
      produce(todoList, (draft) => draft.filter((todo) => todo.id !== id))
    );
  };

  useEffect(() => {
    const handleEnter = (e: KeyboardEvent): void => {
      if (e.code === "Enter") {
        setTodoList(
          produce(todoList, (draft) => {
            draft.unshift({
              id: new Date().getTime().toString(),
              content: inputRef.current.value,
            });
          })
        );
        inputRef.current.value = "";
      }
    };
    document.addEventListener("keydown", handleEnter);
    return () => document.removeEventListener("keydown", handleEnter);
  }, [todoList]);

  return (
    <div className="App">
      <h2 className="title">To-Do List</h2>
      <div className="container">
        <input type="text" ref={inputRef} />
        <div className="to-do-list">
          {todoList.map(({ id, content }) => (
            <div className="to-do-item" key={id}>
              <div className="content">{content}</div>
              <div className="opt" onClick={() => deleteTodoItem(id)}>
                x
              </div>
            </div>
          ))}
        </div>
      </div>
    </div>
  );
}

export default App;
```