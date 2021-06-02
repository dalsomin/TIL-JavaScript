Blob
Blob 객체는 파일류의 불변하는 미가공 데이터를 나타냅니다. 텍스트와 이진 데이터의 형태로 읽을 수 있으며, ReadableStream으로 변환한 후 그 메서드를 사용해 데이터를 처리할 수도 있습니다.

Blob은 JavaScript 네이티브 형태가 아닌 데이터도 표현할 수 있습니다. File 인터페이스는 사용자 시스템의 파일을 지원하기 위해 Blob 인터페이스를 확장한 것이므로, 모든 블롭 기능을 상속합니다.

Blob 사용하기
블롭이 아닌 객체와 데이터에서 Blob을 생성하려면 Blob() 생성자를 사용하세요. 다른 블롭의 일부에서 새로운 블롭을 생성할 땐 slice() (en-US) 메서드를 사용할 수 있습니다. 사용자의 파일 시스템 내 파일을 Blob으로 얻는 방법은 File 문서를 참고하세요.

Blob 객체를 허용하는 API의 목록은 File 문서에도 있습니다.


function typedArrayToURL(typedArray, mimeType) {
  return URL.createObjectURL(new Blob([typedArray.buffer], {type: mimeType}))
}

const bytes = new Uint8Array(59);

for(let i = 0; i < 59; i++) {
  bytes[i] = 32 + i;
}

const url = typedArrayToURL(bytes, 'text/plain');

const link = document.createElement('a');
link.href = url;
link.innerText = 'Open the array URL';

document.body.appendChild(link);
