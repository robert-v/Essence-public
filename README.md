# Essence - free minimal and focused log viewer for macOS

<img width="1604" height="1121" alt="image" src="https://github.com/user-attachments/assets/2b395967-86f7-4893-87cd-30eef8937601" />


macOS 14+, universal .app in .dmg archive

notarized, sandboxed, entitlements: outgoing connections (for lenses 'fetchSync'), user selected file read/write for yaml import/export

## Features
- Native, lightweight (~3 MB) & Free
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
- Import/export lenses and profiles to yaml
   - Feel free to contribute your own profiles and lenses!

## Installation

1. Download dmg archive (from releases on the right side of this page)
2. Mount the dmg and move .app file to /Applications
3. Run the app

## My other tools
[ProcessSpy - Advanced process monitor for Mac](https://process-spy.app)

[Restretto - Minimal REST client for Mac](https://restretto.app)

## Docs

### Regex format
The tool works with Apple's NSRegularExpression which is functionally identical to PCRE (Perl Compatible Regular Expressions) for 99% of common use cases. To test your regex, use site like [regex101.com](https://regex101.com) and set flavor to PCRE2 or Python. 

### Token rules
  Token rules specify which parts of the text will be highlighted, whether they will be visible in minimap and shown in the issue panel.

  #### Examples

- regex to extract ERROR, SEVERE or FATAL words:

  ```
  \b(ERROR|SEVERE|FATAL)\b
  ```

- regex to extract UTC date:

  ```
  \d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2},\d{3}[+-]\d{4}
  ```


### Lenses

  Lenses (smart tooltips) can execute logic for the provided token. The lense below calls external service to get vendor name based on the MAC address:

  <img width="277" height="116" alt="image" src="https://github.com/user-attachments/assets/12266bf0-4740-44f3-83af-33bf75cd1871" />

  Regex:
  ```
  ([0-9a-fA-F]{2}[:-]){5}([0-9a-fA-F]{2})
  ```
  
  Code:
  ```
  let requestOptions = {
      method: "GET"
  };

  // 'fetchSync' and 'match' are provided by Essence app to make a call for the given token
  let responseText = fetchSync(`https://api.macvendors.com/` + match, requestOptions);
  
  return responseText;
  ```

#### More Examples

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
  - Please be aware that you are responsible for what data you send to external services.
    

  ```
  let url = "https://xyz.com/rest/api/";

  let requestOptions = {
      method: "GET",
      headers: {
          "Accept": "application/json",
          "Authorization": "Basic <BASE64 encoded creds>" 
      }
  };

  // 'fetchSync' and 'match' are provided by Essence app to make a call for the given token
  let responseText = fetchSync(url, requestOptions);
  
  // Parse the JSON string
  let responseJson = JSON.parse(responseText);
  
  //parsing logic
  
  return 'something'

  ```


