> 关于 Antd modal 对话框封装和使用

样式使用 [**tailwind**](https://tailwindcss.com/) 书写，实际使用和组件修改根据实际需求修改

```tsx
import { Modal } from "antd";
import React, { forwardRef, useImperativeHandle, useState } from "react";

interface ModalBoxProps {
  title: string;
  contents: React.ReactNode;
  width?: number;
}

export interface ModalBoxRef {
  open: () => void;
  close: () => void;
}

export const ModalBox = forwardRef<ModalBoxRef, ModalBoxProps>(
  (props: ModalBoxProps, ref: React.ForwardedRef<ModalBoxRef>) => {
    const [open, setOpen] = useState<boolean>(false);

    useImperativeHandle(ref, () => ({
      open: () => {
        setOpen(true);
      },
      close: () => {
        setOpen(false);
      },
    }));

    return (
      <Modal
        mask
        maskClosable={false}
        title={<div className="text-xl">{props.title}</div>}
        centered
        destroyOnClose={true}
        open={open}
        onOk={() => setOpen(false)}
        onCancel={() => setOpen(false)}
        width={props.width}
        footer={false}
        closable={false}
      >
        {props.contents}
      </Modal>
    );
  }
);

```



> 使用示例

```tsx
const aiCreationRef = useRef<ModalBoxRef>(null);

<ModalBox
  ref={aiCreationRef}
  width={1600}
  title={"新建制作"}
  contents={<AICopyCreation aiCreationRef={aiCreationRef} />}
/>;

// 打开 modal
aiCreationRef.current.open()

// 关闭 modal
aiCreationRef.current?.close()
```

