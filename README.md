Делаю опыты с gRPC на Ubuntu  
делал по инструкции https://grpc.io/docs/languages/go/quickstart/  
sudo apt install -y protobuf-compiler  
protoc --version  
устанавливаю плагины  
$ go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.28  
$ go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.2  
обновляю PATH  
export PATH="$PATH:$(go env GOPATH)/bin"  
создаю модуль  
go mod init github.com/developerc/grpc3  
установим пакет grpc  
go get google.golang.org/grpc  
создал структуру папок проекта:  
.  
├── cmd  
│ ├── client // код запуска клиента  
│ └── server // код запуска сервера  
├── proto  
└── go.mod  
В каталоге proto создал файл geometry.proto  
генерирую файлы с помощью плагина  
protoc --go_out=. --go_opt=paths=source_relative --go-grpc_out=. --go-grpc_opt=paths=source_relative proto/geometry.proto  
в папке proto появились файлы geometry_grpc.pb.go и geometry.pb.go  
некоторые ссылки подчеркнуты красным, надо подтянуть зависимости:  
go mod tidy  
заполнили кодом ./cmd/server/main.go  
проверили как запускается сервер, потом остановим Ctrl+C  
go run ./cmd/server/main.go  
заполним кодом ./cmd/client/main.go  
запустим сервер в терминали VS  
go run ./cmd/server/main.go  
запустим клиент в Windows консоли  
go run ./cmd/client/main.go  
должны получить распечатку для клиента:  
Area: 207.05  
Perimeter: 61.2  
распечатка для сервера:  
1990/03/01 08:22:55 tcp listener started at port: 5000  
1990/03/01 08:23:17 invoked Area: height:10.1 width:20.5  
1990/03/01 08:23:17 invoked Perimeter: height:10.1 width:20.5  
