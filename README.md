# Twilio

> A golang package for the twilio www.twilio.com cloud telephone service API. This package supports initiating calls, sending SMS and generating TwiML.

## Getting Started

1º Install twilio

```bash
$ go get github.com/chrisenytc/twilio
```

## How to Use

Example with Twilio Rest API:

```go
package main

import (
		"fmt"

        "github.com/chrisenytc/twilio/twirest"
)

func main() {
        // Test account Sid/Token
        accountSid := "enter your account sid"
        authToken := "enter your auth token"

        client := twirest.NewClient(accountSid, authToken)

        msg := twirest.SendMessage{
                Text: "Hello monkey",
                To:   "+15005550001",
                From: "+15005550005"}

        resp, err := client.Request(msg)

        if err != nil {
                fmt.Println(err)
                return
        }

        fmt.Println(resp.Message.Status)
}
```

Example with TwiML:

```go
package main

import (
        "net/http"

		"github.com/chrisenytc/twilio/twiml"
)

func helloMonkey(w http.ResponseWriter, r *http.Request) {

        callers := map[string]string{"+15005550001": "Langur"}

        resp := twiml.NewResponse()

        r.ParseForm()
        from := r.Form.Get("From")
        caller, ok := callers[from]

        msg := "Hello monkey"

        if ok {
                msg = "Hello " + caller
        }

        resp.Action(twiml.Say{Text: msg},
                twiml.Play{Url: "http://demo.twilio.com/hellomonkey/monkey.mp3")
        resp.Send(w)
}

func main() {
    http.HandleFunc("/", helloMonkey)
    http.ListenAndServe(":8080", nil)
}
```

## Contributing

Bug reports and pull requests are welcome on GitHub at [https://github.com/chrisenytc/twilio](https://github.com/chrisenytc/twilio). This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.

1. Fork it [chrisenytc/twilio](https://github.com/chrisenytc/twilio/fork)
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am "Add some feature"`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## Support
If you have any problem or suggestion please open an issue [here](https://github.com/chrisenytc/twilio/issues).

## Credits
This is a fork of the project [ckvist/twilio](https://bitbucket.org/ckvist/twilio). Give some respect to him.

## License 

The MIT License

Copyright (c) 2016, Christopher EnyTC

Permission is hereby granted, free of charge, to any person
obtaining a copy of this software and associated documentation
files (the "Software"), to deal in the Software without
restriction, including without limitation the rights to use,
copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the
Software is furnished to do so, subject to the following
conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES
OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY,
WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
OTHER DEALINGS IN THE SOFTWARE.
