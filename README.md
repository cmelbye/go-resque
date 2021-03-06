# go-resque

Simple [Resque](https://github.com/defunkt/resque) queue client for [Go](http://golang.org).

## Installation

```
go get github.com/cmelbye/go-resque
```

## Usage

For example, a simple Resque job being processed by a Ruby worker somewhere:

```ruby
module Demo
  class Job
    def self.perform(param=nil)
      puts "Processed a job! Param: #{param.inspect}"
    end
  end
end
```

Enqueue this job from Go:

```go
package main

import (
  "github.com/cmelbye/go-resque"
  "github.com/garyburd/redigo/redis"
)

func main() {
  conn, err := redis.Dial("tcp", "127.0.0.1:6379") // Create new Redis client to use for enqueuing

  if err != nil {
    // Handle error
  }

  defer conn.Close()

  // Enqueue the job into the "email" queue with appropriate client
  resque.Enqueue(conn, "email", "Demo::Job")

  // Enqueue into the "default" queue with passing one parameter to the Demo::Job.perform
  resque.Enqueue(conn, "default", "Demo::Job", 1)

  // Enqueue into the "default" queue with passing multiple
  // parameters to Demo::Job.perform so it will fail
  resque.Enqueue(conn, "default", "Demo::Job", 1, 2, "woot")
}
```

