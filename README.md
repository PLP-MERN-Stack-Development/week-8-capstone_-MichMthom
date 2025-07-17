const express = require('express');
const mongoose = require('mongoose');

mongoose.connect('mongodb:                                                                         

const Todo = mongoose.model('//localhost/todos', { useNewUrlParser: true, useUnifiedTopology: true });

const Todo = mongoose.model('Todo', {
  title: String,
  description: String,
  completed: Boolean
});

const app = express();

app.get('/todos', async (req, res) => {
  const todos = await Todo.find();
  res.json(todos);
});

app.post('/todos', async (req, res) => {
  const todo = new Todo(req.body);
  await todo.save();
  res.json(todo);
});

// ...
import React, { useState, useEffect } from 'react';
import axios from 'axios';

function TodoList() {
  const [todos, setTodos] = useState([]);

  useEffect(() => {
    axios.get('/todos')
      .then(response => {
        setTodos(response.data);
      })
      .catch(error => {
        console.error(error);
      });
  }, []);

  return (
    <ul>
      {todos.map(todo => (
        <li key={todo._id}>{todo.title}</li>
      ))}
    </ul>
  );
}

function TodoForm() {
  const [title, setTitle] = useState('');
  const [description, setDescription] = useState('');

  const handleSubmit = (event) => {
    event.preventDefault();
    axios.post('/todos', { title, description })
      .then(response => {
        console.log(response.data);
      })
      .catch(error => {
        console.error(error);
      });
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" value={title} onChange={(event) => setTitle(event.target.value)} />
      <textarea value={description} onChange={(event) => setDescription(event.target.value)} />
      <button type="submit">Create Todo</button>
    </form>
  );
}
