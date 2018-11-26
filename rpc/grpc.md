# grpc

## protobuf语法

    syntax = "proto3";
    package addressbook;

    service AddressBook {
        rpc AddPerson(Person) return (AddressBook){}
    }

    message Person {
        string name = 1;
        int32 id = 2;
        string email = 3;

        enum PhoneType {
        MOBILE = 0;
        HOME   = 1;
        WORK   = 2;
        }

        message PhoneNumber {
            string number = 1;
            PhoneType type = 2;
        }

        repeated PhoneNumber phones = 4;
    }

    message AddressBook {
        repeated Person people = 1;
    }

## protobuf grpc 

    service Greeter {
        rpc SayHello(HelloRequest) returns (HelloReply) {}
    }

    Message HelloRequest {
        string name = 1;
    }

    Message HelloReply {
        string message = 1;
    }

```
sdfsdf
dffggg
```

    

