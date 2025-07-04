
<iframe width="928" height="522" src="https://www.youtube.com/embed/fpW-h9uT6Uo" title="Top privilege escalation techniques - bug bounty case study" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---

# WebSocket: 10 Vulnerabilities You Should Be Aware Of


# What is WSS?

**WSS (WebSocket Secure)** is an encrypted communication protocol that builds upon the WebSocket protocol. It is used to establish secure, persistent, full-duplex connections between clients and servers over HTTPS.

This protocol is widely used in applications that require real-time data exchange, such as:

- **Chat applications**: Delivering instant messages without delays.
- **Live streaming**: Enabling low-latency video or audio streams.
- **Real-time dashboards**: Providing up-to-the-moment updates for stock markets, analytics, or monitoring tools.

The WSS protocol ensures that all communication between the client and server is encrypted, protecting the data from being intercepted by unauthorized parties.

# WSS Vulnerabilities

WSS (WebSocket Secure) is often used in various applications that rely on real-time communication. Examples include:

- **Live streaming platforms**: Delivering video or audio broadcasts.
- **Private meeting rooms**: Facilitating virtual conferences or chats.

When analyzing such programs, my focus shifts to WebSockets rather than traditional HTTP traffic. This is because WebSockets enable direct, continuous communication, making them a potential vector for unique vulnerabilities.

There are countless creative approaches to uncovering WSS vulnerabilities, as this protocol often handles sensitive, real-time data. Whether through manipulating requests, injecting unexpected payloads, or exploring improper authentication and authorization, WSS offers a broad attack surface for security testing.

> #1 How a Single Vulnerability Crashes Live Broadcasts: A Real-World Example

WebSocket Secure (WSS) vulnerabilities often lead to severe disruptions in applications relying on real-time communication. Here’s an example of a vulnerability that caused a complete denial of service for live broadcasts.


# Steps to Reproduce:

1. Log in as an admin and assign a moderator to a classroom.
2. As the moderator, intercept the API request for role assignment using tools like Burp Suite or browser developer tools.
3. Modify the request payload as follows:

```json
{  
    "request_type": "ASSIGN ROLE",  
    "payload": {  
        "role": "crash",  
        "user_id": "55150"  
    },  
    "request_id": "A1Kptpj0FIfef173-biAa"  
}  

```
1. Replace the `role` value with a non-standard string such as `crash`.
2. Send the modified request.
3. Observe the effects:

- ==All live broadcasts in the session will crash.==
- Participants will see the error message:

```
Something went wrong!  
Try to refresh the page  
  
id: ba32ad2f6da04119b3263b6d9cf30aaa  
TypeError: Cannot read properties of undefined (reading 'push')
```


> #2 Moderator Privilege Exploit: Kicking the Host from a WebSocket Session

# Summary

This flaw allows users with **Moderator** privileges to execute an unauthorized action: removing the **Host** from an active session. The exploit leverages crafted WebSocket requests using the `connection_id` of the Host.

# Details

- **Vulnerability Type**: Privilege Escalation
- **Affected Component**: WebSocket API for Session Management
- **Exploit Methodology**:

1. Moderators intercept WebSocket traffic using tools like browser developer tools or proxies.
2. The `**connection_id**` of the Host is extracted from WebSocket messages.
3. A malicious WebSocket request with the following payload is crafted:

```json
{  
  "request_type": "KICK",  
  "payload": {  
    "connection_id": "usr-conn-1ef9c55xu"  
  },  
  "request_id": "dME5ScO1R4kLK_STnmfUQ"  
}
```

4. The crafted request is sent to the server, resulting in the Host being forcibly removed from the session.



> #3 Privilege Escalation Allows Moderators to Demote and Remove Host in Live Streaming Sessions

# Description of the Vulnerability

A privilege escalation vulnerability was identified in the platform, enabling users with **Moderator** privileges to:

1. **Demote the Host** to a lower role, such as “Participant.”
2. **Remove the Host** from the live session entirely.

This vulnerability bypasses the intended role hierarchy, where Moderators should not have the ability to modify or remove a Host’s role.


# Steps to Reproduce

1. Log in as a user with **Moderator** privileges and join a live broadcast.
2. Intercept the WebSocket (WSS) traffic using tools such as **Burp Suite** or browser developer tools.
3. Locate and modify a WebSocket request similar to the following:

```json
{  
    "request_type": "ASSIGN ROLE",  
    "payload": {  
        "role": "Participant",  
        "user_id": "54992"  
    },  
    "request_id": "PasQjmdXxPzvb63AGqyU-"  
}
```

1. Replace the `user_id` with the ID of the **Host**.
2. Set the `role` value to a rank lower than "Moderator" (e.g., "Participant").
3. Send the modified WebSocket request.
4. Verify that the Host’s role has been successfully downgraded.
5. Use the **Remove Participant** feature to remove the Host from the session entirely.

> **#4 System Allowing Unauthorized Document Sharing**

# Description of the Vulnerability

A vulnerability has been identified in a live broadcast system that allows participants with low-level permissions (e.g., _Participants_) to bypass restrictions and start document sharing. This exploit can disrupt the broadcast system for all users, including administrators, making it unmanageable.

# Detailed Description

The issue arises when an unauthorized user, such as a participant, sends a crafted request to the system’s API. This request mimics the action of starting document sharing, which is typically restricted to users with higher permissions. By sending the following payload via a network tool or script, the participant can trigger document sharing for all attendees:

```json
{  
  "request_type": "START DOC SHARING",  
  "payload": {  
    "doc_url": "https://test.com/files/fl-uwbp36k4q",  
    "doc_id": "fl-uwbp36k4q",  
    "presenter_page": 1  
  },  
  "request_id": "ziwi9wH0-RvzS5Il6sEPn"  
}

```
# Steps to Reproduce

1. Join a live broadcast session with a low-level permission account (e.g., as a _Participant_).
2. Send the crafted API request (shown above) to the server.
3. Observe that document sharing is initiated for all users, regardless of permissions.
4. Notice that the broadcast system becomes unmanageable for administrators.

> #5 Vulnerability in Live Streaming Platform’s Storyboard Functionality

# Description of the Vulnerability

A security vulnerability was identified in a live streaming platform that allows unauthorized users with limited permissions (e.g., _Participants_) to interact with the **Storyboard** feature. **This functionality is intended to be accessible only to users with administrative privileges (Admins).** The issue arises due to insufficient access control in the platform’s API endpoints, which allows unauthorized actions such as viewing, adding, and deleting Storyboard entries.

# Detailed Description

1. **Retrieving Storyboard Information**

- **Endpoint**: `GET /api/test/activities/?offset=0&limit=999&room_id=room-<id>`
- **Description**: This endpoint allows any user to retrieve a list of all Storyboard entries in a given room by providing the `room_id`. The response includes detailed metadata for each entry, including unique IDs.

**2. Adding a Storyboard Entry**

- **Endpoint**: `POST /api/test/activities/`
- **Payload Example**:

```json
{  
  "link": "https://www.youtube.com/watch?v=eNvUS-6PTbs",  
  "tag": "youtube",  
  "room": "room-<id>",  
  "meta": {}  
}
```

- **Description**: This endpoint allows users to add new Storyboard entries to any room by specifying the `room_id` in the payload, bypassing permission checks.

**3. Deleting a Storyboard Entry**

- **Endpoint**: `DELETE /api/test/activities/<activity_id>/`
- **Description**: After obtaining the `activity_id` from the `GET` request, a user can delete any Storyboard entry without administrative privileges.

# Steps to Reproduce

1. **Retrieve all Storyboard entries**:

- Send a `GET` request to `/api/test/activities/` with the `room_id`.

**2. Add a new Storyboard entry**:

- Send a `POST` request to `/api/test/activities/` with the required payload.

**3. Delete a Storyboard entry**:

- Use the `activity_id` retrieved from step 1 and send a `DELETE` request to `/api/test/activities/<activity_id>/`.

> #6 Disrupt Live Broadcast Sessions

# Description of the Vulnerability

A vulnerability has been identified in the live broadcast system, which **allows malicious users to disrupt live sessions even after being removed by administrators. This issue arises due to improper request validation and insufficient control mechanisms in handling WebSocket (WSS)** communications. Malicious users can capture WebSocket requests during their session and reuse them to continue interacting with the live broadcast system, even after being removed.

# Detailed Description

The vulnerability lies in the ability to capture WebSocket requests and reuse them to interact with the live broadcast system. A malicious user can **send requests after being removed from the session, causing disruptions such as spamming notifications or other interactions.** The lack of proper validation allows users to bypass removal and continue sending unauthorized actions, potentially disturbing the session for others.


# Steps to Reproduce

- **Join a live broadcast** and monitor WebSocket requests. For example:

```json
{  
  "request_type": "JOIN",  
  "payload": {  
    "nickname": "<h1>hi</h1>",  
    "is_lti": false,  
    "device_connections": [  
      {"jid": "tdXe-6SUqI(u.vm)undefined", "cam": false, "mic": false}  
    ]  
  },  
  "request_id": "-M3naVEoIqcm2OAHvzA4Y"  
}

```

- **Perform an action** such as raising a hand and capture the corresponding request. For example:


```json
{  
  "request_type": "PATCH SELF",  
  "payload": {"hand": 1733531882315},  
  "request_id": "29F7QIgAysobak3Pi-JRh"  
}  
```

1. **Get removed from the session** by the administrator.
2. **Reuse the captured WebSocket requests** to send actions (e.g., hand raises) or other payloads. These actions will be reflected in the live broadcast, bypassing the removal restriction.


This vulnerability allows users to bypass session removal and continue interacting with the system, disrupting the broadcast and negatively affecting other participants.

> #7 Breakout Room Information Leak via WebSocket Request

# Description of the Vulnerability

Participants in a live stream can view sensitive information about breakout rooms created by the admin through WebSocket request data. This allows participants to see details of breakout rooms, including IDs and names, even if they don’t have permission to access or join them. This lack of access control exposes information that should only be visible to admins, creating potential privacy concerns during live broadcasts.

# Detailed Description

The vulnerability arises from the fact that WebSocket responses sent to all participants in a session include sensitive information regarding breakout rooms. This includes the names, IDs, and potentially other details of breakout rooms that should only be visible to the admin or those invited to those rooms. Since WebSocket responses are not properly filtered, unauthorized participants can capture this information by monitoring the WebSocket traffic, exposing room details they shouldn’t have access to.

# Steps to Reproduce

1. **Admin**: Start a live broadcast and create a breakout room. Invite participants to the session.
2. **Participant**: Open browser developer tools and monitor the WebSocket request history.
3. **Admin**: Create multiple breakout rooms during the live broadcast.
4. **Participant**: Observe the WebSocket request history. The breakout room information (IDs and names) will appear in the WebSocket responses even though the participant does not have permission to see or join the rooms.  
    Example WebSocket response:

```json
"breakout_rooms": [  
  {"id": "room-t6h944pcu", "name": "Room 1", "participant_list": []},  
  {"id": "room-4h6khxdos", "name": "Room 2", "participant_list": []},  
  {"id": "room-nrw6s7xrx", "name": "Room 3", "participant_list": []}  
]
```


his vulnerability exposes sensitive information about the breakout rooms to unauthorized participants, undermining the intended privacy and security of the live broadcast session

> #8 Unauthorized Timer Control in Live

**Description of the Vulnerability**  
Participants with the lowest permissions in a live-streaming session can exploit a vulnerability to initiate a countdown timer, bypassing access control mechanisms. This unauthorized action disrupts the experience for all users, including those with higher administrative privileges, as the timer cannot be stopped by them.

**Detailed Description**  
The vulnerability occurs when users with **Participant** or equivalent low-level permissions initiate a countdown timer using a network request. This action bypasses access controls and grants unauthorized control over the session, affecting all participants. Notably, administrators and users with higher privileges are unable to stop or modify the timer, leading to a disruptive experience for everyone in the session.

**Steps to Reproduce**

1. Join a live streaming session with a **Participant** role or equivalent low-level permissions.
2. Send the following request via a network tool (e.g., Postman or a browser’s developer tools):

```json
{  
  "request_type": "START TIMER",  
  "payload": {"seconds": 2525},  
  "request_id": "ARS03V3_QppyiuwZ9daJ"  
}
```

3. Replace `"request_id"` with a valid ID recognized by the system.

4. Observe that the timer starts and is visible to all participants, including higher-level roles.

5. Note that even administrators are unable to stop or modify the timer.

> #9 Unauthorized WebSocket Information Disclosure in Privte Live

# Description of the Vulnerability

A vulnerability exists in the live broadcast feature where unauthorized users can obtain sensitive broadcast information via WebSocket communication, even if they have not been approved to join the broadcast. **This flaw allows unauthorized users to view session details such as usernames, session IDs, and the number of participants.** This issue arises from insufficient access control in WebSocket communications, enabling unauthorized users to monitor session-related updates.

# Detailed Description

The vulnerability is triggered when an unauthorized user connects to a live broadcast link and monitors WebSocket traffic. Although they are not approved to join the broadcast, **they can still receive updates on session details, such as participant nicknames, session IDs, and the number of users currently in the session.** This is due to the lack of adequate access control in the WebSocket communication channel, which exposes sensitive session data to anyone connected to the WebSocket, even if they don’t have access privileges.

# Steps to Reproduce

1. **Setup by Admin**:

- As an admin, create a live broadcast that requires member approval before joining.
- Enter the broadcast as the admin, ensuring the browser proxy is disabled.

**2. Observe from Another Account**:

- From a different account, attempt to access the broadcast link.
- Ensure you are connected to a proxy to monitor WebSocket traffic.
- Refresh the page to establish a WebSocket connection.

**3. Monitor WebSocket Traffic**:

- You will notice a response similar to the following:

```json
{"status": "WebSocket is connected", "code": 200}
```

**4. Trigger an Update from Admin Account**:

- Perform actions such as joining or leaving the broadcast from the admin account or another approved account.
- The unauthorized account monitoring WebSocket traffic will receive data similar to this:

```json
{"type": "online_sessions_info_update", "session_id": "ssn-ovi77f0wl", "data": {"online_count": 2, "nicknames": ["im4x", "Test1 test"], "room": "room-0encmqw38", "notification_type": "UPDATE SESSION"}}
```

This vulnerability exposes sensitive session data to unauthorized users, which could lead to privacy concerns and unauthorized access to session information. Proper access control and validation are required to prevent such information leakage.

> #10 Moderator Microphone Mute Host via WebSocket Requests

# Description of the Vulnerability

A security vulnerability has been discovered in the WebSocket-based API, allowing a user with “Moderator” privileges to mute the microphone of the Host without authorization. This exploit can be executed by crafting and sending a specific WebSocket request containing the `connection_id` of the Host and specifying the target device type.

# Steps to Reproduce

1. **Intercept WebSocket Messages**:  
    As a Moderator, intercept WebSocket messages during an active session using browser developer tools or a proxy tool (e.g., Burp Suite).

**2. Identify Host’s** `**connection_id**`:  
Identify the `connection_id` associated with the Host from the intercepted WebSocket messages.

**3. Craft the Malicious WebSocket Request**:  
Construct a WebSocket request to mute the Host’s microphone, as follows:

```json
{  
  "request_type": "MUTE",  
  "payload": {  
    "connection_id": "usr-conn-8o2spgmab",  
    "device_type": "MIC"  
  },  
  "request_id": "9UbgHwHILYAtDeVjD_Alr"  
}
```

**4. Send the Crafted Request**:  
Send the crafted WebSocket request to the WebSocket server.

**5. Observe the Effect**:  
The Host’s microphone will be muted without their consent or notification.

This vulnerability allows a Moderator to perform unauthorized actions such as muting the Host’s microphone, bypassing their intended role limitations and disrupting the live session.


