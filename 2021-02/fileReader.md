```typescript
export const fileReader = async (
    _data: any,
    _encoding: 'UTF-8' | 'base64',
) => {
    let fileText = await new Promise((resolve) => {
        const fileReader = new FileReader();
        fileReader.onload = () => resolve(fileReader.result);
        if (_encoding === 'UTF-8') {
            fileReader.readAsText(_data);
        } else {
            fileReader.readAsDataURL(_data);
        }
    });

    return fileText;
};
```
