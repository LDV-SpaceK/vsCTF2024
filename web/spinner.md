# spinner 

![image](https://github.com/LDV-SpaceK/vsCTF2024/assets/152776722/76bb3d62-ab49-45c8-9954-46a5025394e5)



![image](https://github.com/LDV-SpaceK/vsCTF2024/assets/152776722/10cfecd1-a522-45d1-8655-49a2e464c58e)


goal: bài này là phải quay 9999 vòng th sẽ hiện ra thông báo data.message 

=> sau 1 lần check code thì bài này ta cần phải config giá trị của spinner qua websocket  


=> websocket là một công nghệ cho phép thiết lập kết nối liên tục và hai chiều (full-duplex) giữa một máy khách (client) và một máy chủ (server) qua một giao thức truyền tải duy nhất. Nó cho phép các ứng dụng web gửi và nhận dữ liệu một cách hiệu quả và thời gian thực

![image](https://github.com/LDV-SpaceK/vsCTF2024/assets/152776722/f5083f85-c659-42e9-8ecc-64cc7299e2fb)


- sau khi checking trên brupsuite ws history thì ta thấy được 1 chuỗi tọa độ được gửi nhiều lần lên server và được trả về trên client 1 giá trị spin : ?


- có 1 số thông tin về tọa độ đó là client X, Y có vẻ cố định và chỉ thay đổi khi mình tạo 1 request khác trong web nhưng X,Y thì thay đổi nên ta có thể tạo ra payload với ý tưởng như thế


payload_original:

```
const WebSocket = require('ws');

const ws = new WebSocket('wss://spinner.vsc/ws');

ws.on('open', () => {
    console.log('WebSocket connected');

    // Hàm để tính toán và gửi dữ liệu chuột
    function sendMouseData() {
        const centerX = window.innerWidth / 2;
        const centerY = window.innerHeight / 2;

        // Gửi dữ liệu chuột mỗi lần gọi hàm này
        document.addEventListener('mousemove', (event) => {
            const { clientX, clientY } = event;

            const message = {
                x: clientX,
                y: clientY,
                centerX: centerX,
                centerY: centerY
            };

            ws.send(JSON.stringify(message));
        });
    }

    // Lắng nghe tin nhắn từ máy chủ
    ws.on('message', (data) => {
        const message = JSON.parse(data);
        console.log('Received message:', message);

        // Kiểm tra nếu số vòng quay đã đạt 9999, in ra flag và đóng kết nối
        if (message.spins >= 9999) {
            console.log('Flag:', message.message);
            ws.close();
        }
    });

    // Gọi hàm gửi dữ liệu chuột
    sendMouseData();
});

ws.on('close', () => {
    console.log('WebSocket connection closed');
});

ws.on('error', (error) => {
    console.error('WebSocket error:', error.message);
});


payload gốc  để hình dung ra quy luật 
```

=> payload trên ta đang gửi nhiều tọa độ lên server sao cho gửi giá trị X, Y thì server trả về 1 giá tri spin tăng dần lên 9999 

payload: node.js => flag

```
const WebSocket = require('ws');

const ws = new WebSocket('wss://spinner.vsc.tf/ws');

ws.on('open', () => {
    console.log('WebSocket connected');

    let intervalId;

    // Function to send mouse data
    function sendMouseData() {
        const centerX = 1536 / 2;
        const centerY = 412 / 2;

        intervalId = setInterval(() => {
            const x = Math.floor(Math.random() * 1536); // Random x coordinate
            const y = Math.floor(Math.random() * 412); // Random y coordinate

            const message = {
                x: x,
                y: y,
                centerX: centerX,
                centerY: centerY
            };

            ws.send(JSON.stringify(message));
        }, 1); // Send data every 1ms
    }

    // Listen for messages from the server
    ws.on('message', (data) => {
        const message = JSON.parse(data);
        console.log('Received message:', message);

        // Check if spins reached 9999, print flag and close connection
        if (message.spins >= 9999) {
            console.log('Flag:', message.message);
            clearInterval(intervalId); // Stop sending data
            ws.close();
        }
    });

    // Call the function to send mouse data
    sendMouseData();
});

ws.on('close', () => {
    console.log('WebSocket connection closed');
});

ws.on('error', (error) => {
    console.error('WebSocket error:', error.message);
});

```

=> thêm đoạn code có thể chạy trên console 

```
function spin(r){
    socket.send(JSON.stringify({centerX: 0,CenterY: 0 ,x: -r , y: -r })
    socket.send(JSON.stringify({centerX: 0,CenterY: 0 ,x: r , y: -r })
    socket.send(JSON.stringify({centerX: 0,CenterY: 0 ,x: r , y: r })
    socket.send(JSON.stringify({centerX: 0,CenterY: 0 ,x: -r , y: r })
}
let i = 0.001;
setInterval(() => {
    for (let z = 0; z < 1000;z++ ) spin(++i);}, 1);

```

![image](https://github.com/LDV-SpaceK/vsCTF2024/assets/152776722/c7c34ea8-5d87-41a8-9096-1d319f4ab6d1)




=>flag: vsctf{i_ran_out_of_flag_ideas_so_have_this_random_string_2CSJzbfeWqVBnwU5q8}
