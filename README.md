# go-resque

Simple [Resque](https://github.com/defunkt/resque) queue client for [Go](http://golang.org).

## Installation

Installation is simple and familiar for Go programmers:

```
go get github.com/cmelbye/go-resque
```

## Usage

Let's assume that you have such Resque Job (taken from Resque examples):

```ruby
module Demo
  class Job
    def self.perform(params)
      puts "Processed a job!"
    end
  end
end
```

So, we can enqueue this job from Go.

```go
package main

import (
  "github.com/kavu/go-resque" // Import these two packages
  "github.com/vmihailenco/redis" //
)

func main() {
  client := redis.NewTCPClient("127.0.0.1:6379", "", 0) // Create new Redis client to use for enqueuing
  defer client.Close()

  // Enqueue the job into the "go" queue with appropriate client
  resque.Enqueue(client, "go", "Demo::Job")

  // Enqueue into the "default" queue with passing one parameter to the Demo::Job.perform
  resque.Enqueue(client, "default", "Demo::Job", 1)

  // Enqueue into the "default" queue with passing multiple
  // parameters to the Demo::Job.perform so it will fail
  resque.Enqueue(client, "default", "Demo::Job", 1, 2, "woot")
}
```

Simple enough? I hope so.

## Contributing

Just open pull request or ping me directly on e-mail, if you want to discuss some ideas.

