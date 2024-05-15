## SteamSocket Packet Handling

### Introduction
The `SteamSocket` class manages network socket connections in a Steam environment. It handles various types of packets received from connected players and performs appropriate actions based on the packet type.

### Methods

| Method Signature                                                 | Description                                                                                                   |
|------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| `OnConnecting(Connection connection, ConnectionInfo data)`       | Called when a connection is being established. Logs a message indicating the connection is being established. |
| `OnConnected(Connection connection, ConnectionInfo data)`        | Called when a connection is successfully established. Logs a message indicating a new player has connected.    |
| `OnDisconnected(Connection connection, ConnectionInfo data)`    | Called when a connection is disconnected. Logs a message indicating a player has disconnected.               |
| `OnMessage(Connection connection, NetIdentity identity, IntPtr data, int size, long messageNum, long recvTime, int channel)` | Called when a message is received over the connection and processes it accordingly.                               |

### Packet Handling

The following table summarizes the packet sizes and their corresponding handling logic:

| Packet Size | Handling Logic                                                                                                                                                                                         |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 105         | If the packet size is 105, it checks if the current lobby is owned by the sender's Steam ID. If so, it processes the packet to start or force start the game based on the game state.                  |
| 1           | If the packet size is 1, it reads the UI communication packet type and performs actions accordingly, such as acknowledging, suggesting starting the game, or rejecting ready-up requests. |
| 3           | If the packet size is 3, it updates lobby-related information such as color, team, and readiness status for the sender.                                                                           |
| 4           | If the packet size is 4, it updates lobby-related information such as selected abilities and readiness status for the sender.                                                                     |
| 15          | If the packet size is 15, it reads the lobby ready packet and performs actions such as checking for available colors and broadcasting readiness information.                                       |
| 2           | If the packet size is 2, it reads the lobby initialization packet and performs initialization actions.                                                                                               |
| 6           | If the packet size is 6, it reads and responds to lobby ping packets.                                                                                                                                |

### Member Variables

- `messageBuffer`: Buffer for storing received message data.
- `ulongConversionArray`, `uintConversionArray`, `ushortConversionArray`: Arrays used for converting data types.
- `lobbyPingAckBuffer`, `lobbyInitBuffer`, `startRequestBuffer`, `uiCommunicationBuffer`: Buffers for specific types of messages.
