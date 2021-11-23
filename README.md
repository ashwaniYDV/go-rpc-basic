# go-rpc-basic

### What will we learn?
* RPC (Remote Procedure Calls)
* How to build an RPC server and client in go
* How to create a CRUD app with Go
* How to use the net/rpc and net/http libraries in Go

### Description
In this Go tutorial, we work at building a basic RPC Server and Client. This includes building the basic app without using the RPC library and then building both the Client and Server applications to showcase some of the basic ideas of RPC with Go. The RPC format is talked about at length and shown off using a CRUD application. This tutorial is in service of being able to talk about Microservices, gRPC and Protobuf.

### Building a Basic CRUD Application in Go
Before adding the RPC logic to the application, we need to scaffold out the basic features of the service. In this case, that means creating a CRUD application. CRUD stands for create, read, update, and delete as those are the basic operations that we want to perform on the data. For the data in this application, we create a simple slice data structure which is used in lieu of a Database. This slice of the custom item type is just positioned as a global variable in the main CRUD and RPC server application.

### Creating an RPC Server and Client
The basic idea of an RPC interface is to be able to call functions remotely as though they were local to the client application. In other words, unlike a standard REST API, an RPC doesn't require us to serialize and deserialize data to and from an intermediate datatype like JSON or XML. We don't need to worry about API endpoints and Domain Specific Languages with an RPC.

```go
client.Call("API.DeleteItem", c, &reply)
client.Call("API.GetDB", "", &db)
  ```

In Go, the net/rpc library requires that the functions in the RPC interface follow a specific format. Each function must be a Method on an Exported Datatype. In the case of this application, that Datatype is just a simple int type Aliased to the name API. Each of these methods must also be exported globally. The Methods must contain exactly two exported type arguments and the second argument must be a pointer. And the Methods must return an error type.

```go
func (a *API) GetByName(title string, reply *Item) error {
	var getItem Item

	for _, val := range database {
		if val.Title == title {
			getItem = val
			break
		}
	}

	*reply = getItem

	return nil
}
```

Above is a code snippet containing the GetByName RPC method. The method is attached to the API type and it has an uppercase first letter which exports it globally. It also contains two arguments, a title string and a reply pointer Item type. The method also just returns an error type. This method can be called from an RPC client because it fits all of the RPC criteria. The RPC client passes in a string for the title that it wants to read from the database and a pointer to an Item type. This pointer is then used to populate the return type in the RPC client.



### Courtesy:
https://steemit.com/utopian-io/@tensor/building-a-basic-rpc-server-and-client-with-go
