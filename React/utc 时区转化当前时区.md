>UTC 世界标准时间

UTC（协调世界时）是世界规范时钟和时间的主要时间标准。全世界都一样，不随季节变化。UTC 通常用作各种应用程序和系统的时间参考。

要在 JavaScript 中表示 UTC 时间，您可以使用以下格式：



1. 带 “ Z “ 后缀的 ISO 8601 格式：

   ```js
   const utcTime1 = "2023-07-31T07:08:34Z";
   ```

   

2. 具有明确 UTC 偏移量（ +00:00 ）的 ISO 8601 格式：

   ```js
   const utcTime2 = "2023-07-31T07:08:34+00:00";
   ```

   

3. UTC 时间戳（以毫秒为单位）：

   ```js
   // UTC 的时间戳示例 July 31, 2023, 07:08:34 
   const utcTimestamp = 1677761314000;  
   
   const date = new Date(utcTimestamp);
   console.log(date.toISOString()); // 输出: "2023-07-31T07:08:34.000Z"
   
   ```

   

在每种情况下，“ Z ”或 “ +00:00 ” 后缀表示时间以 UTC 表示。在 JavaScript 中解析或使用 UTC 时间时，必须确保正确处理时区，以避免因时区转换而导致任何意外行为



> 检查时间戳查看是否为 UTC 格式

检查时间戳字符串本身，UTC 时间戳通常在末尾具有特定的偏移格式，例如 `+00:00`  或 `Z` （代表祖鲁时间，相当于 UTC+00:00 ）

以下为检查时间戳是否采用 UTC 格式的方法

```js
function isUTCTimestamp(timestamp) {
  // 检查时间戳是否以“ Z ”(祖鲁时间)或“ +00:00 ”( UTC 偏移量)结束
  return timestamp.endsWith("Z") || timestamp.endsWith("+00:00");
}

// 示例使用
const timestamp1 = "2023-07-31T07:08:34.56+00:00";
const timestamp2 = "2023-07-31T07:08:34.56Z";

console.log(isUTCTimestamp(timestamp1)); // 输出: true
console.log(isUTCTimestamp(timestamp2)); // 输出: true

const timestamp3 = "2023-07-31T07:08:34.56-05:00"; // Not UTC
console.log(isUTCTimestamp(timestamp3)); // 输出: false

```

通过检查时间戳是否以“ Z ”或“ +00:00 ”结尾，可以确定它是否代表 UTC 时间



> 结合 React tsx 将 UTC 时间全局转化为本地时间

在主入口文件中导入 Day.js 和必要的插件（例如 `index.tsx`  或 `App.tsx`  ）

```tsx
import React from 'react';
import ReactDOM from 'react-dom';
import dayjs from 'dayjs';
import utc from 'dayjs/plugin/utc';
import timezone from 'dayjs/plugin/timezone';

dayjs.extend(utc);
dayjs.extend(timezone);

// 将 day.js 默认时区设置用户的本地时区
const userTimeZone = Intl.DateTimeFormat().resolvedOptions().timeZone;

dayjs.tz.setDefault(userTimeZone);

```



现在，可以在 React 组件中的任何位置使用 Day.js 将 UTC 时间转换为本地时间

```tsx
import React from 'react';
import dayjs from 'dayjs';

export const ExampleComponent: React.FC = () => {
  const utcTime = '2023-07-18T12:34:56.789Z'; // 将其替换为 UTC 时间
  const localTime = dayjs.utc(utcTime).local().format('YYYY-MM-DD HH:mm:ss.SSS'); // 时:分:秒:毫秒

  return (
    <div>
      <p>UTC Time: {utcTime}</p>
      <p>Local Time: {localTime}</p>
    </div>
  );
};

```



在示例中，该 `dayjs.utc(utcTime).local()` 函数用于将给定的 UTC 时间 ( `utcTime` ) 转换为步骤中设置的用户本地时区。然后使用该 `format` 方法对结果进行格式化，以所需的格式显示它

通过将 Day.js 的默认时区设置为用户的本地时区，可以在考虑用户的时区的情况下，有效地将 UTC 时间转换为 React 应用程序的全局本地时间
