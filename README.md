# INT_499

import { useState } from "react";
import { FaPlus, FaTrash, FaLock, FaUnlock } from "react-icons/fa";
import CryptoJS from "crypto-js"; // Import Crypto library

const StreamList = () => {
  const [list, setList] = useState([]);
  const [input, setInput] = useState("");
  const [hashtags, setHashtags] = useState("");
  const [message, setMessage] = useState("");
  const [locked, setLocked] = useState(false);
  const [password, setPassword] = useState("");

  // Function to generate a random salt
  const generateSalt = () => {
    return Math.random().toString(36).substring(2, 8); // 6-character random string
  };

  // Function to salt and hash hashtags
  const saltAndHashHashtags = (tags) => {
    return tags.map((tag) => {
      const salt = generateSalt();
      const saltedHashtag = tag.trim() + salt; // Append salt
      const hashedTag = CryptoJS.SHA256(saltedHashtag).toString(); // Hashing (optional)
      return { original: tag.trim(), hashed: hashedTag };
    });
  };

  // Add a new item to the list
  const addToList = () => {
    if (input.trim() === "") {
      setMessage("Please enter a movie or show name!");
      return;
    }

    const tagArray = hashtags.split(",").map((tag) => tag.trim());
    const saltedHashtags = saltAndHashHashtags(tagArray); // Process hashtags

    const newItem = {
      name: input,
      hashtags: saltedHashtags, // Store the salted and hashed hashtags
    };

    setList([...list, newItem]);
    setInput("");
    setHashtags("");
    setMessage(`"${input}" has been added to your list!`);

    // Hide the message after 3 seconds
    setTimeout(() => setMessage(""), 3000);
  };

  // Remove an item from the list
  const removeFromList = (index) => {
    const newList = list.filter((_, i) => i !== index);
    setList(newList);
  };

  return (
    <div>
      <h1>My StreamList</h1>

      {message && <p style={{ color: "green" }}>{message}</p>}

      <input
        type="text"
        value={input}
        onChange={(e) => setInput(e.target.value)}
        placeholder="Enter movie/show name"
      />
      <input
        type="text"
        value={hashtags}
        onChange={(e) => setHashtags(e.target.value)}
        placeholder="Enter hashtags (comma separated)"
      />
      <button onClick={addToList}>
        <FaPlus /> Add
      </button>

      <ul>
        {list.map((item, index) => (
          <li key={index}>
            <span>
              {item.name}{" "}
              <span style={{ color: "blue" }}>
                {item.hashtags.map((tagObj, i) => (
                  <span key={i}>#{tagObj.original} </span> // Display only original hashtags
                ))}
              </span>
            </span>
            <button onClick={() => removeFromList(index)} style={{ color: "red" }}>
              <FaTrash />
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default StreamList;