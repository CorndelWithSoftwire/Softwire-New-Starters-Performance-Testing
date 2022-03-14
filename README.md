# Install node modules

`npm install`

# Load testing

Install the [loadtest](https://github.com/alexfernandez/loadtest) package globally with 
`npm install loadtest`

(Alternatively, use npx to run loadtest from anywhere.)

# Start the App

`npm run start`

# Do the load test
`curl -X GET "http://localhost:3000/newUser?username=josh&password=password"`
`loadtest -k -c 20 -n 250 "http://localhost:3000/auth?username=josh&password=password"`