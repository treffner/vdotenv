# V dotenv
People configure their app variables via JSON, YAML, or even gitignored .v files. I personally found env files to work the best, especially with [docker-compose](https://docs.docker.com/compose/environment-variables/#the-env_file-configuration-option).

Further reading:
[12 factor apps](https://12factor.net/config)

# Features

- fully compatible with docker-compose .env
- useful helper function dotenv.get()

# Usage:
```shell
v install treffner.dotenv
```

Create a file called .env in the root folder of your application.
Add it to your .gitignore file. (best practice)
Fill it with key=value pairs.

```shell
POSTGRES_HOST=localhost
POSTGRES_USER=admin
POSTGRES_PASSWORD=postgres_password_goes_here
POSTGRES_DB=admin

JWT_SECRET=jwt_secret_goes_here
```

Then in your v source:
```v
module main

import treffner.dotenv
import os

fn main() 
    // load .env environment file
    dotenv.load()
    // you can use build in os.getenv()
    println(os.getenv('POSTGRES_HOST'))
    // you can also use dotenv.get() if you need fallback handling
    secret := dotenv.get('JWT_SECRET') or {
        'default_dev_token' // default, not found, or simply the same on all environments
    }
    println(secret)
}
```

## Syntax rules
These syntax rules apply to the .env file:

- Dotenv expects each line in an env file to be in VAR=VAL format.
- Lines beginning with # are processed as comments and ignored.
- Blank lines are ignored.
- There is no special handling of quotation marks. This means that they are part of the VAL.
- Environment variables may not contain whitespace.

## Test with docker-compose
```shell
docker-compose run --rm v
println(os.getenv('POSTGRES_HOST'))
```
This should print "localhost".

# Todo/ideas
- dotenv.required() method to let people know what variables are needed

# License
[GPL-3.0](LICENSE)
