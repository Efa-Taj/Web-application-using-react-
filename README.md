# Web-application-using-react-
a web application using React that allows users to upload images and receive real-time predictions from a pre-trained machine learning model. The application should be able to classify the uploaded images into multiple categories.
// App.js

import React, { useState } from 'react';
import axios from 'axios';

function App() {
  const [selectedFile, setSelectedFile] = useState(null);
  const [predictions, setPredictions] = useState([]);

  const handleFileChange = (e) => {
    setSelectedFile(e.target.files[0]);
  };

  const handleUpload = async () => {
    if (!selectedFile) return;

    const formData = new FormData();
    formData.append('image', selectedFile);

    try {
      const response = await axios.post('/predict', formData, {
        headers: {
          'Content-Type': 'multipart/form-data',
        },
      });
      setPredictions(response.data.predictions);
    } catch (error) {
      console.error('Error uploading image:', error);
    }
  };

  return (
    <div>
      <input type="file" onChange={handleFileChange} />
      <button onClick={handleUpload}>Upload</button>
      <div>
        {predictions.map((prediction, index) => (
          <div key={index}>
            <p>{prediction.class}</p>
            <p>{prediction.confidence}</p>
          </div>
        ))}
      </div>
    </div>
  );
}

export default App;
