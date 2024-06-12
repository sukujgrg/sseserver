Mostly copied from [here (gist)](https://gist.github.com/Ananto30/8af841f250e89c07e122e2a838698246).

#### Changes made to the original:
1. Added a timer to keepalive the connection. If you put `ssesever` behind `nginx` without sending heartbeat, it will close the connection after a period. To keep the connection alive, added a timer to periodically sent heartbeat.
2. Port number as argument.
3. Removed `w.Header().Set("Access-Control-Allow-Origin", "*")`. Idea is to put it behind a proxy like nginx.

#### build

    go build -o sseserver main.go

#### run

    # on first terminal
    % go run main.go 8080

    # on second terminal
    % curl -v localhost:8080/messages -d '{"title": "1", "text": "2"}'

    # on third terminal
    % curl -Nv localhost:8080/stream
    *   Trying 127.0.0.1:8080...
    * Connected to localhost (127.0.0.1) port 8080
    > GET /stream HTTP/1.1
    > Host: localhost
    > User-Agent: curl/8.4.0
    > Accept: */*
    >
    < HTTP/1.1 200 OK
    < Cache-Control: no-cache
    < Connection: keep-alive
    < Content-Type: text/event-stream
    < X-Accel-Buffering: no
    < Date: Wed, 12 Jun 2024 21:07:22 GMT
    < Transfer-Encoding: chunked
    <
    data: {"title":"1","text":"2"}

    data: {"text":"2024-06-13 06:56:34.791516 +1000 AEST m=+23640.003301834","title":"keepalive"}

    data: {"text":"2024-06-13 06:57:34.790542 +1000 AEST m=+23700.003241793","title":"keepalive"}
