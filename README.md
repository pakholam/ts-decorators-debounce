# ts-decorators-debounce
函数防抖
## How to use?
```typescript
import { debounce } from "./Debounce";
@debounce()
async onClickMe(arg0: any): Promise<void> {
  await Promise.all(arg0);
  ...
}
```
## Code
```typescript
// 函数防抖
export type CallbackFunction = (...args: any[]) => void;

export type MethodDecorators = (
  target: object,
  propertyName: string,
  propertyDescriptor: PropertyDescriptor
) => PropertyDescriptor;

/**
 * 异步函数防抖
 * @exports
 * @return {MethodDecorators}
 */
export function debounce(): MethodDecorators {
  return (target: object, propertyName: string, propertyDescriptor: PropertyDescriptor): PropertyDescriptor => {
    const method: (...args: any[]) => Promise<any> = propertyDescriptor.value;
    let running: boolean = false;
    propertyDescriptor.value = async function (...args: any[]): Promise<any> {
      if (!running) {
        running = true;
        try {
          await method.apply(this, args);
        } finally {
          running = false;
        }
      }
    };
    return propertyDescriptor;
  };
}
```
