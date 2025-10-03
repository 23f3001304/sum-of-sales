## Simple Sales Data Summary

### Summary

This project is a minimalist, single-page web application designed to demonstrate fetching, processing, and displaying data from a local CSV file. It calculates the total sales from `data.csv`, dynamically updates the page title with the sales summary, and presents the sum prominently on the page using Bootstrap 5 for a clean and responsive user interface.

### Setup

To set up and run this application locally, follow these steps:

1.  **Create `index.html`**: Save the following HTML content as `index.html` in your project directory. This file includes the necessary HTML structure, Bootstrap 5 styling, and embedded JavaScript for data fetching and processing.

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="utf-8"/>
        <meta name="viewport" content="width=device-width, initial-scale=1"/>
        <title>Sales Summary</title>
        <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous"/>
    </head>
    <body>
        <div class="container mt-5">
            <h1 class="mb-4">Sales Data Summary</h1>
            <div class="card">
                <div class="card-body">
                    <h5 class="card-title">Total Sales: <span id="total-sales" class="badge bg-primary">Loading...</span></h5>
                </div>
            </div>
        </div>

        <script>
            document.addEventListener('DOMContentLoaded', () => {
                fetch('data.csv')
                    .then(response => {
                        if (!response.ok) {
                            throw new Error(`HTTP error! status: ${response.status}`);
                        }
                        return response.text();
                    })
                    .then(data => {
                        const lines = data.trim().split('\n');
                        if (lines.length < 2) {
                            throw new Error('CSV is empty or only has headers.');
                        }
                        const headers = lines[0].split(',');
                        const salesColumnIndex = headers.indexOf('sales');
                        let totalSales = 0;

                        if (salesColumnIndex === -1) {
                            throw new Error('CSV does not contain a "sales" column.');
                        }

                        for (let i = 1; i < lines.length; i++) {
                            const values = lines[i].split(',');
                            const salesValue = parseFloat(values[salesColumnIndex]);
                            if (!isNaN(salesValue)) {
                                totalSales += salesValue;
                            }
                        }

                        document.getElementById('total-sales').textContent = `$${totalSales.toFixed(2)}`;
                        document.title = `Sales Summary $${totalSales.toFixed(2)}`; 

                    })
                    .catch(error => {
                        console.error('Error fetching or processing CSV:', error);
                        document.getElementById('total-sales').textContent = 'Error loading data.';
                        document.title = 'Sales Summary - Error';
                    });
            });
        </script>
    </body>
    </html>
    ```

2.  **Create `data.csv`**: In the *same directory* as `index.html`, create a file named `data.csv` and populate it with the following content:

    ```csv
    product,sales
    A,100
    B,200
    C,325
    ```

### Usage

To use the application:

1.  Navigate to your project directory.
2.  Open `index.html` in your web browser.

The page will load, fetch `data.csv`, calculate the sum of the "sales" column, update the browser tab title to "Sales Summary" followed by the total, and display the calculated total sales on the page.

### Code Explanation

*   **`index.html`**:
    *   **HTML Structure**: Sets up a standard HTML5 document, including `meta` tags for character set and responsive viewport. It loads Bootstrap 5 via a CDN link for styling, providing a modern and responsive design. The body contains a `div` with Bootstrap's `container` and `mt-5` classes for central alignment and top margin. A `card` component visually encapsulates the "Total Sales" heading, where a `span` element with `id="total-sales"` is designated to display the dynamically calculated sum.
    *   **JavaScript Logic**: An inline `<script>` tag contains the core application logic.
        *   It uses `document.addEventListener('DOMContentLoaded', ...)` to ensure the script runs only after the entire HTML document has been loaded and parsed.
        *   The `fetch` API is employed to asynchronously retrieve the `data.csv` file, treating it as a local resource.
        *   Upon a successful `fetch`, the CSV text content is processed:
            *   It splits the text into individual lines and then parses the header row to identify the `sales` column by its name.
            *   It iterates through the data rows, converting the value in the `sales` column to a floating-point number and accumulating the sum.
            *   Finally, the calculated `totalSales` is inserted into the `textContent` of the `#total-sales` span (formatted to two decimal places).
            *   The `document.title` is dynamically updated to display "Sales Summary" along with the final `totalSales`, fulfilling the dynamic title requirement.
        *   Robust error handling is included to catch potential issues during the fetch operation or CSV parsing, logging errors to the console, and updating the UI accordingly.

*   **`data.csv`**: A simple comma-separated values file that serves as the data source for the application. It contains product identifiers and their corresponding sales figures, specifically with `product` and `sales` columns.

### License

This project is licensed under the MIT License.

```
MIT License

Copyright (c) [Year] [Your Name or Organization]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```