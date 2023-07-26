> Antd 自定义表单规则为什么用 promise

在 antd 中，自定义表单规则要求返回一个 Promise，因为 Promise 可以很好地处理异步操作，同时也支持同步操作。当你需要执行一些异步操作（例如，请求服务器验证输入的值）时，Promise 可以让你的验证逻辑变得更加简洁和易于理解



* Promise 可以很好地处理异步操作，同时支持同步操作
* 使用 Promise 可以让验证逻辑更加简洁和易于理解



> 自定义验证表单实例

```tsx
import React from 'react';
import { Form, Input, Button } from 'antd';
import { Rule } from 'antd/lib/form';

const validateEmailOnServer = async (
  rule: Rule,
  value: string
): Promise<void> => {
  // 模拟异步服务器调用(例如，使用 setTimeout )
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      // 在本例中，我们假设服务器响应指示电子邮件是否已经注册
      const isEmailRegistered = checkIfEmailIsRegisteredOnServer(value);

      if (isEmailRegistered) {
        reject("This email is already registered!");
      } else {
        resolve();
      }
    }, 2000); // 为服务器响应模拟 2 秒延迟
  });
};

const checkIfEmailIsRegisteredOnServer = (email: string) => {
  //模拟一个检查邮件是否在服务器上注册的函数,实际中通常会向服务器调用 API 来检查
  //在例子中，假设 'registeredEmails' 是一个注册电子邮件地址数组
  const registeredEmails = ["user@example.com", "anotheruser@example.com"];
  return registeredEmails.includes(email);
};

export const CustomValidationForm = () => {
  const onFinish = (values: { email: string }) => {
    console.log("Form values:", values);
  };

  return (
    <Form onFinish={onFinish}>
      <Form.Item
        name="email"
        label="Email"
        rules={[
          {
            type: "email",
            message: "Please enter a valid email address!",
          },
          {
            required: true,
            message: "Please enter your email address!",
          },
          {
            validator: validateEmailOnServer,
          },
        ]}
      >
        <Input />
      </Form.Item>

      <Form.Item>
        <Button type="primary" htmlType="submit">
          Submit
        </Button>
      </Form.Item>
    </Form>
  );
};

```



在示例中，该 `validateEmailOnServer` 函数返回一个 Promise，该 Promise 模拟异步服务器调用以检查电子邮件地址是否已注册。该`checkIfEmailIsRegisteredOnServer`函数是一个模拟的服务器端函数，用于检查电子邮件是否已注册

使用的属性将自定义验证规则添加到表单 `email` 中的字段。提交表单时将触发验证，Promise 将处理异步验证过程 `validator` `Form.Item`

在真实的应用程序中，可以将模拟服务器调用替换为对后端服务器的实际 API 调用以进行电子邮件验证

