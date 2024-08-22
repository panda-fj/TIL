# C#-Python間のWebsocket通信

C#側をサーバ、Python側をクライアントとする

## C#（サーバ）

```C#
using System;
using System.Net;
using System.Net.WebSockets;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

class WebSocketServer
{
    private static async Task AcceptWebSocketClients(HttpListener listener)
    {
        while (true)
        {
            // 接続待機
            HttpListenerContext context = await listener.GetContextAsync();
            if (context.Request.IsWebSocketRequest)
            {
                // WebSocket接続を受け入れる
                HttpListenerWebSocketContext wsContext = await context.AcceptWebSocketAsync(null);
                Console.WriteLine("WebSocket接続が確立されました。");

                // WebSocketの処理を開始
                await HandleWebSocket(wsContext.WebSocket);
            }
            else
            {
                // WebSocketリクエストでない場合は拒否
                context.Response.StatusCode = 400;
                context.Response.Close();
            }
        }
    }

    private static async Task HandleWebSocket(WebSocket webSocket)
    {
        byte[] buffer = new byte[1024];
        while (webSocket.State == WebSocketState.Open)
        {
            // クライアントからのメッセージを受信
            WebSocketReceiveResult result = await webSocket.ReceiveAsync(new ArraySegment<byte>(buffer), CancellationToken.None);
            string receivedMessage = Encoding.UTF8.GetString(buffer, 0, result.Count);
            Console.WriteLine("受信メッセージ: " + receivedMessage);

            // クライアントへエコーメッセージを送信
            string message = "Echo: " + receivedMessage;
            byte[] sendMessage = Encoding.UTF8.GetBytes(message);
            await webSocket.SendAsync(new ArraySegment<byte>(sendMessage), WebSocketMessageType.Text, true, CancellationToken.None);
        }

        // WebSocketのクローズ処理
        await webSocket.CloseAsync(WebSocketCloseStatus.NormalClosure, "接続を終了しました", CancellationToken.None);
        webSocket.Dispose();
        Console.WriteLine("WebSocket接続が終了しました。");
    }

    public static async Task Main(string[] args)
    {
        // HttpListenerを初期化し、リッスンするURIを設定
        HttpListener listener = new HttpListener();
        listener.Prefixes.Add("http://localhost:8080/ws/");
        listener.Start();
        Console.WriteLine("WebSocketサーバーが起動しました。");

        // クライアントの接続を待機
        await AcceptWebSocketClients(listener);
    }
}
```

## Python（クライアント）

```Python
import asyncio
import websockets

async def connect_to_server():
    uri = "ws://localhost:8080/ws/"
    async with websockets.connect(uri) as websocket:
        # サーバーにメッセージを送信
        message = "Hello, C# WebSocket Server!"
        await websocket.send(message)
        print(f"送信メッセージ: {message}")

        # サーバーからのメッセージを受信
        response = await websocket.recv()
        print(f"受信メッセージ: {response}")

# イベントループを開始して接続
asyncio.get_event_loop().run_until_complete(connect_to_server())
```