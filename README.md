Mostly copied from this [gist](https://gist.github.com/Ananto30/8af841f250e89c07e122e2a838698246).

Changes made from the original:
1. Added timer goroutine to keepalive the connection. If you put this `ssesever` behind `nginx`, it will close the connection after a period. To keep the connection alive, added a timer to periodically sent heartbeat to `/stream`.
2. Port number as argument.

#### build

    go build -o sseserver main.go
