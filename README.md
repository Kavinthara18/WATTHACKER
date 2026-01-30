# WATTHACKER


BACKEND
import logo from './logo.svg';
import './App.css';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}

export default App;
import { useEffect, useState } from "react";

function App() {
  const [data, setData] = useState([]);

  useEffect(() => {
    fetch("http://localhost:5000/data")
      .then(response => response.json())
      .then(result => setData(result));
  }, []);

  return (
    <div style={{ padding: "20px", fontFamily: "Arial" }}>
      <h1>☀️ Solar Monitoring Dashboard</h1>

      {data.map((item, index) => (
        <div key={index} style={{
          border: "1px solid #ccc",
          margin: "10px",
          padding: "10px",
          borderRadius: "8px"
        }}>
          <p><b>Solar Voltage:</b> {item.solarVoltage} V</p>
          <p><b>Battery Voltage:</b> {item.batteryVoltage} V</p>
          <p><b>Current:</b> {item.current} A</p>
          <p><b>Time:</b> {new Date(item.time).toLocaleString()}</p>
        </div>
      ))}
    </div>
  );
}

export default App;


const express = require("express");
const mongoose = require("mongoose");
const cors = require("cors");

const app = express();
app.use(cors());
app.use(express.json());

// 🔗 MongoDB Connection (LOCAL)
mongoose.connect("mongodb://localhost:27017/solarDB", {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});

const SensorSchema = new mongoose.Schema({npx create-react-app frontend

  solarVoltage: Number,
  batteryVoltage: Number,
  current: Number,
  time: { type: Date, default: Date.now }
});

const Sensor = mongoose.model("Sensor", SensorSchema);

// API to receive data from Arduino
app.post("/data", async (req, res) => {
  await new Sensor(req.body).save();
  res.send("Data saved");
});

// API to send data to frontend
app.get("/data", async (req, res) => {
  const data = await Sensor.find().sort({ time: -1 });
  res.json(data);
});

app.listen(5000, () => {
  console.log("Backend running on http://localhost:5000");
});



 
