# <center> Welcome to use-file-picker 👋 </center>

## _Simple react hook to open browser file selector._

![alt Version](https://img.shields.io/npm/v/use-file-picker?color=blue) ![alt License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg) ![alt Twitter: twitter.com/JankiewiczMi/](https://img.shields.io/twitter/follow/JankiewiczMi.svg?style=social)

<strong>🏠 [Homepage](https://github.com/Jaaneek/useFilePicker 'user-file-picker Github')</strong>

## Documentation

#### Usage

- [Simple txt reader](#simple-.txt-file-content-reading)
- [Image reader](#reading-and-rendering-images)
- [Advanced usage](#advanced-usage)

#### API

- [Props](#props)
- [Returns](#returns)
- [Interfaces](#interfaces)

## Install

`npm i use-file-picker`

## Usage

#### Simple .txt file content reading

https://codesandbox.io/s/inspiring-swartz-pjxze?file=/src/App.js

```jsx
import { useFilePicker } from 'use-file-picker';
import React from 'react';

export default function App() {
  const [openFileSelector, { filesContent, loading }] = useFilePicker({
    accept: '.txt',
  });

  if (loading) {
    return <div>Loading...</div>;
  }

  return (
    <div>
      <button onClick={() => openFileSelector()}>Select files </button>
      <br />
      {filesContent.map((file, index) => (
        <div>
          <h2>{file.name}</h2>
          <div key={index}>{file.content}</div>
          <br />
        </div>
      ))}
    </div>
  );
}
```

#### Reading and rendering Images

https://codesandbox.io/s/busy-nightingale-oox7z?file=/src/App.js

```ts
import { useFilePicker } from 'use-file-picker';
import React from 'react';

export default function App() {
  const [openFileSelector, { filesContent, loading, errors }] = useFilePicker({
    readAs: 'DataURL',
    accept: 'image/*',
    multiple: true,
    limitFilesConfig: { max: 1 },
    // minFileSize: 0.1, // in megabytes
    maxFileSize: 50,
  });

  if (loading) {
    return <div>Loading...</div>;
  }

  if (errors.length) {
    return <div>Error...</div>;
  }

  return (
    <div>
      <button onClick={() => openFileSelector()}>Select files </button>
      <br />
      {filesContent.map((file, index) => (
        <div key={index}>
          <h2>{file.name}</h2>
          <img alt={file.name} src={file.content}></img>
          <br />
        </div>
      ))}
    </div>
  );
}
```

### Advanced usage

https://codesandbox.io/s/pedantic-joliot-8nkn7?file=/src/App.js

```ts
import { useFilePicker } from 'use-file-picker';
import React from 'react';

export default function App() {
  const [openFileSelector, { filesContent, loading, errors, plainFiles }] = useFilePicker({
    multiple: true,
    readAs: 'DataURL', // availible formats: "Text" | "BinaryString" | "ArrayBuffer" | "DataURL"
    // accept: '.ics,.pdf',
    accept: ['.json', '.pdf'],
    limitFilesConfig: { min: 2, max: 3 },
    // minFileSize: 1, // in megabytes
    // maxFileSize: 1,
    // readFilesContent: false, // ignores file content
  });

  if (errors.length) {
    return (
      <div>
        <button onClick={() => openFileSelector()}>Something went wrong, retry! </button>
        {errors[0].fileSizeTooSmall && 'File size is too small!'}
        {errors[0].fileSizeToolarge && 'File size is too large!'}
        {errors[0].readerError && 'Problem occured while reading file!'}
        {errors[0].maxLimitExceeded && 'Too many files'}
        {errors[0].minLimitNotReached && 'Not enought files'}
      </div>
    );
  }

  if (loading) {
    return <div>Loading...</div>;
  }

  return (
    <div>
      <button onClick={() => openFileSelector()}>Select file </button>
      <br />
      Number of selected files:
      {plainFiles.length}
      <br />
      {/* If readAs is set to DataURL, You can display an image */}
      {!!filesContent.length && <img src={filesContent[0].content} />}
      <br />
      {plainFiles.map(file => (
        <div key={file.name}>{file.name}</div>
      ))}
    </div>
  );
}
```

## API

### Props

| Prop name        | Description                                          | Default value | Example values                                   |
| ---------------- | ---------------------------------------------------- | ------------- | ------------------------------------------------ |
| multiple         | Allow user to pick multiple files at once            | true          | true, false                                      |
| accept           | Set type of files that user can choose from the list | "\*"          | [".png", ".txt"], "image/\*", ".txt"             |
| readAs           | Set a return type of [filesContent](#returns)        | "Text"        | "DataURL", "Text", "BinaryString", "ArrayBuffer" |
| limitFilesConfig | Set maximum and minimum files that user can select   | n/a           | {min: 1, max: 2}, {max: 1}                       |
| readFilesContent | Title                                                | true          | true, false                                      |
| minFileSize      | Set minimum limit of file size in megabytes          | n/a           | 0.01 - 50                                        |
| maxFileSize      | Set maximum limit of file size in megabytes          | n/a           | 0.01 - 50                                        |

### Returns

| Name             | Description                                                                              |
| ---------------- | ---------------------------------------------------------------------------------------- |
| openFileSelector | Opens file selector                                                                      |
| filesContent     | Get files array of type [FileContent](#filecontent)                                      |
| plainFiles       | Get array of the [`File`](https://developer.mozilla.org/en-US/docs/Web/API/File) objects |
| loading          | True if the reading files is in progress, otherwise False                                |
| errors           | Get errors array of type [FileError](#fileerror) if any appears                          |

### Interfaces

#### LimitFilesConfig

```ts
LimitFilesConfig {
	min?: number;
	max?: number;
}
```

#### UseFilePickerConfig

```ts
UseFilePickerConfig extends Options {
	multiple?: boolean;
	accept?: string | string[];
	readAs?: ReadType;
	limitFilesConfig?: LimitFilesConfig;
	readFilesContent?: boolean;
}
```

#### FileContent

```ts
FileContent {
	lastModified: number;
	name: string;
	content: string;
}
```

#### ImageDims

```ts
ImageDims {
	minImageWidth?: number;
	maxImageWidth?: number;
	minImageHeight?: number;
	maxImageHeight?: number;
}
```

#### Options

```ts
Options extends ImageDims {
	minFileSize?: number;
	maxFileSize?: number;
}
```

#### FileError

```ts
FileError extends FileSizeError, FileReaderError, FileLimitError {
	name?: string;
}
```

#### FileReaderError

```ts
FileReaderError {
	readerError?: DOMException | null;
}
```

#### FileLimitError

```ts
FileLimitError {
	minLimitNotReached?: boolean;
	maxLimitExceeded?: boolean;
}
```

#### FileSizeError

```ts
FileSizeError {
	fileSizeToolarge?: boolean;
	fileSizeTooSmall?: boolean;
}
```

## Author

👤 **Milosz Jankiewicz**

- Twitter: [@twitter.com/JankiewiczMi/](https://twitter.com/JankiewiczMi/)
- Github: [@Jaaneek](https://github.com/Jaaneek)
- LinkedIn: [@https://www.linkedin.com/in/jaaneek](https://www.linkedin.com/in/mi%C5%82osz-jankiewicz-554562168/)

👤 **Kamil Planer**

- Github: [@MrKampla](https://github.com/MrKampla)
- LinkedIn: [@https://www.linkedin.com/in/kamil-planer/](https://www.linkedin.com/in/kamil-planer/)

## [](https://github.com/Jaaneek/useFilePicker#-contributing)🤝 Contributing

Contributions, issues and feature requests are welcome!  
Feel free to check [issues page](https://github.com/Jaaneek/useFilePicker/issues).

## [](https://github.com/Jaaneek/useFilePicker#show-your-support)Show your support

Give a ⭐️ if this project helped you!

![cooldoge Discord & Slack Emoji](https://emoji.gg/assets/emoji/cooldoge.gif)
