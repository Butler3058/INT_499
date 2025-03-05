# INT_499
import { Link } from "react-router-dom";

const Navbar = () => {
  return (
    <nav>
      <ul>
        <li><Link to="/">StreamList</Link></li>
        <li><Link to="/movies">Movies</Link></li>
        <li><Link to="/cart">Cart</Link></li>
        <li><Link to="/about">About</Link></li>
      </ul>
    </nav>
  );
};

export default Navbar;
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
import Navbar from "./components/Navbar";
import StreamList from "./components/StreamList";
import Movies from "./components/Movies";
import Cart from "./components/Cart";
import About from "./components/About";

function App() {
  return (
    <Router>
      <Navbar />
      <Routes>
        <Route path="/" element={<StreamList />} />
        <Route path="/movies" element={<Movies />} />
        <Route path="/cart" element={<Cart />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </Router>
  );
}

export default App;
import { useState } from "react";

const StreamList = () => {
  const [list, setList] = useState([]);
  const [input, setInput] = useState("");

  const addToList = () => {
    if (input) {
      setList([...list, input]);
      setInput("");
    }
  };

  return (
    <div>
      <h1>My StreamList</h1>
      <input
        type="text"
        value={input}
        onChange={(e) => setInput(e.target.value)}
        placeholder="Enter movie/show name"
      />
      <button onClick={addToList}>Add</button>
      <ul>
        {list.map((item, index) => (
          <li key={index}>{item}</li>
        ))}
      </ul>
    </div>
  );
};

export default StreamList;
nav ul {
  display: flex;
  list-style: none;
  gap: 20px;
  background: #333;
  padding: 10px;
}

nav ul li a {
  color: white;
  text-decoration: none;
}

input {
  margin: 10px;
  padding: 5px;
}

button {
  padding: 5px 10px;
  background: blue;
  color: white;
  border: none;
  cursor: pointer;
}