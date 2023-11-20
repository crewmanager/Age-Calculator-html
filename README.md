# Age-Calculator-html

`main.mo`

```js
actor {
  type TimeData = {
    day : Nat32;
    month : Nat32;
    year : Nat32;
  };

  type TimeDataInput = {
    date : Nat32;
    month : Nat32;
    year : Nat32;
  };

  public func ageCalculate(currentData : TimeDataInput, birthData : TimeDataInput) : async TimeData {
    var years = currentData.year - birthData.year;
    var months : Nat32 = 0;
    var days : Nat32 = 0;

    if (currentData.month >= birthData.month) {
      months := currentData.month - birthData.month;
    } else {
      months := birthData.month - currentData.month;
      years -= 1;
      months := 12 - months;
    };

    if (currentData.date >= birthData.date) {
      days := currentData.date - birthData.date;
    } else {
      days := birthData.date - currentData.date;
      days := days + (30 - currentData.date);
      months -= 1;
    };

    var data = {
      day = days;
      month = months;
      year = years;
    };
    return data;
  };
};

```

`main.css`

```css
body {
  font-family: "Poppins", sans-serif;
  background: linear-gradient(
    135deg,
    #6dd5ed,
    #2193b0
  ); /* Gradient background */
  margin: 0;
  padding: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  color: #343a40;
  line-height: 1.6;
  overflow: hidden;
}

img {
  max-width: 50vw;
  max-height: 25vw;
  display: block;
  margin: 20px auto;
  border-radius: 10px;
  transition: transform 0.3s ease;
}

img:hover {
  transform: scale(1.05);
}

form {
  display: flex;
  justify-content: center;
  gap: 1em;
  flex-flow: row wrap;
  max-width: 40vw;
  margin: 20px auto;
  align-items: baseline;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  padding: 20px;
  border-radius: 10px;
  background: #fff;
  animation: fadeIn 0.5s ease-in-out;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

button {
  padding: 10px 25px;
  margin: 10px 0;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-weight: bold;
  transition: background-color 0.3s ease, transform 0.2s ease,
    box-shadow 0.3s ease;
}

button:hover {
  background-color: #0056b3;
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.calculator {
  box-sizing: border-box;
  background-color: #fff;
  border-radius: 15px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
  padding: 40px;
  text-align: center;
  max-width: 400px;
  width: 100%;
  animation: slideUp 0.5s ease-in-out;
}

@keyframes slideUp {
  from {
    transform: translateY(20px);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

h1 {
  color: #070707;
  margin-bottom: 20px;
  font-size: 2rem;
  text-shadow: 1px 1px 2px #adb5bd;
}

.input-container {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  margin-top: 20px;
}

.input-container label {
  margin-bottom: 10px;
  font-weight: bold;
  color: #007bff;
}

.input-container input {
  width: 70%;
  padding: 12px;
  border: 2px solid #ccc;
  border-radius: 5px;
  outline: none;
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

.input-container input:focus {
  border-color: #007bff;
  box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.25);
}

#result {
  margin-top: 20px;
  word-wrap: break-word;
  background-color: #e9ecef;
  padding: 15px;
  border-radius: 5px;
  box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.1);
}

```
`index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width" />
    <title>Age Calculator</title>
    <base href="/" />
    <link rel="icon" href="favicon.ico" />
    <link type="text/css" rel="stylesheet" href="main.css" />
  </head>
  <body>
    <main>
      <div class="calculator">
        <h1>Age Calculator</h1>
        <div class="input-container">
          <label for="birthdate">Enter your birthdate:</label>
          <input type="date" id="birthdate" />
        </div>
        <button id="calculate-button">Calculate Age</button>
        <div id="result"></div>
      </div>
    </main>
  </body>
</html>

```
`index.js`

```js
import { todo_backend } from "../../declarations/todo_backend";

const calculateButton = document.getElementById("calculate-button");
const result = document.getElementById("result");

calculateButton.addEventListener("click", async () => {
  try {
    result.textContent = "";
    const birthTime = new Date(document.getElementById("birthdate").value);
    let currentTime = new Date();

    let currentYear = currentTime.getFullYear();
    let birthYear = birthTime.getFullYear();
    let currentMonth = currentTime.getMonth();
    let birthMonth = birthTime.getMonth();
    let currentDate = currentTime.getDate();
    let birthDate = birthTime.getDate();

    let currentData = {
      date: currentDate,
      month: currentMonth,
      year: currentYear,
    };

    let birthData = {
      date: birthDate,
      month: birthMonth,
      year: birthYear,
    };

    let data = await todo_backend.ageCalculate(currentData, birthData);

    result.textContent = `${data.year} years ${data.month} months ${data.day} days`;
  } catch (error) {
    result.textContent = error;
  }
});

```
