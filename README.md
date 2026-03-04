# Essence - minimal log viewer for macOS

## Features
- Native & Free
- Token highlighting
- Minimap
  - gutter
  - content preview
  - time of day visualization
- Lenses - tooltips for tokens with Javascript logic
  - get local date from UTC
  - call external system to get user name from user id
  - etc.
- Issue panel with filtering

## Installation

1. Download dmg archive 
2. Mount the dmg and move .app file to /Applications
3. Run the app

## Examples

### Tokens

- regex to extract ERROR, SEVERE or FATAL words:

  ```
  \b(ERROR|SEVERE|FATAL)\b
  ```

- regex to extract UTC date:

  ```
  \d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2},\d{3}[+-]\d{4}
  ```


### Lenses

- convert UTC to local time
  
    ```
    let date = new Date(match.replace(',', '.')); // Adjusting string to standard JS format
    if (isNaN(date.getTime())) {
        return "Parsing date...";
    }
    // Return local time string
    return date.toLocaleString();
    ```
- call external service

  ```
  let url = "https://xyz.com/rest/api/";

  let requestOptions = {
      method: "GET",
      headers: {
          "Accept": "application/json",
          "Authorization": "Basic BASE64 encoded creds" 
      }
  };
  
  let responseText = fetchSync(url, requestOptions);
  
  // Parse the JSON string
  let responseJson = JSON.parse(responseText);
  
  //parsing logic
  
  return 'something'

  ```


