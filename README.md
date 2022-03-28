# Introduction

For this exercise we're going to profile the performance of a fictional (and awful!) authentication app, and check other NFRs

# Install node modules

Firstly install the node modules with:
`npm install`

# Look at the app

Start the app with `npm start`.
It's a simple express app, try and test both of the endpoints either in the browser, postman or curl.

# Load testing

Install the [loadtest](https://github.com/alexfernandez/loadtest) package globally with 
`npm install loadtest`

Close the app and restart it, this time with `NODE_ENV=production node --prof index.js`.

Use loadtest to submit many requests to the auth endpoint of your running app. Send 250 requests with a concurrency of 20. You'll also want to use keep-alive connections.

Make sure to make an account before you hit the authentication endpoint.

<details><summary>Hint</summary>
`curl -X GET "http://localhost:3000/newUser?username=josh&password=password"`
`loadtest -k -c 20 -n 250 "http://localhost:3000/auth?username=josh&password=password"`
</details>

You'll see the percentage of requests that were served within a certain time, the longest request is quite long!
Try varying the amount of concurrency or requests and see how that changes the response time.

Since we started the app with the --prof flag enabled, node will have created some log files! They'll be called something like isolate-0xnnnnnnnnnnnn-v8.log.

They're not very readable in this state!
You'll need to process them:
`node --prof-process isolate-0xnnnnnnnnnnnn-v8.log > processed.txt`

What you'll see now will depend on your node version! But you should find that one function in particular is using most of the time, which one is it?

<details><summary>Hint</summary>
You'll want to look in the Bottom up (heavy) profile bit.
</details>
<details><summary>Answer</summary>
It's the pbkdf2.js.
(If you're in an older version of node it might be node::crypto::PBKDF2)
</details>


# Let's speed it up!

Now that we know what function is taking up most of our apps processing time, let's fix it!
Use your knowledge of concurrent programming to speed up our program!

<details><summary>Hint</summary>
The `pbkdf2Sync` function is done synchronously so it will block the thread running it when it's reached, making future requests have to wait.
</details>

<details><summary>Hint</summary>
You'll want to use `pbkdf2` function over `pbkdf2Sync`
</details>

# Security

Run npm audit to see any security concerns with packages you have installed. Take any steps necessary to fix these security concerns!