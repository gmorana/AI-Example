---
name: fibonacci-series
description: Generate the Fibonacci series up to a specified number of terms. The Fibonacci sequence starts with 0 and 1, and each subsequent number is the sum of the previous two. Use when asked to generate Fibonacci numbers, display a Fibonacci sequence, or create interactive visualizations of the series.
technology: Bash, PowerShell, JavaScript, HTML, CSS

use_cases:
  - "What are the first 10 Fibonacci numbers?"
  - "Generate a Fibonacci series with 15 terms"
  - "Can you give me the Fibonacci sequence up to the 20th term?"
  - "Create a webpage that displays the Fibonacci series up to a certain number of terms"
---

## Overview

The Fibonacci series is a mathematical sequence where each number is the sum of the two preceding ones, typically starting with 0 and 1. This skill provides implementations in multiple scripting languages to generate the sequence efficiently.

## Usage

### Bash Implementation

Generate Fibonacci series using Bash:

```bash
n=<number_of_terms>
a=0
b=1
for ((i=0; i<n; i++)); do
  echo -n "$a "
  fn=$((a + b))
  a=$b
  b=$fn
done
echo
```

### PowerShell Implementation

Generate Fibonacci series using PowerShell:

```powershell
$n = <number_of_terms>
$a = 0
$b = 1
for ($i = 0; $i -lt $n; $i++) {
  Write-Host -NoNewline "$a "
  $fn = $a + $b
  $a = $b
  $b = $fn
}
Write-Host
```

### JavaScript Implementation

Generate Fibonacci series using JavaScript:

```javascript
function generateFibonacci(terms) {
  const fibonacci = [];
  let a = 0,
    b = 1;

  for (let i = 0; i < terms; i++) {
    fibonacci.push(a);
    const temp = a + b;
    a = b;
    b = temp;
  }

  return fibonacci;
}

// Calculate statistics
function calculateStats(fibonacci) {
  const sum = fibonacci.reduce((acc, num) => acc + num, 0);
  const average = (sum / fibonacci.length).toFixed(2);
  const lastTerm = fibonacci[fibonacci.length - 1];

  return { sum, average, lastTerm };
}

// Example usage
const sequence = generateFibonacci(10);
console.log(sequence); // [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

### HTML5 & Tailwind CSS Implementation

An interactive web application has been created with the following features:

**Location**: [index.html](../index.html)

**Features**:

- 📱 **Responsive Design** — Works seamlessly on all screen sizes
- 🎨 **Modern Styling** — Tailwind CSS with gradient backgrounds and color-coded statistics
- 🎯 **Interactive Input** — Generate sequences for 1-100 terms
- 📊 **Real-time Statistics** — Displays total terms, last term, sum, and average
- ✅ **Input Validation** — Prevents invalid entries with error messages
- ⌨️ **Keyboard Support** — Press Enter to generate
- 🔄 **Auto-generation** — Displays results on page load

**HTML Structure**:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Fibonacci Series Generator</title>
    <script src="https://cdn.tailwindcss.com"></script>
  </head>
  <body>
    <!-- Gradient background -->
    <!-- Input section with number field and generate button -->
    <!-- Results display with sequence and statistics cards -->
    <!-- Error handling section -->
    <!-- Information about Fibonacci -->
    <script>
      // JavaScript functions for generation and display
    </script>
  </body>
</html>
```

## Instructions

### For Bash/PowerShell:

Replace `<number_of_terms>` with the desired number of terms in the Fibonacci sequence.

### For JavaScript:

Call the `generateFibonacci(n)` function with the number of terms. The function returns an array of Fibonacci numbers.

### For Web Application:

1. Open [index.html](../index.html) in a web browser
2. Enter the desired number of terms (1-100)
3. Click "Generate" or press Enter
4. View the sequence and statistics

### Algorithm (used across all implementations):

1. Initialize the first two numbers (0 and 1)
2. Loop for the specified number of terms
3. Output the current number
4. Calculate the next Fibonacci number by adding the previous two
5. Shift the values forward for the next iteration
