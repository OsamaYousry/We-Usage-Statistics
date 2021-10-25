# We Usage Statistics
A Node JS command line tool/Server that fetches your WE internet usage statistics from the WE website using their backend APIs

## Environment Variables
The password being sent to the backend gets encrypted using AES (Cipher Block Chaining) which requires a key and an IV,
I've extracted the ones being used in the frontend to be used as environment variables being passed to the server,
these variables need to be set for the code to work

- **AES_KEY**: "0f0e0d0c0b0a09080706050403020100"
- **AES_IV**: "000102030405060708090a0b0c0d0e0f"

## Usage
The code could be run directly using Node 14 or if you don't want to worry about the environment just build and run the Dockerfile using the following commands:
````
docker build -t westatistics .
````
Then running for command line:
````
docker run -e AES_KEY=${AES_KEY} -e AES_IV=${AES_IV} --rm westatistics $PHONE_NUMBER $PASSWORD
````
Or running as a server:
````
docker run -e AES_KEY=${AES_KEY} -e AES_IV=${AES_IV} --rm -p ${PORT}:3000 westatistics 
````

### Command Line
To run the code on the command line just run 
`node index.js $PHONE_NUMBER $PASSWORD`

### API
You can call the following API if you don't pass any command line arguments
`/api/statistics?msisdn=${phoneNumber}&password=${password}`

### Response
For the command line or the API, I made a unified response
```javascript
{
total: 250,
used: 20,
remaining: 230,
percentage: 8,
daysLeft: 20,
expiryDate: "1999-01-01",
subscriptionDate: "1998-12-01"
}
```

## Dependencies

The package depends on the following:

- Express for the server
- Axios for the Http requests to the WE Backend
- AES-JS to encrypt the password using the same algorithm in the WE website
- BTOA to convert the binary from the encryption to base64 ASCII to be sent to the API
- Node-cache to cache the tokens for the user

## Tasker task

I mainly wrote this code to use with Tasker to get a daily notification with my internet usage statistics, so I added the file **taskerTask.xml** with the steps for the tasker task. just replace the ${msisdn} and ${password} variables with your values and import it in your Tasker application

## Next Steps

- [x] Reverse engineer the encryption algorithm to be able to send the passwords as plain text